---
title: "Parallel Programming on GPUs"
date: 2022-01-22T17:13:01+01:00
publishdata: 2022-01-22T17:13:01+01:00
type: "notes"
mathjax: true
---

## Parallel Computing on Graphical Processing Units

Graphics cards are often used for scientific computing using SIMD (single Instruction Multiple Data) and providing parallelism in the cards with threads. GPUs offer cheap hardware that is capable of running thousands of threads at once, and it is better scalable than MPI due to the significantly lower communication overhead. However, GPUs only have limited available memory (often without ECC) and were complicated to program.

### CUDA

CUDA (Compute Unified Device Architecture) make this easier (for Nvidia) by providing GPU programming similar to C with library routines, language extensions such as codewords, etc.
This is similar to OpenCL (Open Computing Language) which was used for parallel kernel programming, however OpenCL is only for GPUs but it is open source unlike CUDA being Nvidia built. 

## GPUs

GPUs are best utilized for Matrix products and not so good for matrix-vector products.

GPUs can have 1 or more devices -> which have several streamprocessors (SM) -> which have up to 32 active concurrent threads (called __warps__). A __block__ is the set of threads that running on single SM. The SM can run up to 32 threads in parallel and up to 768 or 1536 sequential.

Devices have __global memory__ that all streaming multiproccessors can access and which is persistent. A __constant and texture memory__ that can only be __read__ and which also have a local cache on each SM. The SM then each has a shared memory for its threads.
