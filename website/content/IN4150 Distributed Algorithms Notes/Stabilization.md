+++
date = "2021-01-24"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-24"
title = "Stabilization"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

For dealing with transient faults, which means that components in a distributed system may only fail temporarily, _stabilizing_ algorithms have been devised that bring a system back into a correct sate from any incorrect sate it may be in.

A _predicate_, which is the defined order of saying what functions correctly and incorrectly and is defined as legal and illegal configurations, is called stable if once it holds and no faults occur, it continues to hold.

In systems with a _distributed demon_, it picks one process at a time to make a step.

### Stabilizing Mutual Exclusion Algorithms

#### Dijkstra’s Stabilizing Mutual-Exclusion Algorithm for a Unidirectional Ring in the Shared-Memory Model with a Central or a Distributed Demon

The network is a unidirectional ring with $N$ processes $P_{0},P\_1,...,P\_{N-1}$ which are in clockwise order (so $P_0\rightarrow P_1,...$). Each process maintains one variable, which is an integer modulo $K$ for some positive value $K$. Per step, a process reads the value of its predecessor and if it is not equal to their own value sets it to the value of the predecessor. When process $P_0$ finds that his value is equal to his predecessor, it assigns the value $v_0+1\text{ mod }K$ to its value and its successor will then adapt this value and so forth. This can be seen as whenever one process can change their value they can enter the CS, then change the value and the next process then does the same, like a token going around the ring.

Process $P_0$ is constantly introducing new numbers into the system up to $K$, which is called the _missing-label_ technique or _counter flushing_.

An illegal state in this system is when $P_0$ introduces a new value that is already present in the system.

### Stabilizing Datalink Algorithms

#### A Stabilizing Stop-and-Wait Datalink Algorithm

For every message a process sends, it waits for an acknowledgment before sending the next message. This is done by keeping a counter, which is sent along the message and incremented on receive and compared to the local send counter that is incremented on sending a new message (when the receive counter is greater or equal to the send counter). In essence every message has a message number and each process maintains a counter and sends the message number along with the message and waits for the response with that message number before sending any more messages. The system is in a legal state if all counters (in the sender, receiver, and the message) are the same.

#### Non-Stabilizing Sliding-Window Datalink Algorithm

There is a window size $w$ (represents the number of messages that have been sent but not yet received), and the sender maintains _ns_ and _na_ for the number of the next message to be sent and for the number of the next message to be acknowledged. The receiver has a counter _nr_ for the number of the next message to be received.

When the receiver sends an acknowledgment it sends its current value of _nr_ which means that it acknowledges the correct recept of messages up to but not including _nr_. Upon receiving an ack, the receiver will set its value of _na_ equal to the received value if the received value is larger than _na_. Upon a time out in the sender it resends any message sent but not yet acked. The algorithm should stabilize to

$$((na\le nr) \text{ and } (nr \le ns)\text{ and }(ns\le na+w)$$

$$\text{and}$$

$$\text{for  each }(message;i)\text{ in channel SR }(i,ns)$$

$$\text{and}$$

$$\text{for each }(ack;i)\text{ in channel RS }(i\le nr)$$

This states that the number of messages to be acked cannot exceed the number of the next message to be received, the number of the next message to be sent has to be at least equal to the number of the next message to be received, and the numbers of the next message to be sent and acked cannot differ by more than the widow size.

The second clause says that the number of the next message to be sent is larger than the numbers of the message on the channel.

The last clause says that no ack on the channel has a higher number than the number of the next message.

#### Stabilizing Sliding-Window Datalink Algorithm

The sender and the receiver maintain the same counters as before, but now messages sent by the sender carry two integer counters, the first for the message number and the second has the value of _na_ at the time of sending the message. This way when receiving the message, the receiver can catch up with the sender and send the ack that he is waiting for. When the sender times out, the counters _ns_ and _ma_ are made consistent if they are not in a way that indicates the channel is empty, otherwise any unacked messages are resent. The stabilization predicates are

$$((na\le nr) \text{ and } (nr \le ns)\text{ and }(ns\le na+w)$$

$$\text{and}$$

$$\text{for  each }(message;i,j)\text{ in channel SR }$$

$$(( i < ns) \text{ and }(j \le nr)\text{ and }(j < ns))$$

$$\text{and}$$

$$\text{for each }(ack;i)\text{ in channel RS }(i\le nr)$$

Only the second clause changed which now means that the number of the next message to be acked in the messages sent but not yet received cannot be higher than the number of the next message to be received, and it is strictly smaller than the number of the next message to be sent.
