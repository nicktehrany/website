---
title: "Poisson Equation"
date: 2022-01-21T21:23:46+01:00
publishdata: 2022-01-21T21:23:46+01:00
type: "notes"
mathjax: true
---

## Iterative Methos for Linear Systems

Iterations methods are used for solving linear equations (often heat problems, air flow for planes, etc.) with methods such as Jacobi Iteration, Gauss-Seidel iteration and SOR (successive over-relaxation). These iterative methods generate a sequence of approximation vectors that converge to a solution. Evaluations then show how quickly this iteration sequence converges. With Jacobi and Gauss-Seidel, the computation of a new approximation depends on a combination of prior computed approximated vectors, which is why they are called _relaxation methods_.

Convergence for Jacobi and Gauss-Seidel occurs when the matrix (approximation result) is strongly diagonal dominant, which means the absolute values of the diagonal of the elements in the matrix are significantly larger than that the absolute value of the sums of other rows.

Since often convergence with these two methods is not fast enough, an additional _relaxation parameter_ is added. This is what __Jacobi over-relaxation__ is based on. With it the matrix is split with the additional relaxation parameter.

__Successive over-relaxation__ is Gauss-Seidel with the relaxation parameter. The result from the prior approximation is combined with the elements in the current one to give a new relaxation parameter for each iteration.

## Parallel Implementation of Jacobi Iteration Method

Since each element in the approximation can be calculated individually, we can have a maximum parallelism here of the number of approximation elements. However, since approximations of each element require those of the previous approximation there is a need for some communication.

## Parallel Implementation of Gauss-Seidel Iteration Method

Here all elements in the approximation depend on the prior element in that approximation instead of the prior approximation. This means that this cannot be parallelized, but only the part of actually calculating the element (the scalar for that approximation).


## Red-black ordering

Since there is only limited parallelism, there needs to be a reordering that splits the 2D mesh (of grid points that have to exchange approximation elements) into red and black pairs like a checkered board. This way all points can do independent calculations then switch to the other color to do their independent operations (however they then depend on the prior color, which is why always one color is active).

## Summary

For Jacobi SOR and CG it takes $n=N^\frac{1}{2}$ steps with an $nxn$ matrix for the info to have traveled across the matrix.

## Metrics

Data Locality Ratio is given as

$$\frac{T_{computed}}{T_{communicated}}$$


Communication time is given as, with $L$ message length in bytes, $Latency=Startup$ and $B$ being bandwidth

$$T_{comm}(L)=Startup+\frac{L}{B}$$

## PRAM

Shows a simplified abstraction of parallel hardware, where there are e.g. no communication overheads for memory accesses.
It has four variants:

1. __Exclusive-read, Exclusive-write (EREW)__: Each processor can only read and write to its memory not any other belonging to other processors.
2. __Concurrent-read, Exclusive-write (CREW)__: Processors can read from any memory, but can only write to their own.
3. __Exclusive-read, Concurrent-write (ERCW)__: Processors can only read from their own memory but can write to any other belonging to any other processor.
4. __Concurrent-read, Concurrent-write (CRCW)__: Processors can both read and write to any memory.
