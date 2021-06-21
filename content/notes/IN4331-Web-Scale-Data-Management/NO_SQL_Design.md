---
date: "2021-04-29"
description: "IN4331 Web-Scale Data Management Notes"
linktitle: ""
publisdate: "2021-04-29"
title: "NoSQL - Design Principles"
type: "notes"
mathjax: "true"
---

## Replication

### Master-Slave Replication

All updates are made directly to the master which then propagates the changes to all the slaves.

### P2P Replication

Replication without a master node, now updates can be on any of the replicas which then propagate the updates to all the replicas.

## Distributed Transactions

### Two Phase Commit (2PC)

Transactions are submitted to a coordinator which then sends a prepare to all workers. The workers log the request and respond to the coordinator. When the coordinator receives a _ready_ from all the workers it sends a commit message to all workers which says that the workers should do all the work that was proposed in the prepare. In the prepare the workers just say if the request is possible. If any of the workers responds with a _failure_ the coordinator will stop the request and tell all the workers to abort via an _ABORT_ message and any preparation work by the worker will be stopped (clean the log).

### Linearizability vs. Serializability

In linearizability there is one correct order of operations on one element that all servers have to agree upon. With serializability there can be multiple orders of operations across servers, however the final state of the database is the same with any of the orders and is consistent to the operations that were run and expected.

### Base

- **B**asically **A**vailable
- **S**oft State
- **E**ventually Consistent
