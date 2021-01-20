+++
date = "2021-01-10"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-10"
title = "Lecture 6: Block Ciphers and Modes of Operation"
slug = "Lecture 6"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

#### Block Ciphers and Modes of Operation (Chapter 13)

Block ciphers operate on blocks of plaintext at a time to produce blocks of ciphertext. Block sizes tend to be large, 64 bits in DES and 128 bits in other modern block ciphers. In order to limit the advantage of the adversary, the key space is kept very large such that $Adv_{\{F_k\}K}^{PRP}(A)\approx 1/|K|$.
Block cipher is part of an encryption but requires a mode of operation, these are both developed independently.

DES (Data Encryption Standard) was an _iterated_ block cipher which uses a _round function_ repeatedly. DES was deemed unsafe in the 90s but other variants (3DES, shown below) are still being used. Another such cipher is AES (Advanced Encryption Standard). The round function takes a block and returns a block of the same size, and the number of rounds can be fixed or varied. Each use of the round function uses a round key derived from the main secret key using an algorithm called _key scheduling_. The round function should be invertible for decryption and using round keys in reverse order.

![3DES](/images/IN4191/3DES.png)

In DES the round is invertible but not the round function.

In AES both the round and the round function are invertible.

Techniques (+Luck) for breaking block ciphers:

- **Differential Cryptanalysis:** looking at the ciphertext pairs where the plaintexts have certain differences. For breaking DES it requires $2^{37}$ in time and $2^{47}$ in memory.
- **Linear Cryptanalysis:** approximate the behavior of non-linear components of the block cipher with linear functions. For breaking DES it requires $2^{43}$ plaintexts.

Shannon's confusion-diffusion paradigm

- **Confusion:** Split the block into smaller blocks and apply a permutation on each block
- **Diffusion:** Mix permutations so that local change can effect the whole block

**Substitution-permutation networks** are a direct implementation of Shannon's paradigm by mixing keys, substituting, and permutation.

_Avalanche effect_ states that a single change in the plaintext must affect every bit of the output, to not find correlations between input bits and output bits. In order to have avalanche effect there need to be at least 7 rounds.

#### Feistel Cipher

Round functions are invertible regardless of the choice of the function and works by switching right and left block and applying a function on the right with the key and xor it the left block for a new right block. The functions for encryption are

$$\displaylines{L_i\leftarrow R_{i-1},\\R_i\leftarrow L_{i-1}\oplus F(K_i,R_{i-1})}$$

and decryption is done as

$$\displaylines{R_{i-1}\leftarrow L_i,\\L_{i-1}\leftarrow R_i\oplus F(K_i,L_i)}$$

and the operations visually

![Feistel Cipher](/images/IN4191/Feistel.png)

Security of Feistel ciphers depends on:

- the round keys (are they random looking, secure enough)
- the number of rounds
- security of function $F$

#### DES (Data Encryption Standard)

A Feistel cipher with **16 rounds**, **64 bits block size**, **56 bit key length**, and **16 round keys, each 48 bits**. It performs an initial permutation (IP), then runs the Feistel cipher, and then perform a reverse permutation ($IP^{-1}$).

![DES](/images/IN4191/DES.png)

The 3DES version uses three different keys and runs DES 3 times.

The stages of the $F$ function are:

1. **Expansion permutation** where the right half of the 32 bits is expanded and permuted to 48 bits.
2. **Round key addition** where the 48 bit input is xored with the round key
3. **Splitting** the resulting 48 bits are split into eight lots of 6 bit values.
4. **S-Boxes** each 6 bit value goes into an s-box and produces a 4 bit output. S-boxes present the non-linear component, as they are lookup tables that are 4x16 in size.
5. **P-Box** the eight 4 bit lots are combined into 32 bits and are permuted.

