+++
date = "2021-01-14"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-14"
title = "Lecture 10: Public Key Encryption and Signature Algorithms"
slug = "Lecture 10"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## Public Key Encryption and Signature Algorithms (Chapter 16)

### Passively Secure Encryption Schemes

#### Goldwasser Micali Encryption

Is based on the QUADRES problem, stating that given a composite integer $N$ and an integer $e$, it is hard to test if $a$ is a quadratic residue or not. This is given as checking if a value from the set $J_N$ of values that produce a jacobi symbol of 1 (hence might be quadratic residue) is in the set of real quadratic residues $Q_N$.

Key generation is done with $N=p*q$ two prime numbers and $y\in J_N \text{\\\\} Q_N$ (The set of pseudo-squares is the difference between the two sets). The secret key is given as $\mathfrak{sK}=(p,q)$ and the public key as $\mathfrak{pK}=(N,y)$.

![Goldwasser-Micali](/images/IN4191/Goldwasser-Micali.png)

Encryption is for a single bit $b$ done by choosing a value

$$x\leftarrow \mathbb{Z}_N^*$$

and then computing 

$$c\leftarrow y^b*x^2\text{ mod }N$$

Decryption is done by computing the legendre symbol of the ciphertext $(\frac{c}{p})$. If $b=0$, $c$ is a quadratic residue (+1), if $b=1$, $c$ is non-quadratic residue (-1). This scheme is very inefficient as you can only encrypt binary values one at a time.

It **is IND-CPA** secure as it is probabilistic and not deterministic due to choosing a new value $x$ and having a different ciphertext for the same plaintext every time.

It is **not IND-CCA** secure since the system is homomorphic, meaning an operation in the ciphertext domain corresponds to an operation in the plaintext domain (binary addition, which is xor).

#### ElGamal Encryption

It uses a prime number $p$ such that $p-1$ is divisible by another prime number $q$, and element $g$ is an element in the finite field $p$ with an order $q$.

Key generation is done as secret key is $x\leftarrow [0,...,q-1]$ and public key is $h\leftarrow g^x\text{ mod }p$.

Encryption message $m$ in $G$ is done as generating randomness 

$$k\leftarrow \\\{0,...,q-1\\\}$$

ciphertext

$$c_1\leftarrow g^k$$

$$c_2\leftarrow m*h^k$$

and output ciphertext

$$c\leftarrow (c_1,c_2)\in G*G$$

Decrypting $c=(c_1,c_2)$ is done as

$$\frac{c_2}{c_1\text{}^x}=\frac{m*h^k}{g^{x*k}}=\frac{m*g^{x*k}}{g^{x*k}}=m$$

It **is IND-CPA** secure but **not IND-CCA** secure due to multiplicative homomorphism.

This scheme introduces data expansion as the message with the ciphertext will have two pieces.

#### Carmichael Theorem

Let $n=pq$ where $p$ and $q$ are large numbers, $\phi(n)$ is Euler'a totient function and $\lambda(n)=lcm(p-1,q-1)$. For any

$$w\in \mathbb{Z}_{n^2}^*$$

$$w^\lambda=1\text{ mod }n$$

$$w^{n\lambda}=1\text{ mod }n^2$$

#### Pallier Encryption

Based on the difficulty of factoring large integers. Key generation is done with $n=p*q$ and $g$ is a generator of the group

$$\mathbb{Z}_{N^2}^*$$

with an order of $n$ that is $g^n\equiv 1\text{ mod } n^2$ and $\lambda =lcm(p-1,q-1)$ (least common multiplier), where the secret key is $\lambda$ and the public key is $(g,n)$.

Encryption is done as

$$m\in \mathbb{Z}_N\text{ and }r\in_R\mathbb{Z}_N^*$$

$$E_{pk}(m)=g^mr^n\text{ mod }n^2$$

Decryption is done as

$$D_{sk}(c)=\frac{L(c^\lambda\text{ mod }n^2)}{L(g^\lambda\text{ mod }n^2)}\text{ mod }n$$

where $L(u)=\frac{u-1}{n}$.

It is probabilistic, hence it **is IND-CPA** secure, **not IND-CCA** secure since it is additively homomorphic (multiplication in the ciphertext corresponds to addition in the plaintext domain).

#### RSA-OAEP (Optimized Asymmetric Encryption Padding)

![OAEP](/images/IN4191/OAEP.png)

Function $f$ is any $k$-bit trapdoor one way permutation, $k_0$ and $k_1$ are numbers such that the effort of $2^{k_0}$ and $2^{k_1}$ is impossible ($k_0,k_1>128$ bits). $n=k-k_0-k_1$ and the hash functions

$$G:\\\{0,1\\\}^{k_0}\rightarrow \\\{0,1\\\}^{n+k_1}$$

$$H:\\\{0,1\\\}^{n+k_1}\rightarrow \\\{0,1\\\}^{k_0}$$

with message $m$ of $n$ bits then

$$c\leftarrow E(m)=f(\\\{(m||0^{k_1})\oplus G(R)\\\}||\\\{R\oplus H((m||0^{k_1})\oplus G(R))\\\})=f(A)$$

Decryption is done as

![OAEP Decryption](/images/IN4191/OAEP-Decrypt.png)

It **is IND-CCA** secure and **is IND-CPA** secure.

