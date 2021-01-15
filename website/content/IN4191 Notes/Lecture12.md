+++
date = "2021-01-15"
description = "IN4191 Security and Cryptography Notes"
linktitle = ""
publisdate = "2021-01-15"
title = "Lecture 12: Secret Sharing"
slug = "Lecture 12"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

## Secret Sharing (Chapter 19)

**Threshold Cryptography** split key into shares and a certain number of then can reconstruct the secret key. Or performing operations jointly (decryption, signing, etc.) but the secret key is not known to anyone (e.g. group signatures).

Dealer $(D)$ has the secret key $s$ and provides the shares to different parties $P_1,...,P_n$

Distribution: Protocol in which the dealer provides each party $P_i$ a share $s_i$.

Reconstruction: Protocol in which a qualified set of parties pool their shares to obtain secret $s$.

Access structure: The collection of all groups of parties that can reconstruct.

### Shamir Secret Sharing

A dealer first creates a secret polynomial of degree $t$ and gives out points on the graph to parties, and in oder to reconstruct, the points of $t+1$ parties need to come together to reconstruct the polynomial and recover the secret (intercept of $y$ in the graph). Since the $y$-intercept is the secret, the point at $x=0$ is not given out.

### RSA Shared Signature

$(t,n)$ RSA threshold scheme that uses a polynomial of degree $t-1$ and a prime $e$ such that $e>n$. Then

1. compute points on the polynomial
2. each server signs partially a document
3. user received the shared and constructs the signature from $t$ shares

![RSA Shared Signature](/images/IN4191/RSA-Secret-Share.png)
