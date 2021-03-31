+++
date = "2021-01-12"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-12"
title = "Time Concepts"
type = "notes"
mathjax = "true"
+++

Time has an important role in computer systems, the applications that they run need to be able to keep track of time and
events and compare timestamps. In asynchronous systems it may be necessary to reason about events based on their order of
occurrence.

### Happened-Before Relation

The basis of the theory of ordering events in distributed systems is the _happened-before_ relation, and is given as $\rightarrow$ as the following

1. **Local Order:** If $a,b\in E_i$ for $i$ and $a$ occurred in $P_i$ before $b$ then $a\rightarrow b$.
2. **Message Exchange:** If $a\in E_i$ is the event in $P_i$ of sending message $m$ and $b\in E_j$ is the event in $P_j$ of receiving message $m$ for some $i,j$ with $i\ne j$ then $a\rightarrow b$.
3. **Transitivity:** If for $a,b,c\in E$, $a\rightarrow b\text{ and }b\rightarrow c$, then $a\rightarrow c$.

This is often called the _causality relation_, such that if $a\rightarrow b$ it is said that $a$ causally effects $b$.

The _concurrency relation_ states that two events $a,b\in E$ are concurrent (written as $a||b$) when neither $a\rightarrow b$ nor $b\rightarrow a$ holds.

**Causal Past** (history) is given as $P(a)=\\\{b\in E|b\rightarrow a\\\}$.

**Current Events** are given as $C(a)=\\\{b\in E|b|| a\\\}$.

**Causal Future** is given as $F(a)=\\\{b\in E|a\rightarrow b\\\}$.

### Logical Clocks

Logical clocks are functions $C:E\rightarrow S$  that assign an element of some totally ordered set to each event with a timestamp that respects the HB relation in some way (if $a\rightarrow b$, then $C(a)<C(b)$).

Scalar or Lamport clocks cannot characterize the HB relation, hence partial order can be introduced with 1-dimensional vector clocks $C$. Vector clocks are constructed by having each process $P_i$ maintain an integer counter for each other process, with initial value 0 and using it as follows

1. If $a\in E$ and if $a$ is not a message receive event, then $P_i$ first increments $C_i$ by $1$ and sets $C(a)$ equal to the new value of $C_I$.
2. If $a\in E_i$ is the event of $P_i$ sending a message $m$ and $b\in E_j$ is the event in $P_j$ receiving message $m$ for some $i,j$ with $i\ne j$, then $P_i$ sends $C(a)$ along with message $m$ to $P_j$. On receipt, $P_j$ assigns $C_j$ the value $max(C_j+1,C(a)+1)$ and then sets $C(b)$ to the new value of $C_j$.

![Vector Clocks](/images/IN4150/VectorClocks.png)

The size of the vector clocks needs to be at least the number of processes, $k\ge n$.
