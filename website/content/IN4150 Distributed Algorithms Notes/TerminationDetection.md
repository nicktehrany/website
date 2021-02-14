+++
date = "2021-01-16"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-16"
title = "Termination Detection"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

Termination detection is the problem of determining whether a distributed computation in a distributed system consisting of processes which communicate by means of message, has terminated.

Distributed computations have the following properties:

- A process is either _active_ or _passive_
- Only active processes can send messages
- An active process may become passive spontaneously
- A passive process becomes active at the reception of a message

### Termination Detection in an Asynchronous Unidirectional Ring with FIFO Communication

There exits a process $P_{0}$ that is on top of the ring and other processes are connected to it in the order of $P_{n-1}$ to the right of $P_0$ and $P_1$ to the left. $P_0$ sends a _token_ to detect termination to $P_{n-1}$, and each process and the token has a color (black or white, initially white). The token is only forwarded when a process becomes passive, but a passive process can be woken up by a message from a process that the token has not been to yet (a lower numbered process), at which point the process sending the message will change its color to black. When a token arrives at a black process its color is changed to black and no decision can be reached in the current round and the token is relayed immediately. The process also sets its color back to white to start the new round. When $P_0$ receives a white token and is itself passive, it concludes termination.

### Termination Detection in a General Network

A special process $P$ controls the algorithm but does not participate in the computation. All processes and messages carry non-negative weights, which are assigned by process $P$. $P$ initially has weight $1$ and splits its weight over all the processes. When a process wants to send a message, it halves its own weight and sends the half along in the message. The receiving process adds the weight from the message to its own weight. When a process terminates, it sends its remaining weight to process $P$ and sets its own weight to $0$. Terminated is detected when the weight of process $P$ is $1$ again.