#### Fujisaki-Okamoto Transform

Using this transform to obtain IND-CCA secure schemes from IND-CPA secure schemes.

$$\text{Original scheme: }E(m,r)$$

$$\text{new scheme: }E'(m,r)=E(m||r,H(m||r))$$

### Hybrid Systems

In practice the KEM/DEM approach is used, meaning that data will be encrypted using a symmetric cipher and the key of the encryption is sent using an asymmetric cipher.

**KEM:** Key encapsulation mechanism (public key component)

**DEM:** Data encapsulation mechanism (private key component)

To encrypt a message $m$ to a user with $(\mathfrak{pk,sk})$, do

$$(k,c_1)\leftarrow Encap_{\mathfrak{pk}}()$$

$$c_2\leftarrow e_k(m)$$

$$c\leftarrow (c_1,c_2)$$

Upon receiving $c$ the recipient performs

$$k\leftarrow Decap_{\mathfrak{sk}}(c_1)$$

$$\text{If } k=\bot \text{ return }\bot$$

$$m\leftarrow d_k(c_2)$$

$$\text{return }m$$

#### RSA-KEM

Let $N$ be RSA modulus, product of two primes $p$ and $q$, and function $f$ is RSA encryption, then encapsulation will be

$$x\leftarrow \\\{1,...,N-1\\\}$$

$$c\leftarrow f_{N,e}(x)$$

$$k\leftarrow H(x)$$

$$\text{Output }(k,c)$$

Decapsulation will be

$$x\leftarrow f_N^{-1}(c)$$

$$k\leftarrow H(x)$$

$$\text{Output }k$$

RSA-KEM **is IND-CCA** secure under ROM (Random Oracle Mode).

#### DHIES-KEM

Diffie-Hellman Integrated Encryption Scheme uses a cyclic finite abelian group $G$ of prime order $q$ for key generation, as well as a key generator $g$, Key space $K$, key derivation function $H$, generate $x$ in $\mathbb{Z}_N$, and compute $h=g^x$.

Encapsulation and decapsulation are then given as

![DHIES-KEM](/images/IN4191/DHIES-KEM.png)

This **is IND-CCA** secure if the hash function is secure.

### Secure Signature Schemes

#### RSA-FDH

RSA-Full Domain Hash is a combination of RSA and hash functions for an efficient and secure signature scheme. It works by taking the hash of the message and signing it with the private key and upon receiving the signature is verified by using the public key and the calculated hash of the message, and comparing the two hashes. This scheme is secure but the codomain of the hash function needs to match the domain of RSA.

#### RSA-PSS

RSA-Probabilistic Signature Scheme has $N$ as a RSA modulo of size $k$ bits with $e$ and $d$, and two parameters $k_0$ and $k_1$ such that $k_0+k_1<k-1$ with the following hash functions

$$G: \\\{0,1\\\}^{k_1}\rightarrow \\\{0,1\\\}^{k-k_1-1}$$

$$H: \\\{0,1\\\}^*\rightarrow \\\{0,1\\\}^{k_1}$$

and

$$G_1: \\\{0,1\\\}^{k_1}\rightarrow \\\{0,1\\\}^{k_0}$$

$$G_2: \\\{0,1\\\}^{k_1}\rightarrow \\\{0,1\\\}^{k-k_0-1}$$

$$G(w)=g_1(w)||G_2(w)$$

![RSA-PSS](/images/IN4191/RSA-PSS.png)

#### DSA

Digital Signature Algorithm exists because

- RSA based schemes are costly in terms of signature generation
- RSA based signatures are large
- RSA might be broken soon

Digital signature algorithm is based on finite fields or elliptic curves.

It uses a large prime $p$ such that $p-1$ is divisible by another prime $q$, a generator $g$ of the finite field in mod $p$ with an order $q$, and a hash function that maps bit strings to $\mathbb{Z}_p$.

The secret key will be an integer $x$ in $[0,...,q-1]$ and the public key as $h=g^x$.

![DSA](/images/IN4191/DSA.png)

#### EC-DSA

Elliptic curve version of DSA, choose a random integer $a$ and a point $P$, where $a$ is the secret key, and compute the public key $Q=aP$

![EC-DSA](/images/IN4191/EC-DSA.png)

Smaller key size due to elliptic curves and no exponentiations but point operations on the elliptic curve.

#### Schorr Signatures

$G$ is a public abelian group with generator $g$ of prime order $q$ with private key $x$ in $[0,...,q-1]$ and public key $h=g^x$.

Signing a message is done as

$$k\leftarrow \mathbb{Z}_q$$

$$r\leftarrow g^k$$

$$e\leftarrow H(m||r)$$

$$s\leftarrow k+x*e\text{ mod }q$$

Verification is done as

$$r\leftarrow g^s*h^{-e}$$

The signature is only valid if and only if $e=H(m||r)$.

#### Nyberg-Rueppel Signature

If the message is small we can verify the signature and extract the message from it. This is a log based scheme with message recovery, but it requires redundancy (e.g. doubling the message by concatenating it twice $m||m$) which is easy to revert.

$G$ is a public abelian group with generator $g$ of prime order $q$ and private key $x=[0,...,q-1]$ and public key $h=g^x$.

![Nyber-Ruppel](/images/IN4191/Nyberg-Ruppel.png)
