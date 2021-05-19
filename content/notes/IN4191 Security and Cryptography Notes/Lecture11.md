+++
date = "2021-01-15"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-15"
title = "Lecture 11: Certificates, Key Transport, and Key Agreement"
slug = "Lecture 11"
type = "notes"
mathjax = "true"
+++

## Certificates, Key Transport, and Key Agreement (Chapter 18)

What is key management?

- Key generation
- Key distribution
- Key storage
- Key change
- Key usage
- Key destruction

Requirements of key generation:

- secret
- unpredictable
- strong key

It is often desirable to frequently change the key in a cryptographic system.

Types of keys:

- **Static (or Long Term) Keys:** few hours to a few years
- **Ephemeral or Session (or Short Term) Keys** a few seconds or a day

### Certificates and Certificate Authority

We need a binding, linking a public key to an entity. This is achieved for physical tokens such as smart cards with biometrics or PINs, and otherwise we can use digital certificates.

This requires a trusted third party (TTP) or a certificate authority (CA).

The system of CAs and certificates is called the Public Key Infrastructure (PKI).

#### Implicit Certificates

Typical certificates are large which can be undesirable in small systems and therefore we can have smaller certificates that bind the public key and the data in the form of $X|Y$ where $X$ is the data being bound to the public key and $Y$ is the implicit certificate on $X$.

#### Fresh Ephemeral Symmetric Keys form Static Symmetric Keys

Derive short term keys form long term keys using symmetric systems.
Definitions:

- Parties: A, B, and TTP
- Shared Keys: $K_{ab}$,$K_{bs}$, $K_{as}$
- Nonces: numbers used only once, unique but not necessarily random
- Timestamps: $T_a$, $T_b$

$$A\rightarrow B: \color{blue}{M,A,B,}\\\{\color{red}{N_a,M,A,B}\\\}\color{red}{K_{as}}$$

**Wide-Mouth Frog Protocol**

![Wide-Mouth Frog Protocol](/images/IN4191/Wide-Mouth-Frog.png)

Problem of replay attacks if clocks are not synchronized.

**Needham-Schroeder Protocol**

![Needham-Schroeder Protocol](/images/IN4191/Needham_schroeder.png)

**Kerberos**

Authentication system is based on symmetric encryption, and it uses timestamps with synchronized clocks (a requirement that clocks are synced).

![Kerberos](/images/IN4191/Kerberos.png)

#### Fresh Ephemeral Symmetric Keys from Static Public Keys

Problem that TTP based solution assume that there is a shared long term key, solving this can be done in two ways, key transport based on public key cryptography, or kry agreement that outputs a symmetric key.

Alice sends the symmetric key to Bob by encrypting it with Bob's public key.

![Key Transport](/images/IN4191/key-transport.png)

**Forward secrecy:** a compromise on a key should not lead to security problems on the previous messages decrypted before that time.

#### Diffie-Hellman Key Exchange Protocol

It enables two entities to establish a symmetric key even though they have never met before, based on the discrete log problem, and works with a finite field version on abelian group $G$ of order $q$, and an elliptic curve version.

![Diffie-Hellman Key Exchange](/images/IN4191/Diffie-Hellman-Key-Exchange.png)

Has the problem of man in the middle attacks.

![Signed Diffie-Hellman Key Exchange](/images/IN4191/Signed-Diffie-Hellman.png)

Signed is still not secure as the attacker can change the signature. The randomness of $g^a$ or $g^b$ is not linked with the ID of the sender.

#### Station-to-Station (STS) Protocol

![STS](/images/IN4191/STS.png)

#### Blake-Wilson-Menezes Protocol

Achieving authentication without signatures and MACs. It assumes that Alice and Bob have long term keys $(g^a,a)$ and $(g^b,b)$ respectively, and they obtain each others public key via certificates, then

$$A\rightarrow B: \mathfrak{ek}_A=g^x$$

$$B\rightarrow A: \mathfrak{ek}_B=g^y$$

Then Alice computes

$$k\leftarrow H(\mathfrak{pk}_B^x,\mathfrak{ek}_B^a)=H(g^{b*x},g^{y*a})$$

Bob computes

$$k\leftarrow H(\mathfrak{ek}_A^b,\mathfrak{pk}_A^y)=H(g^{x*b},g^{a*y})$$
