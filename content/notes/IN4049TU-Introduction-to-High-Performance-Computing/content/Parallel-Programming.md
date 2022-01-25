---
title: "Parallel Programming"
date: 2021-09-14T15:50:41+02:00
publishdata: 2021-09-14T15:50:41+02:00
type: "notes"
mathjax: true
---

For distributed memory MPI is used as programming model, with shared memory OpenMP is used, and for GPUs CUDA is used.

### MPI Program

With MPI programs, the processes that execute the program all have their own local data. Typically one process will be on one core, each process can then access its own data locally and use message passing to get other data from other cores. In MPI programming the environment needs to be initialized before it can be used, and finalized when it's done. `numberOfProcs` is set to the total number of processes (e.g. with 4 processes this is 4), and `rank` is the rank of the process, this starts at 0 (!!), so the first process will have rank 0, second will have rank 1, and so forth.

### Point-to-Point Communication

For enabling communication for processes that don't share memory.

`MPI_Send` and `MPI_Recv` are both blocking calls, therefore deadlocks can occur and should be avoided.

MPI Datatypes are used to tell it how to interpret binary values from the send buffer/receive buffer, convert them etc. It has a bunch of predefined datatypes such as `MPI_CHAR` or `MPI_INT`.

MPI also has `MPI_Sendrecv` which sends one message and receives another in any order, and buffers (send, and recv buffers) have to different and cannot overlap. There is also the `MPISendrecv_replace` the first sends the message then receives using the same buffer.

`MPI_Isend` and `MPI_Irecv` are the non-blocking counterparts to the prior mentioned ones. One could then also use the `MPI_Wait` to block until it's completed, typically this is done after some work is completed and the process needs the data from the recv.

Four send modes in MPI:

- Standard mode: Block until the message is fully transferred.
- Synchronous mode: Block until a reply (the recv) from the receiver has arrived.
- Buffered mode: Block until the message (to be sent) is copied to a buffer (specified by user)
- Ready mode: If a receive exists only then does this succeed. Not recommended to be used
