+++
date = "2021-01-11"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-11"
title = "Lecture 7: Hash Functions, MAC, and Key Derivation Functions"
slug = "Lecture 7"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## Hash Functions, MAC, and Key Derivation Functions (Chapter 14)

Hash functions are used for integrity protection (checksum, file system integrity), one way encryption (password protection), asymmetric crypto schemes, MACs, key derivations, pseudo random number generators, and many more applications.

Hash functions receive an arbitrary length bit string and output a fixed length string called the hash value, digest, fingerprint, or hashcode.
Hash functions are one way: easy to compute $y$ given $x$, where $y: H: \\{0-1\\}*\rightarrow\\{0-1\\}^n$

$$H(x)=y$$

and $x$ is called the preimage.

Need to have three properties:

1. **Preimage Resistance:** given a hash value $y$, it should be infeasible to find $x$.
2. **Second Preimage Resistance:** given a preimage $x$, it should be hard to find an $x'$ with the same hash such that $y=y'$. This is done by making the output space large enough (i.e. $2^{128}$), since $2^n$ attempts are required.
3. **Collision Resistance:** given two preimages $x$ and $x'$ it should be infeasible to compute the same hash $y$. The difference to second preimage resistance is that here we are not looking for a single pair of preimages that give the same hash, but any pair (remember all your previous hash calculations).

As there is guaranteed to exist an adversary that can break collision resistance, since we know a larger input domain will have to map multiple inputs to the same output if the output domain is fixed (pigenhole principle applies), but given human ignorance we can assume hash functions to be collision resistant. Human ignorance states that in the same domain the collision will likely be a meaningless preimage. For example trying to find the collision of a hashed contract will just be random bits.

Definition: A functionHis said to be collision resistant (by human ignorance) or HI-CR secure if it is believed to be infeasible to write down a collision for the function, i.e. two elements in the domain mapping to the same element in the codomain.

Terminology:

- **one way:** preimage + second preimage resistant
  - sometimes only preimage resistant
- **weak collision resistant:** second preimage resistant
- **strong collision resistant:** collision resistant
- **OWHF (One way hash function):** preimage and second preimage resistant
- **CRHF (collision resistant hash function):** second preimage and collision resistant

### Padding

The input to a compression function needs to a multiple of the block size, which requires to add padding bits such that

$$m||pad_i(|m|,b)$$

5 main padding methods of which method 0 is not secure.

1. **Method 0:** Let $v$ denote $b−|m|(\text{ mod } b)$. Add $v$ zeros to the end of the message $|m|$, i.e. $m‖pad_0(|m|,b)=m‖0∗$.
2. **Method 1:** Let $v$ denote $b−(|m|+1) (\text{ mod }b)$. Append a single 1 bit to the message, and then pad with $v$ zeros, i.e. $m‖pad_1(|m|,b)=m‖10∗$.
3. **Method 2:** Let $v$ denote $b−(|m|+ 65) (\text{ mod }b)$. Encode $|m|$ as a 64-bit integer $l$. Append a single 1 bit to the message, and then pad with $v$ zeros, and then append the 64-bit integer $l$, i.e. $m‖pad_2(|m|,b)=m‖10∗‖l$.
4. **Method 3:** Let $v$ denote $b−(|m|+ 64) (\text{ mod } b)$. Encode $|m|$ as a 64-bit integer $l$.Pad with $v$ zeros, and then append the 64-bit integer $l$, i.e. $m‖pad_3(|m|,b)=m‖0∗‖l$.
5. **Method 4:** Let $v$ denote $b−(|m|+2) (\text{ mod }b)$. Append a single 1 bit to the message, and then pad with $v$ zeros, and then add a one-bit, i.e. $m‖pad_4(|m|,b)=m‖10∗1$.

### Merkle-Damgard Construction

**Compression function:** A hash function that receives a fixed length input.

![MD](/images/IN4191/MD.png)

Its construction is based on a family of compression functions $f_k$ that map $(l+n)$ bit inputs to $n$ bit outputs.

![MD Algorithm](/images/IN4191/MD_Alg.png)

Conventionally MD algorithms use padding method 2.

### MD-4 Family

The main algorithms in the MD-4 family, which are based on the MD construction are:

- **MD-4:** The function $f$ has 3 rounds of 16 steps and an output bit length of 128 bits.
- **MD-5:** The function $f$ has 4 rounds of 16 steps and an output bit length of 128 bits.
- **SHA-1:** The function $f$ has 4 rounds of 20 steps and an output bit length of 160 bits.
- **RIPEMD-160:** The function $f$ has 5 rounds of 16 steps and an output bit length of 160bits.
- **SHA-2:**
  - **SHA-256:** The function $f$ has 64 rounds of single steps and an output bit length of 256bits.
  - **SHA-384:** The function $f$ is identical to SHA-512 except the output is truncated to 384bits, and the initial chaining valueHis different.
  - **SHA-512:** The function $f$ has 80 rounds of single steps and an output bit length of 512bits.

Due to birthdaying, the collision space to find a collision in MD-4 becomes $2^{1/2n}$

#### Birthdaying Paradox

Given a set of $t (\ge 10)$ elements, take a sample of size $k$ (drawn with repetition) in oder to get probability $\ge 1/2$ on a collision (i.e. an element drawn at least twice). $k$ has to be $\ge 1.2 \sqrt{t}$

Therefore if $F:A\rightarrow B$ (function $F$ is a mapping from $A$ to $B$) is a random function and $\text{sizeof }A>>\text{sizeof }B$ (size of $A$ is huge compared to $B$) then one can expect a collision after about $\sqrt{(\text{sizeof }B)}$ random function calls.

### HMAC

Have a keyed hash from an unkeyed hash function. Trivially this can be done by concatenating the message with the key and then hashing all of it

$$t=H(k||m||pad_i,(|k|+|m|,n))$$

but this is not safe as the adversary can construct new valid hashes based on the previous hashes, without knowing the key.

Used a nested MAC (**NMAC**), that is built from two keyed hashes

$$\text{NMAC}_{k1,k2}(m)=F_{k_1}(G_{k_2}(m))$$

$$F_{k_1}(x)=f((x||pad_2(l+|x|,l))||k_1)=\text{MD}[f,k_1]*(x)$$

$$G_{k_2}(m)=\text{MD}[f,k_2]*(m)$$

then we have

$$\text{NMAC}_{k_1,k_2}(m)=\text{MD}[f,k_1]*(\text{MD}[f,k_2]*(m))$$

We can then construct the function HMAC using NMAC as follows

$$\displaylines{\text{HMAC}_k(m)=H((k\oplus opad)||H((k\oplus ipad)||m))\\
=\text{MD}[f,IV]((k\oplus opad)||\text{MD}[f,IV]((k\oplus ipad)||m))}$$

with $opad$ and $ipad$ being fixed $l$ bit padding values.

#### MAC from a Block Cipher

We can use a cipher block chaining mode on MAC in the following way

![CBC MAC](/images/IN4191/CBC-MAC.png)

$$\text{EMAC}_{k_1,k_2}(m)=e_{k_2}(CBC-MAC_{k_1}(m))$$

### Sponge Construction

The construction absorbs the message and when output is required it is squeezed from the construction. The permutation function is a fixed function (not one way).

![Sponge Construction](/images/IN4191/Sponge.png)

Using this sponge construction **IND-CCA secure** encryption and decryption can also be done, or sponge based MACs and sponge based stream ciphers.

![IND-CCA Secure Sponge](/images/IN4191/IND-CCA-Sponge.png)
