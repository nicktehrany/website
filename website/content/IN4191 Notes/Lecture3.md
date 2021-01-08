+++
date = "2021-01-06"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-06"
title = "Lecture 3: Information Theoretic Security"
slug = "Lecture 3"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

#### Information Theoretic Security (Chapter 9)

A crypto system is said to be _computationally secure_ if the beset possible algorithm for breaking it requires $$N$$ operations,
where $$N$$ is some large number above $2^{128}$. It is impossible to prove that a system is computationally secure, as
we do not know if there is a better algorithm for breaking it, hence a system is called secure if the best _known_ algorithm
requires a large number of computations.

_Provably secure_ systems are reduced to breaking the system by solving some well studied hard problem (i.e. factorizing large
numbers).

A system is said to be _unconditionally secure_ if the adversary has unlimited computational power but still cannot break
 the system.

 #### Probability and Ciphers

 Given $\mathbb{P}$ the space of possible plaintexts, $\mathbb{K}$ the space of possible keys, and $\mathbb{C}$ the space of possible ciphertexts,
the probability that $p(C=c)$ is given as,

$$p(C=c) = \sum_{k:c\in \mathbb{C}(k)}p(K=k)*p((P=d_k(c)))$$

For example, given four messages $\mathbb{P}$={a,b,c,d} with probability of occurring

- p(P=a) = 1/4
- p(P=b) = 3/10
- p(P=c) = 3/20
- p(P=d) = 3/10

and keys $\mathbb{K}$={$k_1,k_2,k_3$} with probabilities

- p($K=k_1$) = 1/4
- p($K=k_2$) = 1/2
- p($K=k_3$) = 1/4

and resulting $\mathbb{C}$={1,2,3,4} with

|       | a   | b   | c   | d   |
| ----- | --- | --- | --- | --- |
| $k_1$ | 3   | 4   | 2   | 1   |
| $k_2$ | 3   | 1   | 4   | 2   |
| $k_3$ | 4   | 3   | 1   | 2   |

then different probabilities can be computed

$$\displaylines{
p(C=1)=p(K=k_1)*p(P=d)+p(K=k_2)*(P=b)+p(K=k_3)*p(P=c)=0.2625\\
p(C=2)=p(K=k_1)*p(P=c)+p(K=k_2)*(P=d)+p(K=k_3)*p(P=d)=0.2625\\
p(C=3)=p(K=k_1)*p(P=a)+p(K=k_2)*(P=a)+p(K=k_3)*p(P=b)=0.2625\\
p(C=4)=p(K=k_1)*p(P=b)+p(K=k_2)*(P=c)+p(K=k_3)*p(P=a)=0.2125}$$

As can be seen all ciphertext are almost uniformly distributed. Based of this we can compute the conditional probability that c is the ciphertext given plaintext m with

$$p(C=c|P=m)=\sum_{k:m=d_k(c)}p(K=k)$$

Which is the sum of probabilities over all of the keys where the input plaintext m gives the resulting ciphertext x. For the example this will be as follows,

$$\displaylines{
\begin{aligned}p(C=1|P=a)= 0, \quad &p(C=2|p=a)=0, \\ p(C=3|P=a)=0.75, \quad &p(C=4|P=a)=0.25\\
p(C=1|P=b)= 0.5, \quad &p(C=2|p=b)=0, \\ p(C=3|P=b)=0.25, \quad &p(C=4|P=b)=0.25\\
p(C=1|P=c)= 0.25, \quad &p(C=2|p=c)=0.25, \\ p(C=3|P=c)=0, \quad &p(C=4|P=c)=0.5\\
p(C=1|P=d)= 0.25, \quad &p(C=2|p=d)=0.75, \\ p(C=3|P=d)=0, \quad &p(C=4|P=d)=0
\end{aligned}}$$

With said probability we can calculate the probability of a plaintext m given ciphertext c,

$$p(P=m|C=c)=\frac{p(P=m)*p(C=c|P=m)}{p(C=c)}$$

#### Perfect Secrecy

A system with the property that the ciphertext does not reveal any information about the plaintext. So,

$$p(P=m|C=c)=p(P=m)$$

Shannon's theorem on perfect secrecy says a crypto system is perfectly secure _iff_

- every key is used with equal probability 1/#$\mathbb{K}$
- for each m $\in\mathbb{P}$ and c $\in \mathbb{C}$ there is a unique key k such that $e_k(m)=c$

#### Vernam Cipher (one-time pad)

Uses a binary key that is as long as the message to be encrypted and each of the plaintext bits is xor with the key bit, and each key is only allowed to be used once.

#### Entropy

Keys as long as messages are impractical in modern crypto systems, therefore systems will not be unconditionally secure but are hoped to be computationally secure.

Given a random variable $X$ which takes on a finite set of values $x_i$ and has probability distribution $p_i=p(X=x_i)$ entropy is given as

$$H(X)=-\sum_{i=1}^np_i*log_2p_i$$

Joint probability is given as

$$H(X,Y)=-\sum_{i=1}^n\sum_{j=1}^jr_{i,j}*log_2r_{i,j}$$

and conditional probability as

$$H(X|Y=y)=-\sum_{x}p(X=x|Y=y)*log_2 p(X=x|Y=y)$$

the conditional probability can then be simplified as follows

$$\displaylines{H(X|Y)=\sum_yp(Y=y)*H(X|Y=y) \\
=-\sum_x\sum_yp(Y=y)*p(X=x|Y=y)*log_2p(X=x|Y=y)}$$

which presents the amount of uncertainty that is left about $$X$$ if $$Y$$ is revealed.

$$H(X,Y)=H(Y)+H(X|Y)$$

The _key equivocation_ property is what uncertainty is left about the key after a ciphertext is revealed, and is stated as

$$H(K,C)=H(K)+H(P)$$

#### Spurious Keys and Unicity Distance

When information about a key is leaked by a ciphertext, a certain ciphertext rules out a subset of the keys, the remaining possible, but incorrect keys, are called _spurious keys_.

The entropy of a natural language is calculated as

$$H_L=\lim_{n\rightarrow\infty}\frac{H(P^n)}{n}$$

The redundancy of a language is given as

$$R_L=1-\frac{H_L}{log_2\#P}$$

and expresses the percentage of the text in the language that can be removed without losing its meaning.

The _unicity distance_ of a cipher is the average number of ciphertexts for which the expected number of spurious keys becomes zero. It presents the average amount needed of a ciphertext before an attacker can determine the key (assume infinite compute power). It is calculated as

$$\displaylines{\overline{s}_n\ge\frac{\#\mathbb{K}}{\#\mathbb{P}^{n*R_L}}-1\\
n_0\approx\frac{log_2\#\mathbb{K}}{R_L*log_2\#\mathbb{P}}}$$
