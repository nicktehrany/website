+++
date = "2021-01-13"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-13"
title = "Lecture 9: The RSA Algorithm"
slug = "Lecture 9"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## The RSA Algorithm (Chapter 15)

The RSA algorithm is based on the difficulty of the RSA problem that it is difficult to to find $d$ given a large composite number $N$ and $e$. It works by taking two large secret prime numbers $p$ and $q$ and computing $N=p*q$. Then picking an encryption exponent $e$ that satisfies

$$\text{gcd}(e,(p-1)*(q-1))=1$$

It is common to chose values $e=3,17,65537$. Now the public key will be shared as the pair of $\mathfrak{pC}=(N,e)$. The private key is computed by applying the extended euclidean algorithm on $e$ and $(p-1)(q-1)$ to obtain the decryption exponent $d$ that satisfies

$$e*d=1\text{ mod }(p-1)(q-1)$$

The secret key can be kept as just $\mathfrak{sK}=(d)$ or $\mathfrak{sK}=(d,p,b)$

**Encryption** can then simply be done by

$$c\leftarrow m^e\text{ mod }N$$

and **decryption** is done as

$$m\leftarrow c^d\text{ mod }N$$

![RSA Correctness](/images/IN4191/RSA-Proof.png)

### Security of RSA

Computing $d$ given $e$ and $N$ is no harder than factoring $N$, so if you can factor $N$ you can compute $d$.

The current suggestion for medium term security is to use a modulo of 2048 bits.

RSA **is OW-CPA** secure, but **not IND-CPA** secure since the system is deterministic.

Additionally RSA is malleable due to **homomorphism**, stating that an encryption scheme has the (multiplicative) homomorphic property if given the encryptions of $m_1$ and $m_2$ we can determine the encryption of $m_1*m_2$ without knowing $m_1$ or $m_2$.

$$(m_1*m_2)^e\text{ mod }N=((m_1\text{}^e\text{ mod }N)*(m_2\text{}^e\text{ mod }N)) \text{ mod }N$$

RSA is **not OW-CCA** secure. This is proven by multiplying the challenger's ciphertext by two and having the oracle decrypt it. The oracle checks if the ciphertext is the original ciphertext, which it is not and will decrypt. But since you know the relationship to the original ciphertext, you can just divide the decrypted plaintext by 2 and retrieve the original message.

### Rabin Encryption

Choose $p$ and $q$ such that $p=q=3\text{ mod }4$ and then use private key $\mathfrak{sK}=(p,q)$ and public key $\mathfrak{pK}=(N)$.

Encryption is done by computing $c\leftarrow m^2 \text{ mod } N$ and decryption as

$$m_p=\sqrt{c}\text{ mod }p=c^{(p+1)/4}\text{ mod }p$$

$$m_q=\sqrt{c}\text{ mod }q=c^{(q+1)/4}\text{ mod }q$$

followed by applying the Chinese Remainder Theorem to combine $m_p$ and $m_q$.

**Is OW-CPA** secure based on the factoring problem but the mapping is not injective, meaning that encryption produces one ciphertext but decryption produces 4 possible plaintexts.

**Not OW-CCA** secure and **not IND-CPA** secure.

### The "Naive" RSA Signature

Construct $d$ the same way as was done in RSA before, now senders sign a message by decrypting it and the receiver verifies the signature by encryption and obtains the message

$$\text{Signing: }s\leftarrow m^d\text{ mod }N$$

$$\text{Verification: }m\leftarrow s^e\text{ mod }N$$

#### Checking Validity of Signatures

We need to check the validity of signatures, which means that padding is required. Padding in RSA works with message $m$ is $t$ bits, and $N$ is $k$ bits with $t<k-32$ bits. Pad $m$ with zeros on the left to make it a multiple of 8 bits and add $(k-t)/8$ bytes to the left of $m$ such that

$$m\leftarrow 00||01||FF||FF ...||FF||00||m$$

This way of padding prevents trivial selective forgery.

#### Selective Forgery

If there is a signing oracle and the attacker wants to obtain a signature $s$ of $m$, he generates a random message $m_1\in \mathbb{Z}_N^*$ such that

$$m_2 \leftarrow \frac{m}{m_1}$$

then asking the oracle to sign $m_1$ and $m_2$ and getting the individual signatures to construct the original one

$$s_i=m_i^d\text{ mod } N$$

$$s\leftarrow s_1*s_2\text{ mod }N$$

since

$$s=s_1*s_2\text{ mod }N$$

$$=m_1^d*m_2^d\text{ mod }N$$

$$=(m_1*m_2)^d\text{ mod }N$$

$$=m^d\text{ mod } N$$

Hence, by applying this kind of padding the homomorphism is destroyed.

#### Signing Documents

Divide the message $m$ into blocks, serial numbers and redundancy is needed, but signing data one by one is very inefficient, since RSA decryption is very slow due to the large value of $d$.

To solve it we do not want to sign every block, but hash the message and sign the hash but send the signed hash as a pair with the message, and the receiver will calculate the hash of the message and compare that to the received hash.

![RSA Signing Documents](/images/IN4191/RSA-Sign_Docs.png)

For this system to be secure, the hash function needs to be secure (preimage-, second preimage-, and collision- resistance).

#### More Security of RSA

**Knowing $\phi(N)$ and $N$** one can find $p$ and $q$, since $\phi(N)=(p-1)(q-1)=p*q-p-q+1=N-p-q-1$, and $S=N+1+\phi$, and $S=p+q$, then we have

$$f(X)=(X-p)(X-q)=X^2-S*X+N$$

$$p=\frac{S+\sqrt{S^2-4*N}}{2}$$

$$q=\frac{S-\sqrt{S^2-4*N}}{2}$$

**Using a shared modulus** (using the same $p$ and $q$), the attacker can trivially calculate $d$. In the first case, bob and the attacker are sharing a modulus (and bob is index 2), the attacker can calculate $d$ using $e_2*d_2\equiv 1\text{ mod }\phi(N)$.

In the second case, if the attacker can obtain two different ciphertexts for the same plaintext by both other parties, he can find the keys

![Shared Modulus](/images/IN4191/RSA-Shared-Modulus.png)

**Using a small public exponent**, suppose there are three parties and they all use the same small public exponent, then

![Small exponent](/images/IN4191/RSA-Small_EXP.png)

**Wiener's Attack** If one wants to speedup the performance by choosing a smaller value for the private exponent $d$, this will lead to a large value of the encryption exponent $e$ and we cannot choose too small a value for $d$, otherwise an attacker could find $d$ using exhaustive search. But it turns out that $d$ needs to be at least the size of $\frac{1}{3}*N^{1/4}$, otherwise one could attack using Wiener's attack.

The attack uses continued fractions to give a linear time algorithm to determine the private exponent when it is less than $\frac{1}{3}*N^{1/4}$.

#### Fault Analysis

Fault analysis is the introduction of faults into a system and tricking it into doing some calculation incorrectly, by for example altering the environment, heating or cooling the chip, or by damaging the circuit in some way.
