+++
date = "2021-01-07"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-07"
title = "Lecture 4: Defining Security"
slug = "Lecture 4"
type = "notes"
katex = "true"
markup = "mmark"
+++

### Security Games (Chapter 2.2)

Security games are used to define security for cryptographic components, which contain an adversary and a challenger, and the idea is that the adversary needs to reach a certain objective given data provided by the challenger. These are typically represented visually with the adversary in a box and the challenger outside with the date it provides. The advantage of an adversary $$A$$ is defined as a function bounded by time $$t$$ that the adversary spends to try to solve the problem. For RSA and SQRROOT games this is defined as

$$Adv_v^X(A,t)=Pr[A \text{ wins the game X for }v=log_2N \text{ in time less than }t]$$

The quadratic residue of a value check all values that it comprises (i.e. with value 7, $$ℤ_7={0,1,2,3,4,5,6}$$), and square each containing value (except 0 of course) module the value itself. For the example with ℤ=7 this will be,

$$1^2\equiv1 \text{ mod }7\\
2^2\equiv4 \text{ mod }7\\
3^2\equiv2 \text{ mod }7\\
4^2\equiv2 \text{ mod }7\\
5^2\equiv4 \text{ mod }7\\
6^2\equiv1 \text{ mod }7$$

Where the residue class is all remainders, in this case $$Q_7={1,2,4}$$. 

For the QUADRES game, where the challenger picks a value $$a$$ to be a quadratic residue and the adversary guesses if it is a quadratic residue or not (50/50 chance with random guessing). Hence, the advantage is zero if the adversary just guesses, which makes the function

$$Adv_v^{QUADRES}(A)=2*\lvert Pr[A \text{ wins the QUADRES game for }v=log_2N]-\frac{1}{2}\lvert$$

### Discrete Logarithms (Chapter 3.1)

The discrete log problem (DLP) states that given a finite group $$G$$, and g,h$$\in G$$

$$g^x=h\\
\text{ or }x=dlog_g(h)$$

### Defining Security (Chapter 11)

Three aspects of modern cryptography:

- **Definitions:** The first challenge for modern cryptography is to arrive at a concrete mathematical definition of what it means for a particular cryptographic mechanism to be secure.
- **Schemes:** Once it has a security definition, we want to design schemes which it hoped will meet the security definition (i.e. system whose security relies on factoring large numbers).
- **Proofs:** Ask whether the design meets the security definition.

A pseudo random function (PRF) is a function that appears random to the adversary, such that he cannot predict the output. 

In the security game, instead of giving the adversary the Function $$F$$ and value pair Ix,y), the adversary is provided with a family of functions $${F_K}K$$ from which one function is chosen with private key $$K$$ and the adversary has access to an oracle for querying function applications. This is done, as if the adversary is supplied directly with the function, he can directly apply the function and win, and without the oracle the adversary would have no chance. The PRF security game will be,

![PRF_sec_game](/images/IN4191/PRF_sec_game.png)

#### One-Way Functions and Trapdoor One-Way Functions

One way functions (OWF) work by giving the adversary a public function and asking him to invert the function on an element of the challenger's choosing, shown below

![OWF_sec_game](/images/IN4191/OWF_sec_game.png)

Examples of OWFs are the discrete logarithm problem and RSA, but RSA also has an extra value $$d$$ for efficiently inverting the function, called the _trapdoor_.

#### Public Key Cryptography

Users hold a public and private key pair, where the public key is used to encrypt messages and send them to the user and the private key is the only one that can decrypt the message. The keys are linked in a mathematical way such that obtaining the private key from the public key is difficult but the public from private is easy.

#### Security of Encryption

