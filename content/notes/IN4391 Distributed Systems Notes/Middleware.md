---
date: "2021-03-19"
description: "IN4150 Distributed Systems Notes"
linktitle: ""
publisdate: "2021-03-19"
title: "Middleware"
mathjax: "true"
---

Middleware is the part that goes between the OS (or lower level parts) and the applications, these define a set of protocols and APIs between the applications and the network or OS for services and communication. Java RMI is an example of this. Three levels of middleware exist:

1. Basic messaging
2. Programming primitives (such as RMI)
3. Full services (platform)

There are different classes of middleware; the remote-call oriented (RPC), the object-oriented (RMI), the service-oriented (CORBA, web services), and the message-oriented (pub/sub, message bus).

### CORBA

CORBA is a Common Object Request Broker Architecture, that handles interaction between components such as transactions, security, time, etc. across multiple servers that run the client stub for providing an interface to remote calls, and marshalling serialization with the CORBA library to make objects serializable, which all runs on top of the Object Request Broker (ORB).


### Types of Middleware

#### Reflection

This method can examine the properties of an object and possibly modify it.

#### Adaption

Adaption can change the behavior of the system, for example at compile-time with macros or at load-time with library interpositioning or at run-time with self modifying code.
