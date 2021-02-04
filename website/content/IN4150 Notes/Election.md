+++
date = "2021-01-20"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-20"
title = "Election"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

It may sometimes be necessary in a DS to assign a certain process with a higher privilege, for which election is used. If each process has a unique integer id, the system isa said to be _non-anonymous_ and _anonymous_ otherwise. In anonymous rings no deterministic solution exists to election, as processes need to randomly generate some id value (e.g. 64 bit number). Algorithms that work in a ring of an unknown size are called _uniform_. _Comparison based_ algorithms rely on the comparison of ids, hence they can only send and receive messages and compare process ids. The message complexity of comparison based algorithms in rings is $\Omega(n\text{ log }n)$.

### Bidirectional Rings

#### Hirschberg's and Sinclair's Election Algorithm in Bidirectional Rings

A process tries to establish if its id is larger than that of its neighbors. In the first round it sends a _PROBE_  with its id and a hope counter (to know when to stop forwarding the message) to both its neighbors (both directions) and if it is larger the neighbors send an _OK_ if the hop counter is 0 otherwise it decrements the hop counter and forwards it, otherwise if its id is larger than that in the message it discards the message. In the next round, if the process received an _OK_ from both its neighbors it continues by checking its next 4 neighbors (2 in each direction) by sending its own id. In the next round, if it received _OK_ from the neighbors it sends its id to 8 of its neighbors (4 in each direction) and does the same. Each round the number of neighbors is increased by double (2x). If the ring size is known the processor knows it has been elected when it received _OK_ in round when the probe messages meet at the other side of the ring, and if the size is not known it knows it has been elected when it receives its own _PROBE_ from the wrong side.

**Message complexity** is $O(n\text{ log }n)$, since $n$ messages will be sent and in each round at least half of the processes stop, hence the $\text{log }n$ part.

#### Enhanced Version

As in the comparisons still happen if a process already knows it will now be elected, this algorithm cuts down on this extra by assigning states to processes. If a process detects that ts own id is larger than those of its neighbors it remains active and initiates the next phase, otherwise it becomes passive. In subsequent rounds only the active processes execute the same algorithm in the _virtual_ ring of still active processes (passive ones simply relay messages). When a process receives its own id it has been elected.

### Unidirectional Rings

Assume that neighboring processes can only send to their right hand neighbors.

#### Non-Comparison Based Algorithm in a Unidirectional Ring

In this algorithm the process with the minimum id is elected, and the size of the ring is known to all processes. When a process finds that its id is equal to 1 it knows that it will be elected and sends its id to its neighbor. Every process immediately relays received ids. If a process does not receive a 1, it knows id 1 does not exist and in the next round id 2 is checked. This continues with rounds until a process has an id that is equal to the round number.

**Message complexity** is $n$, the size of the ring, and **time complexity** is $n$ times the minimum of the ids (so $n$ rounds until id is equal).

#### Chang's and Robert's Election Algorithm in a Unidirectional Ring

At least one processor randomly starts the algorithm by sending a message with its id to its neighbor. Upon receiving a message, if the id is larger than the processes' own id it forwards the received id, and if it is smaller it sends its own id. A process is elected when it receives its own id.

**Message complexity** is at least $n$, at most $n(n+1)/2$ and on average $O(n\text{ log }n)$.

#### Peterson's Election Algorithm in a Unidirectional Ring

This algorithm works similar to the improved version of the bidirectional algorithm, in that it will simulate bidirectional rings in unidirectional rings. It works by having every process send its id to its right neighbor and this neighbor then sends the maximum of its own id and the value it received to its right neighbor. This way the third one has information about the 2 predecessors, which is the same as in bidirectional rings where processes talk to both neighbors, viewing the 3 ids it has as the middle one the requesting one. Among three values it received, it checks that the value of the middle one of the three is at least as large as both its neighbors, and if so the process remains active and its id is now represented as the middle value, otherwise it becomes passive. A process is elected when it receives its own id.

The number of rounds is at least equal to $\text{log }n$ since in every round the number of active processes is cut in half. **Message complexity** is $2n\text{ log }n$ since in every round exactly two messages are sent along every link.

### Complete Networks

#### Afek's and Gafini's Election Algorithm in Synchronous Complete Networks

In this algorithm a node that wants to b e elected sends its id repeatedly to ever larger different subsets of other nodes, which only return acks if the id received exceeds the largest id in the system that they know about. When a candidate does not receive enough acks, not equal to the number of nodes in its subset, it ceases to be a candidate, otherwise if it received all acks for its subset it is elected. The initial subset is size 1 and every round the subset is doubled. Nodes keep track of other nodes they have sent their id to in order to not send them the id again if they are in the subset.

Any process can spontaneously start the algorithm by creating a _candidate_ process and an _ordinary_ process, other nodes that did not start the algorithm just start an _ordinary_ process. The _candidate_ process keeps track of their _level_ which is the round number since they started. Candidates send messages containing the level and their own id to the nodes in the subset. Processes order the messages they received in every round based on the level (higher is better and level first) and then id as tiebreaker. If the maximum of the candidate ids is larger than the own id of the process it sends an _ACK_ to only that process. Then it is said that the ordinary process is _captured_ by the candidate process (since it assumes its id), also called the _owner_. Ordinary processes keep track of their level which is incremented by 1 in every round, or is set to the level of a received message, if larger. The node elected is the node with the largest id (among all candidates) and was earlier to start the algorithm.

Maximum number of rounds is $\text{log }n$ since after this the winner will have captured all nodes. Maximum number of messages is $2n*\text{ log }n$, since every node sends at most $n*\text{ log }n$ acks (one every round).

**Note** that in any round the set of nodes captured by different candidate processes are disjoint (do not contain any common elements).

#### Afek's and Gafini's Election Algorithm in Asynchronous Complete Networks

This algorithm works in the same way as the previous but now the level indicates the number of nodes a candidate process has captured. This introduces the problem, of a candidate process capturing a node that had already been captured by another process. Since the current owner will not be elected anyways, and to reduce the number of messages, the node captured again tries to kill its owner. It does this by first maintaining a pointer to the _owner_ and the _potential-owner_ and since this is asynchronous, the previous owner may already have been killed by another node.

**Time complexity** is $n$ since candidates capture 1 nodes independently and in the end will have $n-1$ captures. When a candidate process kills another candidate process, the former must  have done at least the same amount of work as the latter, since it needs to have a higher level to take over ownership of a node. **Message complexity** is $n\text{ log }n$.

#### General Networks

Leader election in general networks by one process initiating it and creating a spanning tree on which it propagates its own id down. Nodes only propagate the message further if the id is larger than their own, and leave nodes will send a _SUCCESS_ message up the tree. When the initiator receives _SUCCESS_ messages on all its links it has been elected, if not some other process can then start their own initiation procedure.
