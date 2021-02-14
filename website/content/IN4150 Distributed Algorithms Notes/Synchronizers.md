+++
date = "2021-01-13"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-13"
title = "Synchronizers"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

Synchronizers are algorithms that simulate synchronous systems on top of asynchronous systems. They proceed in rounds of sending messages, receiving messages, and performing local computations, and these algorithms work on the foundation of issuing a _pulse (clock)_ to allow a process to move to the next round.

### Types of Synchronizers

#### Alpha Synchronizer

In $\alpha$-synchronizers, when a node receives a message, it sends an ACK message back to the sender. When a process received an ACK for every message it has sent in some round, it is called _safe_. It then sends a SAFE message to its neighbors, and when a node is safe and has received a SAFE message from all its neighbors it can proceed to the next round.

**Communication complexity** is twice the number of edges (1 for the ACK, 1 for the SAFE) additional to the conventional messages that are sent.
With $N$ number of nodes, the worst case communication complexity is $O(N^2)$.

#### Beta Synchronizer

In the $\beta$-synchronizer, the nodes first elect a leader and create a spanning tree with the leader as the root. In every round there is a wave of PULSE messages from the leader downwards, indicating that all nodes can start the next round. When a leaf receives a PULSE message and it knows it is safe (received ACK for every message it sent), it sends a SAFE message. When a non-leaf node in the tree has received a SAFE from all its descendants and is itself safe, it sends a SAFE message to its parent. When the root has received a SAFE message from all its descendants and is itself safe, it generates a new PULSE for the next round.

**Message and time complexity** are both in the order of $O(N)$ as there are $N-1$ links in the spanning tree and the maximum depth is $O(N)$.

#### Gamma Synchronizer

In the $\gamma$-synchronizer nodes are partitioned into clusters, where each selects a leader and constructs a spanning tree ($\beta$-synchronizer), and a single preferred link is selected that connects any two nodes of every pair of clusters. The $\beta$-synchronizer is executed in every tree, and the $\alpha$-synchronizer is executed among the trees, and when a root knows its whole tree is safe it sends a CLUSTER\_SAFE down its tree and across the preferred link to its neighboring tree. CLUSTER\_SAFE messages stop at the node in the other cluster with the preferred link. Once that cluster
is then safe it will send its own CLUSTER\_SAFE messages down the tree, and once a node has received READY messages from all its
descendants and has received a CLUSTER\_SAFE on all the preferred links it is connected to it will propagate a READY message
 upwards (leaf nodes start since they do not have any descendants). When a root receives all READY messages it knows that its
 own tree is safe and that all its neighbors are safe.

The **message complexity** if of the order $O(E)$ with $E$ the total number of links. The **time complexity** is in the oder of $O(H)$ with $H$ the maximal height of the spanning trees.

Synchronizers only work in fault free systems, therefore wen faults occur synchronous systems are more powerful.
