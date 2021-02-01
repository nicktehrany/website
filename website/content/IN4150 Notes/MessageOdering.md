+++
date = "2021-01-14"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-14"
title = "Message Ordering"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

In asynchronous systems messages may have arbitrary but finite delays but the applications may impose some kind of ordering on the messages (_causal order_). _Multicasting_ is when a message is sent to group of processes rather than just to a single process.

### Ordering

A message order is **causal** when for every two messages $m_1$ and $m_2$, if $m(m_1)\rightarrow m(m_2)$ then $d_i(d_1)\rightarrow d_i(d_2)$ for all $i\in Dest(m_1)\cap Dest(m_2)$.

A message order is **total** when for every two messages $m_1$ and $m_2$, $d_i(m_1)\rightarrow d_i(m_2)$ iff $d_j(m_1)\rightarrow d_j(m_2)$ for all $i,j \in Dest(m_1) \cap Dest(m_1)$.

Therefore causal message ordering implies FIFO message ordering among separate channels, but total ordering does not imply causal ordering.

### Birman-Schiper-Stephenson Algorithm for Causal Message Ordering of Broadcast Messages

Every process maintains a vector logical clock $V$ and numbers its broadcast consecutively in its own component and sends the vector along with the message. If a receiving process finds that the message cannot yet be delivered, $D_j(m)=(V+e_j\ge V_m)$, stating that message $m$ is what the process expected (is up to date with respect of all other processes as $P_j$ is), and if not the message will be buffered.

### Schiper-Eggli-Sandoz Algorithm for Causal Message Ordering of Point-to-Point Messages

Processes keep vector logical clocks initialized to all zeroes for assigning timestamps to all events in the system, and every process maintains an initially empty buffer $S$ of ordered pairs of a process id and a vector timestamp. Whenever a process sends a message it sends the contents of the buffer along (basically sending its knowledge of the system). The receiving process can then check if it is up to date with the knowledge of the sender by seeing if its own id is in the buffer sent in the message. If its id sis not there it is not missing any messages and can deliver the message, and it will also update its own knowledge about the other processes by taking the maximum of its own buffer $S$ and the one in the message. If its id is in the message buffer, it compares its own vector clock to the buffer and if it is smaller, it is missing messages and has to buffer the message. After receiving a new message it can then check the buffer to see if now it can deliver any buffered message. For each receive, send, deliver event the local vector clock is also incremented.

### Total Ordering of Broadcast Messages

Very inefficiently total ordering can be achieved using a central component for total ordering with a special process (called _sequencer_) to which all processes send their broadcast messages. The special process gives each message a sequence number and broadcasts it to all processes.

Another algorithm uses FIFO links and scalar clocks with process ids as tie breakers (hence total order achieved). Every process also maintains a queue of non-delivered messages (but received, just buffered), which are ordered based on the timestamp. Every message is acknowledged to all other processes (including self). When a process has received an acknowledgment for the message at the head of its queue from every other process, that message is removed from the queue and is delivered.
