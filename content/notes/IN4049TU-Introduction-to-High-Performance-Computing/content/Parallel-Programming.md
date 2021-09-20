---
title: "Parallel Programming"
date: 2021-09-14T15:50:41+02:00
publishdata: 2021-09-14T15:50:41+02:00
type: "notes"
mathjax: true
---

For distributed memory MPI is used as programming model, with shared memory OpenMP is used, and for GPUs CUDA is used.

### MPI Program

With MPI programs, the processes that execute the program all have their own local data. Typically one process will be on one core, each process can then access its own data locally and use message passing to get other data from other cores. In MPI programming the environment needs to be initialized before it can be used, and finalized when it's done. `numberOfProcs` is set to the total number of processes (e.g. with 4 processes this is 4), and `rank` is the rank of the process, this starts at 0 (!!), so the first process will have rank 0, second will have rank 2, and so forth.

### Point-to-Point Communication

For enabling communication for processes that don't share memory.

`MPI_Send` and `MPI_Recv` are both blocking calls, therefore deadlocks can occur and should be avoided.


