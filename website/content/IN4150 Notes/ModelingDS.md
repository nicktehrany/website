+++
date = "2021-01-11"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-10"
title = "Modeling Distributed Systems"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

DSs and DAs are modelled with a set of processors or processes, which do local computations and send and receive messages,
and are connected by unidirectional communication channels, and networks are assumed to be connected such that there is a
path from every process to every other process.

In a _complete network_ there is a link from every processor to every other processor.
In  _ring_ every processor is connected to two other processors. In a _unidirectional ring_ every processor can only
send messages to one of its neighbors, called the _downstream neighbor_ and receive messages from its other neighbor,
called the _upstream neighbor_.

Message passing is synchronous when the message delays are bounded.

Processors are synchronous when the ratios of their speeds are bounded.

In systems with _asynchronous communication_ the events of sending a message and receiving the same message are truly
separate events, with the latter occurring after the former (receive may be blocking).

In systems with _synchronous communication_ the two events of sending a message and receiving it occur logically simultaneously.

A synchronous system can be simulated as an asynchronous system by requiring explicit acknowledgements to force the sender
only to continue when the message has been received.

An asynchronous system can be simulated as a synchronous system by introducing for every link a process with a buffer that
may delay messages.

At any point in time a process is in a certain _state_ which is defined as the set of values of all its variables.
The state of a channel is the set of messages sent along it but not yet received.
A _global state_ or _configuration state_ is made up of the joint local states of all its processes and channels. A state
change is then called a _transition_, which are caused by _events_ in one or more processes.
There are three types of events:

- _Internal events_ in a process only causes its local state to be modified.
- _Message send events_ that modifies the state of one of the outgoing channels by adding a message to it.
- _Message receive events_ that modify the state of one the process' incoming channels by removing a message from it.

An _execution_ of an asynchronous DS is a finite or infinite sequence of events that start with all processes in one
of their initial states and all channels empty. The order in a sequence of events which constitutes an execution is a
_partial order_.

_Local simulation_ makes a process(or) in the system $(M',P)$ look like $M$, but to an outsider observer there may be a
difference. In _global simulation_ also to an outside observer there is no difference.
