+++
date = "2021-02-08"
description = "IN4150 Distributed Systems Notes"
linktitle = ""
publisdate = "2021-02-08"
title = "Introduction"
type = "notes"
mathjax = "true"
+++

What is the difference between parallel computing and distributed computing?

In parallel computing there typically is one job that is split into multiple tasks, which are then split up to different cores/threads that run simultaneously. Such systems typically are homogeneous, the hardware is all the same, i.e. the threads run in the same system on the same CPU.

In distributed computing there are also multiple tasks, but these can come from one job or possibly multiple jobs. These tasks run on heterogeneous hardware, different computers in different locations with different specifications, and hence requires some synchronization to keep the overall system in a legal state.

A distributed system is a system that consists of a colleciton of independent computers/servers that appear to the end user (client) as a single coherent system. A distributed system should make resources readily available and hide from the client that it is distributed (be transparent) and it should be scalable.

Problems arise due to the need for synchronization, which is achieved by message exchanging, which is far from perfect. Messages can be lost, corrupted, or delayed, thus the communication is asynchronous.

Distributed systems can run in a shared state, shared memory, or not in a shared state, where there is no notion of a common clock and all communication happens with messages. Distributed systems then have the following properties

- **C**onsistency: stating that there exists one copy of the data that is up-to-date
- **A**vailability: stating that it always has to be possible to modify or retrieve some data
- **P**artition Resilience: stating that if the network were to be interrupted, the system can handle this

An example of a distributed system is a distributed hash table (DHT), where individual nodes have finger tables such that the number of nodes to send a message to for finding the successor is in the order of $O(\text{log }n)$. If nodes fail, others can take over since data is replicated over different nodes. But this fails with network partitions as it would create two different rings.

Hence when designing distributed systems it is about finding the tradeoff of the three properties for the requirements of the system.

Another distributed system is a data cache that keeps time stamped information and a secondary engine then runs, which is permanently active, to retrieve the information from the system and update the cache.
