+++
date = "2021-01-25"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-25"
title = "State Machine Replication"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

The state of a machine is constituted by the contents of the data store (data written by the clients), and for redundancy might want to be replicated across multiple machines, such that if one fails another can immediately take over. In order for all machines to maintain the same state they have to implement _linearizability_, since they all need to execute the client requests in the exact same order.

### Stopping Failure Tolerant SMR Algorithms

#### Lamport's Single-Value Paxos Consensus Algorithm

The algorithm assumes only stopping failures (may restart later, mut no malicious or bugs(Byzantine fault)) and an asynchronous system. This is the single-value paxos, in which a group of processes agrees on a single value proposed by one of its participants, and there is also a multi-value paxos, where a group of processes decides on a sequence of values. Messages may be reordered, duplicated, or lost, but not corrupted. Votes can only be in favour or no vote at all (no message reply), but **no** voting against.

There are three types of processes,

1. _Proposers_ that start with a value each that they may propose.
2. _Acceptors_ that accept and decide on a value (essentially voters)
3. _Learners_ that learn about the final outcome (essentially the servers)

The algorithm runs in rounds, each with a sequence number, and only one proposer per round proposes its value (use a predefined list of which proposer does which round). A round has two phases.

Phase 1 where the proposer tries to build the majority without actually sending them the value, using _REQUEST\_TO\_PARTICIPATE_ messages that are sent to some of the processes. When the acceptor receives the message and the round number is new, it sends a _PARTICIPATE_ with its last vote and the round number of that last vote.

Phase 2 where the proposer selects the most recent vote (highest round number) with the majority and proposes the corresponding value of the vote with a _REQUEST\_TO\_VOTE_ message. The proposer itself sends a message to the learner if it votes in favour (the learners receive all the votes and accept a value if at least one learner has received a value with the majority).

Variables that processes have are; _rnd_ the highest round number it has participated in, _vrnd_ the highest round number it has voted in, _vval_ the value it has voted for in the highest round number, _prnd_ is what the proposer maintains for the round number it initiated and _pval_ for the value it is proposing.

This algorithm has livelock, which can be solved with an election algorithm for electing a leader that is the only one doing proposals.

### Byzantine Fault Tolerant SMR Algorithms

These algorithms work with signatures and hashing, and use the notion of _views_ where each view has a _primary_ replica and the remainders the the _backup_ replicas. Views are numbered consecutively and the primary of a view $v$ is replica $p$ such that $p=v\text{ mod }n$ for $n$ total number of severs. Local data for replicas is the state machine, current view number, message log, and checkpoints of client requests that have been served. When all requests in a checkpoint are validated by all replica the checkpoint is called _stable_.

#### Castro’s and Liskov’s Practical Byzantine Fault Tolerance (PBFT)

In each view one of the replicas acts as the _primary_ and others are the backups. If a replica suspects a primary to be failing it will initiate a view change with $v+1$. Replicas broadcast their checkpoints in separate messages with a digest of the state and the number of the last request executed in it. When a replica received $2f+1$ matching checkpoint messages the checkpoint is _stable_, and previous checkpoints are deleted.

The operation of the algorithm consists of three phases,

1. After a client sent a timestamped request to the primary the primary gives the request a sequence number and sends a _pre-prepare_ message with the request, the view, and request number to all replicas. Replicas accept this message if it is in its current view and it has not accepted another _pre-prepare_ message in the same view and request number.
2. Each replica then sends a _prepare_ with the view and request number to all replicas (including themselves), and when a replica has received this and the corresponding _pre-prepare_, and at least $2f$ corresponding _prepare_ messages it sends a _commit_ message to all replicas.
3. When a replica received at least $2f+1$ _commit_ messages (also counting its own) with the correct view and request number its local state reflects the execution of all previous requests. It then runs the request and replies to the client.

A view change request from the backup (i.e. when it thinks the primary is faulty after some timeout and not receiving a _pre-prepare_) contains view number $v+1$ and the sequence number of the last stable checkpoint, the set of $2f+1$ messages that constituted the checkpoint, and for all request after the checkpoint for which it did the _pre-prepare_ the corresponding _pre-prepare_ and _prepare_ messages, which is all broadcasted to all replicas. When the primary of the new view receives $2f$ of these view change messages it broadcasts a _new-view_ message with the new view number, all _view-change_ messages it received and _pre-prepare_ messages for client request that may not have been executed. This way all replicas can sync up on the state of their checkpoint by retrieving missing information from other replicas (it knows its behind or ahead when comparing the sent data from the primary).

#### Kotla et al.’s Speculative Byzantine Fault Tolerance (Zyzzyva)

Again, the client sends its request to the primary which assigns it a sequence number and forwards it to all replicas (including self). Replicas immediately execute the request and reply to the client. If with no failures and fast replies a client receives $3f+1$ messages the request is considered done.

If there are failures or delays and the client receives less than $3f+1$ but at least $2f+1$ it sends a commit certificate to all replicas with proof that the majority of servers replied. When replicas receive the commit they respond with a local commit and when the client receives at least $2f+1$ such local-commit messages it considers the request done.

If the client receives less than $2f+1$ replies, it sends the request to all replicas who then expect the primary to still act on it. If the primary does nothing the replicas build a quorum of $f+1$ replicas that think the primary is failing by exchanging _I\_hate\-the\-primary_ messages, and once a replica receives $f+1$ such messages it sends a _view-change_ to all replicas. When the new primary (are assigned in pre-defined circular order) gets $2f+1$ _view-change_ messages it announces the new view with a broadcast.

## Randomized Solutions

With random algorithms at some points one or more processes flip a coin to make progress towards a solution. These algorithms may always terminate but only have a correct result with some probability, or it may terminate with some probability but with a correct result, or it may only both terminate and produce a correct result with some probability.

#### Ben-Or’s Randomized Algorithm for Consensus in Synchronous and Asynchronous Systems with Crash Failures

Every process has a binary input value $v$ and the number of faulty servers $f$ is $f< n/2$. The algorithm runs in rounds $r$ with three phases

1. The _notification_ phase where messages contain the message type $N$
2. The _proposal_ phase where messages contain the message type $P$
3. The _decision_ phase.

Processes wait for $n-f$ messages when they expect messages from all other processes.

The value $v$ is changed to $w$ (from the message with either a 0 or 1) after a single message was received and a decision on this value is made after more than $f$ messages with that value were received.

#### Ben-Or’s Randomized Algorithm for Consensus in Synchronous and Asynchronous Systems with Byzantine Failures

This algorithm has a different decision procedure, in which the value of $v$ is only changed to $w$ (from the message which is either 0 or 1) when more than $f$ messages were received, and decision is then only made when more than $f$ messages with the same $w$ were received.
