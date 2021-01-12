+++
date = "2021-01-12"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-12"
title = "Lecture 8: Number Theory and Elliptic Curves"
slug = "Lecture 8"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## Number Theory and Elliptic Curves (Chapter 1 & 4)

### Modular Arithmetic

A positive integer $N$ called modulus is written as

$$a=b\text{ mod }N$$

if $N$ divides $b-a$, $a$ and $b$ are congruent modulo $N$.

The set of values produced by postfix operation mod $N$ is $\mathbb{Z}_N=\{0,1,...,N-1\}$ (can also be written as $\mathbb{Z}/N\mathbb{Z}$).

The properties of modulo arithmetic are

![Modulo Arithmetic properties](/images/IN4191/Mod-properties.png)

A group is a set which is closed, has an identity, is associative, and every element has an inverse. A group that is commutative is called **abelian**.

Properties 1,2,3,4 are **Groups**.
Properties 1,2,3,4,5 are **Abelian Groups**.

Group types:

- **multiplicative group** group operation is multiplication to create the group
  - $(G,x):f=g\*h \text{ and } g^5=g\*g\*g\*g\*g$
- **additive group** group operation is addition
  - $f=g+h \text{ and } 5*g=g+g+g+g+g$
- **cyclic abelian group** if there is a special element (generator) from which every other element in the group can be obtained by applying the group operation.

Rings:

A ring has two operations (+,\*) with properties 1 to 9, and is denotes a s(R,+,\*). If multiplication is commutative (property 10), then the ring is commutative.

#### Euler's Totient Function

For equations $a*x=b\text{ mod }N$ finding the solution,

$$7*x=3\text{ mod } 143\text{ has one solution}$$
$$11*x=3\text{ mod } 143\text{ has no solution}$$
$$7*x=22\text{ mod } 143\text{ has 11 solution}$$

finding the number of solutions is done by computing the gcd of $a$ and $N$ and has the cases:

1. if $\text{gcd}(a,N)=1$, there is exactly one solution
2. if $\text{gcd}(a,N)=g\ne1 \text{ and gcd}(a,N)\text{ divides }b, \text{ there are }g \text{ solutions}$
3. otherwise there is no solution

The case where the $\text{gcd}(a,N)=1$ $a$ and $N$ are called relatively prime or coprime.

The number of integers in $\mathbb{Z}_N$ is given by Euler's Totient function

$$\phi(N)=\prod_{i=1}^np_i^{e_i-1}(p_i-1)$$

where

$$N=\prod_{i=1}^np_i^{e_i}$$

With this the totient function will tell us how many coprimes there are in the set of $\mathbb{Z}_N$ and we can then calculate all the coprimes in the set as $\mathbb{Z}_N^*=\{1\le a < n | \text{ gcd}(a,n)=1\}$

If $p$ is a prime, then $\phi(p)=p-1$.  
If $p$ and $q$ are both prime, then $\phi(p*q)=(p-1)(q-1)$.

#### Multiplicative Inverse Modulo

Finding a $c$ such that $a*c=c*a=1\text{ mod } N$, such a $c$ is called the multiplicative inverse modulo $N$ of $a$: $a^{-1}$. Such a $c$ only exists when $a$ and the module $N$ are coprime.

If $p$ is a prime, then all non-zero elements have a multiplicative inverse in $\mathbb{Z}_N$, thus $a*x=b \text{ mod } b$ has a unique solution. A ring with all non-zero elements having a multiplicative inverse is called a field.

A field is a set with two operations $(G,*,+)$ such that

- $(G,+)$ is an abelian group
- $(G/0,x)$ is an abelian group
- $(G,*,+)$ satisfies the distributive law

The size of $\mathbb{Z}_N$ is given by $\phi(N)$ and if $N$ is a prime, $\mathbb{Z}_N^*=\{1,...,p-1\}$

**Lagrange's theorem** states that if $(G,*)$ is a group of order (size) $n=\text{ sizeof }G$ (order is calculated with the totient function), then for all $a$ in $G$ we have $a^n=1$ (every single item in $\mathbb{Z}_N$ to the power of the order).

**Fermat's Little theorem** states that if $p$ is a prime and $a$ is in $Z$, them $a^p=a\text{ mod } p$.

#### Basic Algorithms

**Euclidean Algorithm** is for computing of $\text{gcd}(a,b)$ and $a=qb+r$.
We can reduce with $\text{gcd}(a,b)=\text{gcd}(a\text{ mod } b,b)$ until we reach $gcd(0,x)$ where $x$ is the $\text{gcd}$

![Euclidean Algorithm Example](/images/IN4191/Euclidean-example.png)

**Extended Euclidean Algorithm** states that any number $\text{gcd}(a,b)=r$ can be written in the form of $ax+by=r$, hence for a $\text{gcd}(a,N)=d$ where $d=1$ we can compute $ax+yN=1$, where $x$ is the multiplicative inverse of $a$ in modulo $N$.

**Chinese Remainder Theorem** states that if we have two equations $x=a\text{ mod } N\text{ and }x=b\text{ mod }M$ then there is a unique solution module $M*N$ if and only if $\text{gcd}(N,M)=1$, and is given as

$$x\leftarrow \sum_{i=1}^ra_i*M_i*y_i\text{ mod } M$$

where

$$M_i\leftarrow M/m_i \text{ and } y_i\leftarrow M_i^{-1} \text{ mod }m_i$$

### Elliptic Curves

