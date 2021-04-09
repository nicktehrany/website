---
date: "2021-02-29"
description: "IN4150 Distributed Systems Notes"
linktitle: ""
publisdate: "2021-02-29"
title: "Non-Functional Requirements"
mathjax: "true"
---

## Non-Functional Requirements

### Consistency

Client vs. data centric consistency, in the client side model there is a consistency model for every client but not a global consistency. From the perspective of the client there is no difference in the two models. When to use which model is a tradeoff decision again, therefore when you can afford it you want to use a weaker consistency model. The client consistency can be used if there is some notion of a client session.

Sequential vs. causal consistency, sequential consistency execution of everything is done in some sequential order but everyone applies in the same order that everyone agreed upon (it creates a total order of operations), in causal order you reason about which pairs of operations need to be in a certain order, causal is weaker than sequential.

Eventual consistency, things will converge to a consistent state if the writes stop at some point, since consistency is not immediate. Therefore the client can see states that are not directly consistent, as some writes may have not reached all the devices.

### Metrics

As most systems use a queue for storing tasks before they can be executed, queuing theory can be used for measuring the performance of a system with the arrival time, start time, end time, wait time, etc.

One of the modern metrics is elasticity which is the degree to which a system adapts to workload changes that adapting resources dynamically depending on the needs.

### Gustafson's Law of Weak Scaling

For any problem that we look at, we look at the relationship between smaller and larger jobs, the communication overhead is almost the same, therefore if we exploit more parallelsim for larger jobs, the sequential parts stay constant and additional resources are only used for larger lobs and the speedup comes from the sequential portion.

### Evaluating Performance Metrics

- Micro-Benchmark: test individual components of a system such as perf or stream
- (Micro-)Kernel Bechmark: benchmark hot functions of a system such as Berkeley dwarfs
- Synthetic workloads: approximate a model of the behavior of a system with made up scenarios that aim to represent real world workloads such as application skeletons
- Real-world workloads: Model the behavior of real word scenarios on systems such as TPC-DS
