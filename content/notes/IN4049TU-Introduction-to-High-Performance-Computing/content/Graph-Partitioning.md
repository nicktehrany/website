---
title: "Graph Partitioning"
date: 2022-01-22T19:10:04+01:00
publishdata: 2022-01-22T19:10:04+01:00
type: "notes"
mathjax: true
---

The goal with graph partitioning is that the entire graph is partitioned into even parts (about the same size/weight associated to the nodes/edged). However, choosing an optimal partitioning is NP-complete (can be proved that this is just as difficult as other hard problems in Nondeterministic Polynomial time). Therefore the only algorithms for partitioning have exponential cost on the number of nodes in the graph. This is why a heuristical approach is needed.

## Partitioning with Nodal Coordinates

### Inertial Partitioning

A 2D graph, pick a line that has half the nodes on one side and the other on the other side (for other dimensions it's 2D plane, etc.). Then a perpendicular line to that line is chosen and each point is projected onto that lean, with which now the median to partition the nodes by minimizing the sum of the squares of each node to the line L. The resulting line is the total least squares of all the points.

## Partitioning without Nodal Coordinates

### Breadth First Search (BFS)

From a graph a subgraph $T$ with root $r$ is produced and each node is assigned a level which is its distance to the root. Then using a queue going up in the levels (visually from root going down but level id increases) and have a queue for processed nodes. For the current node, for all it's unmarked children (hasn't been processed) add the child to the new tree $T$ as node $N_T$. Also add the edge to the tree as $E_T$. Then add the this child to the queue for processing, mark that child as having been processed (this happens in for loop for all unmarked children, so first process all unmarked children then later their children). The queue is FIFO and the entire thing runs in a loop while the queue is not empty,


### Coordinate Free: Kernighan/Lin

Given a graph, iteratively improve it by picking a new partition that splits the graph into two even halves $A$ and $B$, then find a subset of each of the splits $X$ from $A$ and $Y$ from $B$ such that they are even $|X|=|Y|$. If the cost of the two is lower when swapping the two swap them (cost is summation of edge weights)

### Spectral Bisection

Using an Incidence matrix, we can show with it the graph connection by having in the Incidence matrix the connections depicted (1 for source -1 at the sink) vertex (each row in Incidence matrix is an edge and column is a vertex). Based on the Incidence matrix we can generate the Graph Laplacian as the transpose of the Incidence matrix time the Incidence matrix ($C^T * C$). 

## Multilevel Partitioning

Similar to the previous methods for multigrids, here also replace the graph by a coarse approximation and partition that approximated graph. That partition we can then iteratively improve. Coarsening a graph can be done with maximal matching, which is a greedy approach that checks each node and matches them such that there are no two edges that share an endpoint.
