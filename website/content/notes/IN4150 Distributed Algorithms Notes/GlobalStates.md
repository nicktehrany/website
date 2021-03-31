+++
date = "2021-01-15"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-15"
title = "Global States"
type = "notes"
mathjax = "true"
+++

Detecting of global states is the recording of an asynchronous system at some point in time for checkpointing or detecting _stable properties_ such as deadlock or termination.

A cut presents a set of internal events and can be considered consistent (when receiving events happens after sending events) or inconsistent (when receiving events happen without the send events).

![Cuts](/images/IN4150/GlobalStates.png)

### Chandy's and Lamport's algorithm for detecting global states in distributed systems with unidirectional FIFO channels

Any processor wishing to record the global state of the system first records its own local state and then sends a _marker_ on every outgoing channel. Upon the first receive of a marker along any channel, that process records the state of that channel as the empty state, records its own state and sends a marker along every outgoing channel, and creates an empty FIFO message buffer for each of its incoming channels (except the one on which the original marker came). For any later received markers on a channel, the state of that channel is recorded as the sequence of messages in the corresponding buffer. When a process receives a marker along every incoming channel it is done.

An event that happens local to some process before the process recorded its own state is called a _pre-recording_ event, and one that happens after it recorded its own state is called a _post-recording_ event.
