+++
date = "2021-01-22"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-22"
title = "Minimum-Weight Spanning Trees"
type = "notes"
mathjax = "true"
+++

A reason for using minimum-weight spanning trees (MST) is to broadcast some message along the tree with minimum weight, if for example the transmission cost is used as the weight for edges. MSTs are constructed from weighted (unique weights) undirected graphs. Unique weights are required for finding a unique solution for an MST, and for the following algorithm. A _fragment_ of graph $G$ is a subtree of its MST. Edge $e$ of $G$ is the minimum-weight outgoing edge (MOE) of fragment $F$ if it has the minimum weight that connects $F$ to another fragment.

### Gallager’s, Humblet’s, and Spira’s Algorithm for the MST of an Undirected Weighted Graph with Unique Edge Weights

The idea of the algorithm is to have fragments connect to each other along their MOEs, starting with single node fragments and resulting with the final MST. The MST is constructed by repeatedly connecting pairs of fragments along a single edge that is the MOE of one of the fragments. Each fragment has a _level_, and single node fragments start with level 0, and a _core_. The level is used for deciding wether to merge or absorb other fragments, and is only increased when a merge happens. With a merge, the core will also be changed to the edge along which the merge happened.

**Message complexity** is

$$O(5*|V|* \text{ log }|V|+2*|E|)$$

A MST of level $k$ has a maximum of $2^k$ nodes.
