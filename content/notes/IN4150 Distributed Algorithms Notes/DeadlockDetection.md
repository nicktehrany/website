+++
date = "2021-01-17"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-17"
title = "Deadlock Detection"
type = "notes"
mathjax = "true"
+++

### Models for Deadlock

A (directed) Wait-For-Graph (WFG) can be maintained with the processes as nodes and with an edge from process $P$ to process $Q$ when $Q$ is holding a resource that $P$ is requesting.

In the _resource model_ resources are associated with a process (e.g. $R_a$) and processes can then request said  resource. When a process $P$ wants access to a resource $a$ from $R_a$, an edge is created from $P$ to $R_a$, and when the resource is granted, the edge is removed and replaced by an edge from $R_a$ to $P$ indicating that $R_a$ is waiting for $P$ to release the resource. If the WFG contains a cycle there exits a deadlock and all processes in cycles on paths leading to cycles are deadlocked.

In the _communication model_ processes can do a blocking _RECEIVE_ operation to a set of processes, indicating that they want to receive a message from one process in this set (called the _dependent set_). After this _RECEIVE_ operation, the WFG contains an edge from $P$ to every process in its dependent set. If there exits a _knot_ in the WFG, there is a deadlock. A knot is present if a set of processes with a path from every process in the set to every other process in the set exists, and there are no edges from any process in the set to any process outside the set.

In _N-out-of-M_ requests a process sends a request to $M$ processes that can satisfy the request and it can proceed as soon as it has received $N$ _REPLY_ messages (It may send a _RELINQUISH_ to processes from which it has not received a _REPLY_). For static systems when $N=1$ this request is called OR-request, and when $N=M$ this request is called AND-request.

### Chandy, Misra, and Haas Deadlock Detection for AND (resource model WFG) Requests

A process is said to be _dependant_ if there is a path from it to some other process in the WFG. Processes maintain an array of dependencies _dep_, initially all set to false, and sets $dep_i(j)$ to true if $P_j$ knows $P_i$ is depending on it.

When a process suspects to be deadlocked, it sends a _PROBE_ message to the processes it is waiting for. The _PROBE_ messages are propagated by the receiver further through the system to the processes that the receiving process is waiting for. When the initiating process receives the _PROBE_ message, a cycle is detected and it is deadlocked (the _PROBE_ message is the form _probe(i,j,k)_ with id _i_, initiating process _j_, and receiving process _k_).

### Chandy, Misra, and Haas Deadlock Detection for OR (communication model) Requests

Every blocked process has a _dependent set_ of processes that contains the process for which it is waiting to receive a message, and upon receiving a message from any process in the dependent set, it becomes active again.

When a process suspects to be deadlocked, it sends a _QUERY_ to all processes in its dependant. Blocked processes will check the sequence number of the query, and if it has not seen it yet, propagate these messages to their dependent sets (generating a tree structure). Processes send a _REPLY_ message back to their engager process (from whom they received the query), once they have received a _REPLY_ from all the processes they themselves engaged (keep a counter of sent messages and received replies). A _REPLY_ is only sent when the process is still blocked by the same query (it has not become active by getting a response from someone else, then there is no deadlock) A process is deadlocked if for every _QUERY_ message it sent it receives a _REPLY_.

### Bracha and Toueg Deadlock Detection for Static Systems with Instantaneous Communication

Every node $P$ maintains a set of _OUT_ nodes $Q$ such that $(P,Q)$ is in $W$ (WFG) (the nodes to which P has sent a request but not received a reply from and also has not sent a relinquish to) and a set of _IN_ nodes $Q$ such that $(Q,P)$ is in $W$ (opposite set of nodes as the other one, the nodes to which it should send a reply or receive a relinquish from). The algorithm has two phases, the _notify_ phase where the spanning tree is created. When a node does not have any pending requests ($n=0$), it performs the _grant_ procedure (send a reply to any request it receives) and sets its _free_ variable to **true**. Sending of grants ends when a process has received a _REPLY_ from every process in its _OUT_ set. During the _simulate_ phase, _GRANT_ messages are used to simulate _REPLY_ messages, and when a process receives enough _GRANT_ messages, it becomes active again. If the initiating process has _free_ set as **false**, it is deadlocked.

### Bracha and Toueg Deadlock Detection for Static Systems without Instantaneous Communication

The problem with the previous algorithm is the assumption of instantaneous messages, in this algorithm messages can be in transit. Messages now have different colors

- _gray_, if $P$ has sent a _REQUEST_ to $Q$ which has not yet been received and $P$ has not sent a _RELINQUISH_ to $Q$
- _black_, if $Q$ has received a _REQUEST_ from $P$ but has not yet replied and $P$ has not sent a _RELINQUISH_ to $Q$
- _white_ if $Q$ has sent a _REPLY_ to $P$ which has not yet been received and $P$ has not sent a _RELINQUISH_ to $Q$
- _translucent_, if $P$ has sent a _RELINQUISH_ to $Q$ which $Q$ has not yet received

The algorithm is identical to the previous algorithm except the local sets _IN_ and _OUT_ now only contain black edges, and $n_v-$ #$graywhite_v$ is used instead of $n_v$.

At some point gray edges will become black, and white and translucent edges will disappear. We look at the total number of outgoing gray and white edges (#graywhite) and a node $v$ is active is $n_v\le$ #$graywhite_v$.

There may be a deadlock without detecting it, which means the algorithm has to be rerun to detect one.

Colors of edges are exchanged between messages by sending their _IN_ and _OUT_ sets to each other and comparing them to see the color (i.e. when  $u$ is not in $IN_v$ and $v$ in $OUT_u$ the edge is gray or white).

### Dynamic Systems

No static WFG graph of the system is available, but deadlock detection can be done by detecting a global state (e.g. with Chandy-Lamport) and then applying the algorithm for static systems with messages.
