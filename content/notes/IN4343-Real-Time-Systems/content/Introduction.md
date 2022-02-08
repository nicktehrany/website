---
title: "Introduction"
date: 2022-02-08T10:22:07+01:00
publishdata: 2022-02-08T10:22:07+01:00
type: "notes"
mathjax: true
---

## What is a Real-Time system?

Real-Time systems are the systems that depend on their output (e.g. decisions from computations) and on the time in which these were produced. For example, airplanes will have many smaller systems that produce some result that is required to happen within some time frame (e.g. getting the airplane speed every couple of seconds). Therefore, the computations in Real-Time systems must be performed within a certain __timing constraint__. Additionally, the system has to provide _bounded response times_ for the computation under all possible scenarios, such that response times can be predicted for any possible input.

For real-time systems, often the computations are related to its environment (like the airplane example), and operations are adapted to this environment.

## Classes of Real-Time Tasks

- __Hard__ Real Time: the system has to generate a response before the deadline (e.g. safety critical systems).
- __Soft__ Real Time: the system does not have to meet the deadline all the time, some misses are fine (e.g. keyboard I/O can sometimes lag).
- __Firm__ Real Time: similar to Soft real time but now there is no benefit for completing computations after the deadline, results are useless after that point.

## Properties & Requirements of Real-Time Systems

- Often resources are limited, therefore high efficiency in resource management is required.
- If resources are shared, there has to be some temporal isolation to avoid tasks interfering.
- As these systems often interact with the environment, they have to have predictable results and required time to produce these.
- Tasks can also be very different, therefore the system has to be able to adapt and handle these, as well as being able to handle overloads at times.

## Classes of Control Systems

With real-time systems, as stated before, there is most commonly an object being controller. This object is then called the controlled object (the system), where the real-time system is the controller (doing the computations for changes), and the operator is the environment.

### Open-Loop Control System

In these systems the feedback from the actions are loosely dependent on the environment and there is no feedback from the controlled object to the environment.

### Closed-Loop Control System

In these, there exists frequent interaction between the system and the environment. Therefore, feedback is going from the environment to the system and from the system to the controller. This often happens at a sampling rate, depending on the use case.

### Multi-Level Feedback System

Here feedback is generated for different systems, of the larger system, at different rates. For example in the airplane, the airspeed may be collected every couple of seconds, and the altitude as well, but the autopilot checks all these and others every half a second.

## Taxonomy of Real-Time Systems

__Static__ analysis can be done at compile time, this implies that duration of tasks is know, and therefore we can tell what the Worst Case Execution Time (WCET) is. However, this is only possible for static systems, and very simple dynamic systems.

We can then also have systems that have a know task duration (either single or multiple durations but all known) which implies that it is a __periodic__ system. On the contrary, if we do not know this, it is called an __aperiodic__ or sporadic system. These create dynamic situations (e.g. autonomous cars).

