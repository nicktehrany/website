+++
date = "2021-01-08"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-08"
title = "Lecture 5: Modern Stream Ciphers"
slug = "Lecture 5"
type = "notes"
mathjax = "true"
+++

#### Modern Stream Ciphers (Chapter 12)

Encrypt bits rather than blocks as this is faster and easier to implement in hardware and software. Stream ciphers work by

$$c=m\oplus F_k(0)$$

and are IND-PASS secure if the PRF is secure.

Feedback shift registers are a standard way of producing a binary stream of data. These consist of a number of memory cells, where some of them are tapped that feed a feedback function. Registers are shifted down and the result from the feedback is shifted to the empty cell. Typically linear feedback shift registers (LFSRs) are used as these are easier to implement in hardware, and have a linear feedback function that is an xor of all the bits. Sequences will repeat after a a period $$N$$ such that

$$s_{N+i}=s_i$$

and $N$ can be a maximum of $2^L-1$ (-1 as we do not include all 0 bits)

The _connection polynomial_ specifies the properties of the LFSR and is given in the form of

$$C(X)=1+c_1*X+c_2*X^2+...+c_L*X^L\in\mathbb{F}_2[X]$$

![LFSR](/images/IN4191/LFSR.png)

and the characteristic polynomial is given as

$$G(X)=X^L*C(1/X)$$

#### Primitive Polynomial

- If $C_L=0$ the polynomial is singular (not choosing the last register for connection polynomial).
- If $C_L=1$ the polynomial is non-singular and there is a period (including the last register in the connection polynomial).
  - $C(X)$ is irreducible: and the period is the smallest $N$ such that $C(X)$ divides $1+X^N$
  - $C(X)$ is primitive: Every initial state produces an output sequence that is periodic and of exact period $2^L-1$

If $$N$$ is the period, then the characteristic f(x) is a factor of $1-X^N$. For example a shift register with 3 registers has a maximum sequence of $2^N -1=2^3 -1=7$, then the connection polynomial with maximum sequence is a factor of $1+X^7$ such that one of the factors from which we can build the connection polynomial with $C(X)=X^3+X+1$.

Linear feedback registers are not secure since they are linear, observing the $2L$ outputs with $L$ registers will reveal the connection polynomial since the first $L$ bits reveal the s values (initial values) and the remaining unknowns can be found with

$$s_j=\sum_{i=1}^Lc_i*s_{j-i}(\text{ mod 2})$$

#### Combining LFSRs

Change the use of LFSRs in a non-linear way, that hides the linearity of them, by combining them.

The maximum length of combining registers will be given as 2 to the sum of all the lengths of the different registers used minus 1,

$$2^{\sum_{i=1}^L L_i}-1$$

Some of the most influential LFSR combination implementations are:

- Filter generator: Turns the output stream into a non linear function by applying function $f$ to it.
- Alternating-step generator: Takes three different shift registers of the roughly same length and the first LFSR is the clock for the others, if it is output is on the second LFSR is clocked and the output of the third is repeated. If the value is zero, the third LFSR is clocked and the output of the second is reused. The final output is the xor of the second and third.
- Shrinking generator: with two LFSRs, the output of the first decides the output of the second (if value is one the output is that of second LFSR, if zero it is nothing)
- A5/1 generator: uses three LFSRs and based on the majority bit decides which LFSR to clock.
- Trivium: uses three shift registers with a total 288 bits ($93+84+111$), which are fed into each other with a number of formulas. It is used in practice and has not been broken. The system is clocked 1152 times before the output is used.
