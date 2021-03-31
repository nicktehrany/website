+++
date = "2021-02-12"
description = "IN4150 Distributed Systems Notes"
linktitle = ""
publisdate = "2021-02-12"
title = "Functional Requirements"
mathjax = "true"
+++

## Reading Material Before Lecture

### Remote Procedure Call (RPC)

RPC allows one program to call a procedure (subroutine or method) in another address space than its own, typically on
another system, without the programmer explicitly implementing this remote interaction but instead it is the same as
if a local process would invoke some method, except now it comes from a non-local process.

### Message Bus

Since distributed systems rely on message passing between different systems for sharing of data, the message bus enables
this with the addition of hotplugging systems to this at any point in time without affecting any other system.

### Lamport Timestamps

In order to provide ordering on messages in distributed systems, since different nodes are not synchronized, Lamport
timestamps can be used to timestamp events with a simple and lightweight algorithm.

### Vector Clocks

Vector clocks are a more advanced algorithm for generating causal ordering (adhering to the HB relation) in which each
message contains the state of the logical clock of the sender. This is an array the size of the number of processes (an
index for each process) of clocks for the state of each process.

The difference between Lamport and Vector clocks is that Lamport is only a single value (single clock) whereas Vector
is a collection of clocks of all processes in the system.

### Flat Naming

It is vital for distributed systems to have some efficient naming scheme for the different systems/nodes, which is used
as their identifier or to determine their location and many more. With flat naming, the identifier is just a set of bits.

### Hierarchical Naming

With hierarchical naming the system or network is divided into subparts of one domain with a single top-level (root)
domain and ever expanding sub-domains until there are some leaf nodes.

### Domain Name System (DNS)

This is a hierarchical naming system for decentralized systems. It effectively implements efficient naming and allows
convenient partitioning, which in turn allows desired allocation, i.e. many nodes of the leaf domain and only a few
top-level domain nodes.

### Lightweight Directory Access Protocol (LDAP)

LDAP is a directory service similar to flat naming in order to provide lightweight directory naming by associating a
record in a directory entry with an associated attribute such that each record is represented as a (attribute, value)
pair.

## Lecture Notes

### Raft

**Safety property:** when there are no byzantine failures there should never be an incorrect result.  
**Availability:**  $n/2+1$ servers required to produce valid result.  
**No clocks:** Nodes do not depend on clocks.  
**Stragglers:** It can handle stragglers (nodes making process very slowly) if $N/2+1$ servers vote in the end.  

Leaders are elected for each term and each leader keeps track of the current term number, if election failed the term
has no leader.

### Replication

**Advantages:**

- Reliability/Redundancy
- geographical replication and more nodes to handle client requests

**Disadvantages:**

- Lower performance due to the need to maintain consistency across all servers (CAP theorem, atomic operations difficult
on many servers)
- Consistency also uses up resources (to manage consistency, i.e. SMR protocols)

Amdahl's Law that is given as

$$S(n)=\frac{1}{(1-P)+\frac{P}{n}}$$

stating that speedup of an application is a function of how parallelizable the application is, throwing unlimited resources
at it will at some point not lead to any performance gains.

Replication systems have _Leaders_ or _Masters_, which are the servers that take the client requests, and _Followers_, _slaves_,
or _Replicas_ that provide the read only data access. Systems can have a single leader (master-slave configuration), multiple
leaders (master-master configuration) opr be leaderless replication where all nodes are the same.

When the system is synchronous the writes need to be configured by a specified number of slaves before it can be considered
successful. In an asynchronous system the master can classify it as successful immediately and the slaves apply changes
when they are ready.

**Push-based** systems are where servers provide the modifications to the slaves, whereas **pull-based** is where clients
are asking for updates. **Leases** allow for some time the server will send updates but once the lease expires the
client will have to start pulling or renew the lease to get the updates from the master. Leases can be age-based, renewal-
frequency based (clients that request more often get longer leases), or overhead based (busy servers give shorter leases).

### Consistency

**A**tomicity: Updates are either done full or not at all.  
**C**onsistency: The system will always be in a valid state, transitions are only from valid to valid.  
**I**solation: Transactions do not interfere with each other.  
**D**urability: Modifications of successfully commited transactions are never lost.  

Two schedules are **Equivalence schedules** if they work on the same transactions and the conflict pairs are ordered in
the same way. A schedule is then considered to be correct if it is equivalent to a serializable schedule and hence is
also serializable. Serializabily differs from linearizability in that it works on multiple objects in arbitrary order
while linearizability works on a single object in real time order.

This can be achieved by using **Two-Phase Locking** which means that in order to participate in a transaction it needs
to acquire all necessary locks for that object (rule 1), and once a transaction releases a lock it cannot acquire any
more locks (rule 2).

In a distributed system this can be achieved with **Two-Phase Commit (2PC)** where the coordinator sends a _VOTE\_REQ_ to
all slaves, and they respond accordingly _VOTE\_COMMIT_ or _VOTE\_ABORT_ depending on if the slave succeeded with the transaction,
and if the coordinator receives a single abort he sends a _GLOBAL\_ABORT_ to all slaves to abort everything, otherwise if 
he receives no abort he sends a _GLOBAL\_COMMIT_ to all the slaves.
