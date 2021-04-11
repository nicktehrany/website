---
date: "2021-03-12"
description: "IN4150 Distributed Systems Notes"
linktitle: ""
publisdate: "2021-03-12"
title: "Programming Models and System Architecture"
mathjax: "true"
---

## Design Elements of a Distributed System

- The Architecture, is it client-server, hierarchical, peer-to-peer, etc.
- The programming model that is used, is it remote procedure call (RPC), library based, actor model based (akka), mapreduce, etc.

Tradeoffs between the different programming modles and the sotware architecture have to be made, based on the function/non-functional requirements, the cost of convenience, which is easier to use based on the hardware for example, and which is needed for a large scale system.

### Programming Model

The programming model is the abstraction of the underlying hardware that forms the execution environment.

These models are often based on multiple abstractions such as the model of computation, communication, or data.

Certain programming models exist for parallel and distributed systems, such as pthread. Different models then offer different functions, one might be synchronous while another is asynchronous.

MPI (message passing interface) is another programming model that offers message passing on point-to-point links or broadcasting.

#### Middleware

Middleware is the part that goes between the OS (or lower level parts) and the applications, these define a set of protocols and APIs between the applications and the network or OS for services and communication. Java RMI is an example of this. Three levels of middleware exist:

1. Basic messaging
2. Programming primitives (such as RMI)
3. Full services (platform)

#### Interceptors

Interceptors are software that break the usual execution flow to allow some other application code to be executed. This allows for adaptation of middleware by "injecting" some additional custom code.

#### Tuple Spaces

Tuple spaces is another form of a programming model which offers a form of distributed shared memory that is paired with synchronization mechanisms.

#### Actors

Actors is another well know model where only an actor can access their internal state and messages can be sent (asynchronously) to other actors. Akka is the Scala example of such a model.

### System Architecture

Pthread implies an implicit system model of MIMD (Multiple Instruction Multiple Data), which is shared memory such that threads can use the same memory.

In large scale systems failures are the norm and will happen.
Message passing with RPC is very tough at a large scale (unless there is a minimal known amount of message passing that only happens between certain edges).

The goal is to restrict the programming modes so that the system can do more automatically (scheduling, data distribution, fault-tolerance, etc.).

Sometimes it is necessary for self adapting systems that change based on some monitoring. Such systems are organized with a feedback-control system and are called  _autonomic computing systems_.

#### Different System Architectures

Many different architectures exist:

- Star (centralized): such as clients only communicating with servers but not with other clients. Or Spark nodes only communicating with the master (though sometimes nodes can communicate with each other)
- Hierarchical: DNS is the best example for this, and provides a quicker way to send messages. Imagine peer-to-peer for looking up an IP you would have to possible ask everyone, but with DNS you can find it through three traversal from the root down.
- Super-peers: peers are connected to other peers that have some additional responsiblity (hence super), and these super nodes are then connected to other super nodes. This replaces the need for servers, as it was used by Skype earlier.
- All-peers (decentralized): BitTorrent or other peer-to-peer networks.

In most cases servers are organized in three tiers, the logical switch (can have multiple servers) that accepts client requests, the application/compute servers that do the actual processing of the request, the the distributed file/database system for storing the data. The logical switch will dispatch the requests to worker servers which use the database systems for retrieving of data.
