+++
date = "2021-01-07"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-07"
title = "Lecture 4: Defining Security"
slug = "Lecture 4"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

#### Security Games (Chapter 2.2)

Security games are used to define security for cryptographic components, which contain an adversary and a challenger, and the idea is that the adversary needs to reach a certain objective given data provided by the challenger. These are typically represented visually with the adversary in a box and the challenger outside with the date it provides. The advantage of an adversary $$A$$ is defined as a function bounded by time $$t$$ that the adversary spends to try to solve the problem. For RSA and SQRROOT games this is defined as

$$Adv_v^X(A,t)=Pr[A \text{ wins the game X for }v=log_2N \text{ in time less than }t]$$

Finding the quadratic residue of a value is done by checking all values that it comprises (i.e. with value 7, $\mathbb{Z}_7=\\\{0,1,2,3,4,5,6\\\}$), and squaring each containing value (except 0 of course) modulo the value itself. For the example with $\mathbb{Z}=7$ this will be,

$$\displaylines{1^2\equiv1 \text{ mod }7\\
2^2\equiv4 \text{ mod }7\\
3^2\equiv2 \text{ mod }7\\
4^2\equiv2 \text{ mod }7\\
5^2\equiv4 \text{ mod }7\\
6^2\equiv1 \text{ mod }7}$$

Where the residue class is all remainders, in this case $Q_7=\\\{1,2,4\\\}$.

For the QUADRES game, where the challenger picks a value $$a$$ to be a quadratic residue and the adversary guesses if it is a quadratic residue or not (50/50 chance with random guessing). Hence, the advantage is zero if the adversary just guesses, which makes the function

$$Adv_v^{QUADRES}(A)=2*\lvert Pr[A \text{ wins the QUADRES game for }v=log_2N]-\frac{1}{2}\lvert$$

#### Discrete Logarithms (Chapter 3.1)

The discrete log problem (DLP) states that given a finite group $G$, and $g,h \in G$

$$g^x=h\text{ or }x=dlog_g(h)$$

in other words $a^x=y\text{ mod }p$, given $a$ and $y$ it is difficult to find $x$.

### Defining Security (Chapter 11)

Three aspects of modern cryptography:

- **Definitions:** The first challenge for modern cryptography is to arrive at a concrete mathematical definition of what it means for a particular cryptographic mechanism to be secure.
- **Schemes:** Once it has a security definition, we want to design schemes which it hoped will meet the security definition (i.e. system whose security relies on factoring large numbers).
- **Proofs:** Ask whether the design meets the security definition.

A pseudo random function (PRF) is a function that appears random to the adversary, such that he cannot predict the output.

In the security game, instead of giving the adversary the Function $F$ and value pair (x,y), the adversary is provided with a family of functions $\\{F_k\\} K$ from which one function is chosen with private key $K$ and the adversary has access to an oracle for querying function applications. This is done, as if the adversary is supplied directly with the function, he can directly apply the function and win, and without the oracle the adversary would have no chance. The PRF security game will be,

![PRF_sec_game](/images/IN4191/PRF_sec_game.png)

#### One-Way Functions and Trapdoor One-Way Functions

One way functions (OWF) work by giving the adversary a public function and asking him to invert the function on an element of the challenger's choosing, shown below

![OWF_sec_game](/images/IN4191/OWF_sec_game.png)

Examples of OWFs are the discrete logarithm problem and RSA, but RSA also has an extra value $d$ for efficiently inverting the function, called the _trapdoor_.

#### Public Key Cryptography

Users hold a public and private key pair, where the public key is used to encrypt messages and send them to the user and the private key is the only one that can decrypt the message. The keys are linked in a mathematical way such that obtaining the private key from the public key is difficult but the public from private is easy.

#### Security of Encryption

Different types of attacks:

- **PASS:** Passive attack where the adversary should not learn the message underlying a specific ciphertext. E.g. OW-PASS depicted below, the same applies to public key crypto with public and private key pairs

![ow-pass](/images/IN4191/OW_PASS.png)

- **CPA:** Chosen plaintext attack where the adversary also has access to an oracle for encrypting a chosen plaintext. This is done as with PASS, the adversary has very limited power.
E.g. in symmetric crypto this would be as follows,
   ![OW-CPA](/images/IN4191/OW_CPA.png)
   Note that in the previous game with public crypto, it is also CPA as the adversary has the public key and can encrypt whatever plaintext he chooses.

- **CCA:** Chosen ciphertext attack gives the adversary an oracle to decrypt chosen ciphertexts, except of course decrypting the original ciphertext. This game looks as follows for symmetric and public crypto,
   ![OW-CCA](/images/IN4191/OW-CCA.png)
   As this notion only provides a sense of fully breaking the encryption but not retrieving parts of the message, there are additional security classifications about how secure a message should be, such that the adversary cannot retrieve any information about the plaintext.

- **Perfect Security:** As was seen in earlier chapters, perfect security is when the adversary cannot gain any information about the message, by using a key with the same length as the plaintext. This is not used in practice as it is highly unpractical.
- **Semantic Security:** Similar to perfect security but the run time of the adversary is bounded by a polynomial function of the underlying security parameter (i.e. the key size).
- **IND Security:** As semantic security is difficult to show, indistinguishability (IND) is easier to show and a system that is IND secure is also semantically secure. IND works in two stages,
  - **Find** stage where the adversary produces two plaintexts of the same length
  - **Guess** stage where the adversary is given the encryption of one of the plaintexts ($$m_b$$) and has to guess the bit $$b$$ with probability greater than one half.

![IND-CCA](/images/IN4191/IND-CCA.png)

Any encryption function that is IND-CPA secure must be probabilistic.

Definition: A crypto algorithm is secure if it is semantically secure against a CPA attack.  
Definition: An encryption algorithm is secure if it is IND-CCA secure.  
Theorem: A system that is IND-PASS secure must be semantically secure against passive adversaries.

$$\displaylines{\prod \text{ is IND-CCA} \Longrightarrow\prod \text{ is IND-CPA}\Longrightarrow\prod\text{ is IND-PASS}\\ \prod \text{ is IND-XXX}\Longrightarrow\prod\text{ is OW-XXX}}$$

#### Other Notions of Security

- Many time security: How many times can we use the LR oracle?
- Real or random: RoR oracle encrypts either the real message or a random message.
- Lunchtime attack (CCA1): Adversary has the decryption oracle during the find stage for some time.
- Nonce-based encryption: Deterministic algorithms are not IND-CPA secure, therefore use a nonce (number used once) to provide some randomness.
- Data encapsulation mechanism: Symmetric system but the key is used only once.
- Non-malleability: Based on having the ciphertext of an unknown plaintext, the adversary can compute a new ciphertext for a related plaintext.
- Plaintext aware: It is computationally difficult to construct a valid ciphertext without being given the corresponding plaintext.

![Notions of Security](/images/IN4191/NotionsOfSec.png)

Random Oracle Model (ROM): All parties have a random function with domain {0,1}* and a finite codomain C, the challenger has the control of the oracle, and the adversary can call her. This kind of function cannot exist in the real world, but hash functions act as such an oracle.
