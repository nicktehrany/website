+++
date = "2021-01-18"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-18"
title = "Mutual Exclusion"
type = "notes"
mathjax = "true"
+++

In a distributed system in order to access the resource, a process will execute the critical section of the resource, hence it is required to guarantee that at most one process is in the critical section at a time. These algorithms aim to solve mutual exclusion with no deadlocks, no starvation, and some notion of fairness.

### Assertion-Based Mutual Exclusion

A process has to request permission from all or part of the other processes and based on their replies it may conclude that is the only one with rights to enter its CS.

#### Lamport's Mutual Exclusion Algorithm

All links are FIFO and all messages carry a timestamp with a pair consisting of a scalar logical time and the id of the sending process. A process that wants to enter its CS broadcasts a _REQUEST_ message (including to itself). Upon receiving a request, a process enters the request in its request queue, which is ordered according to timestamp (with ids as tie breakers), and sends a _REPLY_. When a process has received a reply from every other process and is at the head of its request queue, it enters its CS. When a process leaves the CS, it sends a _RELEASE_ message to all processes, and upon receiving the _RELEASE_ processes remove the sending process from their request queue.

Message complexity is $3(n-1)$.

#### Ricart's and Agrawala's Mutual Exclusion Algorithm

Since in the previous algorithm the sending of reply messages while one process is in the CS is superfluous, as the other process needs to wait for the release anyways. In this algorithm a process defers sending a _REPLY_ to a request if it currently has a request of its own that is older until its own request has been satisfied, If a process does not have a request of its own it sends a _REPLY_ immediately, and this way _RELEASE_ messages are no longer needed.

Message complexity becomes $2(n-1)$.

#### Maekawa's Mutual Exclusion Algorithm

Request sets have the following properties:

1. Every two request sets have a non-empty intersection: $R_i\cap R_n\ne0$
2. Every process is contained in its own request set: $i\in R_i$
3. Every request set has the same number of elements, so $|R_i|=K$ for a positive int $K$
4. Every process appears the same number of times in a request set, so $i\in R_j$ for $D$ values of $j$ for some positive int $D$

When a process wants to enter its CS it multicasts a _REQUEST_ message to the members of its request set $R$. After a process received a _GRANT_ from all processes in $R$, it can enter its CS, and when done it sends a _RELEASE_ to every process in $R$. In order to avoid deadlocks, when a process received a _REQUEST_ it replies with a _GRANT_ if it has not sent the grant to another process. If it had already sent a grant, it checks the timestamp of the request and if it is new, it enqueues it in the request queue $Q$ and sends a _POSTPONE_ message to the requesting process, otherwise it will send an _INQUIRE_ to the process to whom it has sent the grant. A process that receives a _GRANT_ will either wait for grants from every process in $R$ or until it received a _POSTPONE_ (i.e. once it knows if it will be able to enter its CS). If it cannot enter its CS it will send a _RELINQUISH_ to give the grant back, and the receiver will enqueue the request in $R$ (the process where the relinquish came from), and send a _GRANT_ message to the head of request queue. (Note: the request queue is sorted by oldest request first, based on the timestamp and ids as tiebreakers).

Under light system load the message complexity will be $3(K-1)$ since only _REQUEST, GRANT,RELEASE_ messages will be sent. Under more contention it will be $4(K-1)$ since some additional _POSTPONE_ messages will be sent, and when there is old requests it will be $5(K-1)$ since additionally now _RELINQUISH_ messages will be sent.

#### Generalized Mutual Exclusion Algorithm

Every process has a request set $R_i$ and an inform set $I_i$. The processes that are informed about releases are the inform set, which is a subset of the request set. This way requests are sent to all processes in the request set and releases are only sent to a subset of it. Grants also come from the complete request set. The status set is the converse of the inform set, e.g. process $P_i$ informs $P_j$ about its state, then we know that $P_j$ is in the inform set of $P_i$ and therefore the status set of $P_j$ will contain $P_i$.