DES key scheduling takes the 56 bit key (it's 64 bits but 8 bits are parity bits for error detection at every 8th bit, so 8 16 ... 64) and permutes the bits with the PC-1 permutation. The output of the permutation is divided into two 28 bit halfs $C_0$ the left half and $D_0$ the right. Each round we compute

$$C_i\leftarrow C_{i-1}\lll p_i,$$

$$D_i\leftarrow D_{i-1}\lll p_i$$

where $x \lll p_i$ means performing a cyclic shift on $x$ to the left by $p_i$ positions. If the round number is 1,2,9,16 we shift left by one position, otherwise by two. The two halfs $C_0$ and $D_0$ are joined back together and are permuted again with the PC-2 permutation. Visually depicted below

![DES Key Scheduling](/images/IN4191/DES_key_sched.png)

Decryption is identical only keys will be used in reverse order.

#### AES (Advanced Encryption Standard)

AES is not a Feistel cipher but still has a round function. It is built on the mathematical foundation of finite fields (Galois fields). Galois fields are represented as $GF(p^m)$ with $p$ a prime number and $m$ an integer. Fields with value $m=1$ are prime fields and fields with $m\ge2$ are extension fields. Extension fields work with polynomials (have operations of addition, subtraction, multiplication, and non-negative exponentiation). Operations in the fields are in mod $p$ such that

$$a+b\equiv c \text{ mod } p$$

and

$$a*b\equiv c \text{ mod }p$$

and inverses are given as

$$a*a^{-1}\equiv 1 \text{ mod }p$$

where all variables come from the range of values in the set (e.g. $\mathbb{Z}_7=\\{0,1,2,3,4,5,6\\}$). Extension fields are represented in $m-1$ degree, for example with $GF(2^3)=GF(8)$ and is represented as

$$A(x)=a_2x^2+a_1x^1+a_0$$

shown with three different values for the coefficients (that can only be 0 or 1) as $(a_2,a_1,a_0)$. Then constructing all possible values for the coefficients we get the values for the field

$$GF(8)=\\{0,1,x,x+1,x^2,x^2+1,x^2+x,x^2+x+1\\}$$

Operations are still in mod $p$, such that for example in $GF(8)$ will be

$$(x^2+x)+(x^2+x+1)=((1+1)x^2+(1+1)x+1)\text{ mod }2=1$$

Operations subtraction and addition in the galois field of 2 $GF(2)$ are the same and are the same as doing an xor of the coefficients. For mutiplication with fields of degree larger than 2, to reduce the function to the original form

$$GF(x)=a_nx^n+a_{n-1}x^{n-1}+...+a_0$$

using an irreducible polynomial of degree $m-1$ with $m$ the degree of the multiplication result.

AES arithmetic is performed in modulo irreducible polynomial

$$m(x)=x^8+x^4+x^3+x+1$$

with 32-bit words with polynomial $F_{2^8}[X]$ of degree less than 4, such that

$$\displaylines{a_0||a_1||a_2||a_3\\a_3*x^3+a_2*x^2+a_1*x^1+a_0}$$

note the order of the coefficients is reversed to what they appear (using big-endian). Arithmetic on polynomials in $F_{2^8}[X]$ are modulo reducible polynomial $m(x)=x^4+1$, hence this is a ring rather than a field.

AES supports 128, 192, 256 bit blocks with respective keys 128, 192, and 256 bits. Block size 128 has 10 rounds, 192 has 12 rounds, and 256 has 14 rounds (add 2 per increase).

AES operates on a 4x4 matrix called the _state matrix_, keys are also held in a 4x4 matrix.

AES Operations:

- **SubBytes:** two types of S-boxes exist in AES, one for encryption and one for decryption (one being the inverse of the other). Each byte of the state matrix is considered as an input into the S-Box.
- **ShiftRows:** performs a cyclic shift on the state matrix, where each row is shifted by a different offset.
- **MixColumns:** each column in the state matrix is taken in turn and a new colum is produced by applying it to the polynomial
   ![AES MixColumns](/images/IN4191/AES_MixColumns.png)

- **AddRoundKey:** the state matrix is xored with ($\oplus$) with the round key.

AES can then be described with the following algorithms for encryption and decryption respectively

![AES Encrypt](/images/IN4191/AES_encrypt.png)

![AES Decrypt](/images/IN4191/AES_decrypt.png)

The round keys are computed from the main key, where each round key is a 32 bit word,  with substitution and permutation as follows

![AES Key Schedule](/images/IN4191/AES_KeySched.png)

#### Modes Of Operation

Block ciphers are needed but as is they cannot be used in practice because of missing security notions (IND-CPA and IND-CCA).

There are many different operations but these are the 5 mainly used ones:

- **Electronic Code Book Mode (ECB):** simplest operation that divides the message into blocks and encrypts them individually.
   ![ECB Operations](/images/IN4191/ECB.png)
   The problems with this mode are that the same input block will result in the same output block (repetitions in english text), deletion is possible without detection, and replay attack is possible (insert blocks from other messages). This means it is **not IND-PASS** secure and **not OW-CCA** secure but **is OW-CPA** secure.

- **Cipher Block Chaining (CBC):** encrypt the message by xoring the ciphertext after encryption with the ciphertext of the previous message (IV for the first message).
   ![CBC Operations](/images/IN4191/CBC.png)
   The problem with this mode is that it is sequential and cannot be parallelized, and single bit errors cause the whole block to be decrypted wrong and a single wrong bit in the next block. If IV is used once and never repeated it **is IND-PASS** secure, if IV is fixed it is deterministic and hence only **IND-PASS** secure and **not CPA** secure. A truly random IV **is IND-CPA** secure.

- **Output Feedback Mode (OFB):** a block cipher that can be used as stream cipher.
   ![OFB Operations](/images/IN4191/OFB.png)
   with a fixed IV it is **not IND-CPA** secure and **not OW-CPA** secure. With a nonce it **is IND-CPA** secure.

- **Cipher Feedback Mode (CFB):** also a block cipher that can be used a stream cipher, but here the keystream output is generated by encrypting the ciphertext.
   ![CFB Operations](/images/IN4191/CFB.png)
   with a nonce it is **not IND-CPA** secure but **is OW-CPA** secure.

- **Counter Mode (CTR):** the IV is increased for every block, therefore insertion/deletion is not possible as all following decryptions will be invalid.
   ![CTR Operation](/images/IN4191/CTR.png)
   this **is IND-CPA** secure and it allows for parallel computations.

Overall security for the different modes:

![Modes of Operations OW Security](/images/IN4191/MO_OW_Security.png)

![Modes of Operation IND Security](/images/IN4191/MO_IND_Security.png)

#### Obtaining Chosen Ciphertext Security

All previous modes of operations are not CCA secure (the decryption oracle would decrypt any old message), in order to achieve this security we need to use authentication encryption modes.

- **Encrypt then MAC:** append a message authentication code to the ciphertext. This is done by
   ![MAC](/images/IN4191/MAC.png)
   and the message authentication code is applied to the ciphertext. This **is IND-CCA secure**.

- **Encrypt and MAC:** works in a similar way except the encryption is done on the ciphertext (not the plaintext). This is **not IND-CPA** secure.

- **MAC then Encrypt:** MAC the plaintext and then encrypt the MAC and the plaintext together.  With a deterministic MAC it is **not IND-CPA** secure.
