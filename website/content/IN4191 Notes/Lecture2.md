+++
date = "2021-01-05"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-05"
title = "Lecture 2: Classical Systems"
slug = "Lecture 2"
type = "notes"
katex = "true"
markup = "mmark"
+++

#### Classical Systems (Chapter 7)

An encryption algoithm or ciper is the transformation of plaintext to ciphertext with a secret key, depicted as

$$c=e_k(m)$$

with $$m$$ being the plaintext,  
$$e$$ being the cipher function,  
$$k$$ being the secret key,  
$$c$$ being the ciphertext

The reverse process is called decryption or decipherment, depicted as

$$m=d_k(c)$$

The key above needs to be known to both parties, but must be kept secret from all others. These encryption schemes are
called _symmetric cryptosystems_.  
Another form of system that uses different types of keys; public key for encryption and private key for decryption, is
known as an _asymmetric cryptosystem_ or _public key cryptosystem_

Historical ciphers can easily be broken with _Cryptanalysis_ based on the properties of the underlying language (english). The frequencies of
most commonly appearing letters (E and T) will be visible in certain ciphers, or the most common bigrams (TH and HE), or
or trigrams (THE and ING).

#### Kerckhoff's Principle

"The method must not be required to be secret, and it must be able to fall into the hands of the enemy without inconvenience."

##### Shift Cipher

One of the simplest ciphers is the shift cipher, which shifts the letters in the alphabet based on some key.
Commonly called the Ceasar cipher, as he used this algorithm with a key of three. For example with 3 as a key, the
letter A would become the letter D. ($$c=m+k$$ mod 26)

Each letter in the alphabet is associated with a number (0-25), which is used to convert plaintext into a sequence of
numbers and then shifted by adding the key to each value modulo 26. Hence, this cipher is a _stream cipher_ calculating
on an incoming stream of values from the converted plaintext.

Breaking the shift cipher with statistical distance given as

$$\Delta[X,Y]=\frac{1}{2}\sum_{u\in V} \displaystyle\left\lvert \underset{X \leftarrow D_1}{Pr} [X=u]-
\underset{Y \leftarrow D_2}{Pr} [Y=u] \right\rvert$$

with random variables $$X$$ and $$Y$$, $$V$$ the support of $$X$$ and $$Y$$ (all values which can occur for $$X$$ and
$$Y$$ with non-zero probability). After doing this calculation with all possible values for key $$k$$ the result will be
26 different distributions. The one with the lowest is the one most likely to be the key.

##### Substitution Cipher

Problem with shift cipher is too small key space, substitution uses a permutation of the alphabet to substitute each letter
with a letter from the permutation using a chosen permutation. Here the key space is equal to the number of possible
permutations $$26!\approx 4.03*10^{26}\approx 2^{88}$$.

Feasibility of brute forcing is given as under $$2^{80}$$ steps.

Substitution cipher can still be broken using statistical distance as with the shift cipher, and the details of the
underlying language.

##### Vigénere Cipher

Problem in the substitution cipher is that each plaintext letter always corresponds to the same ciphertext letter.
A solution is to use a polyalphabetic substitution cipher, similar to the regular substitution cipher but with 2 different
permutations, where even numbers of the plaintext are substituted with one permutation and odd numbers with the other.
This way, different plaintext letters can map to the same ciphertext letter, making it harder to find underlying
information of the language. With 5 different alphabets, the key space becomes

$$(26!)^5\approx2^{441}$$

But it's hard to remember the key, hence there is the vigénere cipher. Where the key is a word, which is continuously repeated.
The same problem arises that once the length of the key is known, the cipher can be broken using the Kasiki test to fist
finding the length of the keyword by looking at the distances of commonly appearing bigrams and trigrams and finding their
greatest common divisor (gcd). Then with the length of the keyword look at every first letter of the key length (ie. with key 5, letters 0,5,10...)
and use statistics to find the plaintext letter.

##### Permutation Cipher

Permuatation ciphers work by having a permuatation $$\sigma \in S_n$$, for example

$$ \sigma =
\begin{pmatrix}
   1 & 2 & 3 & 4 & 5 \\
   2 & 4 & 1 & 3 & 5
\end{pmatrix}
=(1243) \in S_5$$

Taking the plaintext and splitting it into all lowercase 5 letter chunks, one can use the defined permutation to encrypt
the plaintext one chunk at a time. Then spaces can be removed to hide underlying structure of the language.
This can be broken by asking for a permutation of a plaintext of your choosing and deducing the permutation from the
given ciphertext and the chosen plaintext. Sequences will repeat key at modulo $$n$$. For example, asking to encrypt

**abcdefghijklmnopqrstuvwxyz** and obtaining ciphertext

**cadbehfigjmknlorpsqtwuxvyz** gives the resulting permutation of

$$\begin{pmatrix}
   1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & ... \\
   2 & 4 & 1 & 3 & 5 & 7 & 9 & 6 & 8 & 10 & ...
\end{pmatrix}$$

where modulo 5 the permutation starts repeating.