When a process wants to access the CS it sends a _REQUEST_ to all processes in the request set and waits for a _GRANT_ from all. When leaving the CS a process only sends a _RELEASE_ to its inform set, where each process keeps a variable _p\_in\_CS_ for all processes to which it has sent a _GRANT_ and not yet received a release from. Grants are then given if _p\_in\_CS_ is NULL (no one has the grant). When receiving a release the _p\_in\_CS_ is reset and it checks its queue if it has outstanding requests and sends the grant and setting the variable to the next outstanding request.

### Token-Based Mutual Exclusion

Token based algorithms are based on the use of a single token that grants access to the CS to the process holding it, hence mutual exclusion is trivially satisfied.

#### Suzuki's and Kasami's Broadcast-Based Mutual Exclusion Algorithm

A single token exists that is circulated among the processes and the process holding the token can enter its CS. When a process wants to enter its CS it sends a request to all other processes (including itself). Every process maintains a local queue of requests with the process id as index (e.g. with 3 processes $LN=[0,0,0]$, and increments $LN_i$ when it receives a request from $P_i$). The token also has an array $N$ of all the satisfied requests and same as before increments $N_i$ when the request of $P_i$ has been satisfied. By comparing the elements in the two arrays $N$ and $LN$ it decides who is next to receive the token. In order to avoid starvation the search starts at the index of the own id.

Message complexity is $n-1$ plus the one message for the token transfer.

#### Singhal’s Multicast-Based Mutual Exclusion Algorithm

In this algorithm processes only send a request to the processes they think may have the token, cutting down on message complexity. Processes can be in four states:

1. **R:** requesting the token
2. **E:** executing the critical section
3. **H:** holding the token but not in the critical section and not aware of any other requests
4. **O:** other

Every process has an array $N$ of integers counting the number of requests for each process (initialized to all 0) and an array $S$ of the state of the processes. The array $S$ is initialized in the shape of a triangular matrix, meaning that $P_0$ will initialize it to

$$S[0]=H,$$
$$S[j]=0,j=1..,n-1$$

meaning that it is the only process in state $H$ (holding the token), and process $P_i,i=1...,n-1$ as

$$S[j]=R,j=0,...i-1,$$
$$S[j]=O,j=i,...,n-1$$

meaning that all processes with lower ids get state $R$ and higher ids get state $O$. This way processes always think the token is in one of the processes with a lower id. The token has an array $TN$ which works exactly as in the previous algorithm and an array $TS$ to maintain knowledge on the state of the processes. When a process $P_i$ requests the token and receives a request from $P_j$, it sends $P_j$ a request message, as it can get the token earlier in the future and so $P_i$ can request it.

Message complexity is on average in a very low contention system $n/2$ _REQUESTS_ and the token transfer. In high contention when almost every process has an outstanding request message complexity approaches $n$.

#### Raymond's Token-Based Mutual Exclusion Algorithm in a Tree

This algorithm uses a spanning tree for the processes and assumes this has been created ahead of time, and the root of the tree may change over time.  The process that holds the token is the current root of the tree and can enter its CS. Every process stores its parent in a variable _holder_ (the neighbor on the path to the root). Every process maintains a queue of ids of those neighbors that requested the token. When a process wants to enter the CS it adds itself to its own queue and sends a _REQUEST_ message to its parent. Upon receiving a _REQUEST_ message, the process appends the sender's id to its local queue, if not already done it sets _asked=false_ (this is to not serve multiple requests, i.e. only propagate the first request and then queue everything else without propagating), and sends a request to its parent. The root sends the token to the head of its queue (removes the element from the queue) and assigns it to be its parent. A process receiving the token enters the CS if it is at the head of its own queue and otherwise it sends the token to the head of its queue (remove it as well) and sets it to be its parent.

Worst case message complexity is $2(N-1)$ if the tree is just a chain.
