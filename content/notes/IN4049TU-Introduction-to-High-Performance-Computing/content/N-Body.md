---
title: "N-Body"
date: 2022-01-23T13:21:28+01:00
publishdata: 2022-01-23T13:21:28+01:00
type: "notes"
mathjax: true
---

N-Body simulations approximate the evolution of a system numerically. In this each body of the system constantly interacts with every other body.

The bottleneck within this if particles depend on all other particles. If they just depend on some external factor or close neighbors, this can easily be done but all other particles means for every particle you have to loop through all other particles which is a heavy overhead ($O(N^2)$.

An approach to improve this is with divide-and-conquer algorithms that only account for neighbors that are close enough to a certain particle instead of the entire system. While this means that there will be less computing, it can be inaccurate or even incorrect (depending on the application). A so called __box__ is then a collection of particles that will be represented as one, and the __radius__ $r$ is the distance from the current particle to the center of that box (Center of Mass, CM).

From this one can then construct a __Quad Tree__ where each non-leaf node has 4 children (in a complete quad tree, otherwise it can have less), each node (depicting the box has more boxes in it). Similar to this you can have _oct trees_ with up to 8 children per node.

Since such complete tree would waste space, as there are not the same number of particles in each box, and therefore a lot of the children nodes will be empty, there are __adaptive quad trees__ where empty nodes are excluded in the tree.

## Barnes-Hut Algorithm

It uses the quad tree and then for each node calculates the CM of all its particles (so it's children) then uses this to compute forces of sub-squares. Complexity is $O(N \textmd{log} N)$ just like the prior tree methods.

## Fast Multipole Method (FMM)

Instead of BH it includes more info than the center mass and total mass at each box, which makes it more accurate but more expensive. However, to compensate for this it only accesses a fixed set of boxes at each level in the tree. The subset of the quad tree that a local processor needs (in the parallel running of it) is called the __Locally Essential Tree (LET)__. For each LET the force calculations can then be done in parallel.
