---
title: "Modeling"
date: 2022-02-11T14:01:01+01:00
publishdata: 2022-02-11T14:01:01+01:00
type: "notes"
mathjax: true
---

The goal of modeling is to

- abstract away the system components (hardware and software)
- establish assumptions, parameters for model description, variables to use, and understand the constraints on the system
- define the metrics upon which to optimize the system (performance/cost)

## Tasks

A task is defined as a sequence of instructions (e.g. the code to execute) which is run by the processor until it's finished. We then have the __activation time__ that depicts the time the task is created, the __start time__ for the time the task starts running, the __computation time__, and the __finishing time__.

Additionally, the __execution time__ is given as $f_i - s_i$ with finishing time $f_i$ and start time $s_i$ for the specific task. __response time__ is given as $f_i-a_i$ with $a_i$ arrival time for the task.

## Ready Queue

In concurrent systems there can be several tasks that are in the active state, but only one is actually being executed at any point in time. Therefore, such active tasks that are not currently running are in the __ready__ state. We then also have a __ready queue__ in which the tasks in a ready state are being queued. The scheduling policy then decides how to dispatch the tasks from the ready queue (e.g. FIFO, priority based, LIFO, etc.).

If the system supports __preemption__ tasks can be interrupted when another tasks with higher priority needs to be run (e.g. store task states, run higher priority one, restore prior task states once high priority one is done). However, this means that the task is placed again into the ready queue.

## Task States

Tasks can be in the __idle state__ or a __blocked state__ as well, where in the idle state they do not do any work and rather are waiting for the next period, and in the blocked state they are waiting for a signal (e.g. with shared memory wait to receive signal that it's safe to access the memory).


## Definitions

![Definition](/images/IN4343/deadline.png)

A RT task is then called __feasible__ when it completes before the absolute deadline $$f_i \leq d_i$ or $R-I \leq D_i$. __Schedulable__ then means that there exists an algorithm that can produce a feasible schedule.

We have additional timing constraints ![Timing Constraints](/images/IN4343/timing_constraints.png). Also worth mentioning is that jobs are task instances. A task specifies something to do and a job is then an instance of doing that task, meaning multiple jobs of a task can exist.

### Types of Tasks

- __Periodic Tasks__ These tasks are instantiated automatically every given time frame (their period). We can further classify __single rate__ when tasks in the system all have the exact same period, and __multi-rate__ where tasks have different periods.
- __Aperiodic Tasks__ These tasks are the opposite and happen at no given time interval but rather depending on things such as external events or interrupts. Aperiodic tasks can be further classified into __unbounded arrival time__ or __event based__ triggering of tasks and __sporadic tasks__ for which there is a minimum gap between arrivals (we don't know the exact arrival rate but at at least 8seconds elapse before any new task can arrive).

### Periodic Tasks

For periodic tasks we can then calculate the utilization as, with $C_i$ computation time and $T_i$ the task period

$$U_i=\frac{C_i}{T_i}$$

$\phi$ depicts the phase of the instance (its release time).

![phase](/images/IN4343/phase.png)

### Computation Time

We can estimate the computation time using experiments of prior runs and taking their runtimes, however this fails to account for all real world scenarios, therefore we have several different execution time estimations,

![Computation Time](/images/IN4343/computation_time.png)

Setting the safety margin factors in on the predictability (are we guaranteed to fall into the margin if it is large enough) and efficiency (if the margin is too large we are wasting resources).

### Jitter

The jitter measures the variation in time for a periodic event, given as

![Jitter](/images/IN4343/jitter.png)

### Timing Constraints

The timing constraints give bounds on the timings within the systems (e.g. minimum completion time, maximum allowed jitter, etc.). Often implicit timing constraints need to be met in order for the system to function correctly, whereas explicit timing constraints are specified for the system.


#### Minimum Sampling Period

Guarantees that the system is not overloaded using, where $U_{other}$ depicts the sum over all other tasks $\sum_{i \ne s}^{i} \frac{C_i}{T_i}$

![Minimum Sampling Period](/images/IN4343/msp.png)

### Maximum Sampling Period

Applies the same logic but rather uses the worst-case timings.

### Precedence Constraints

Often tasks need to run only after some other task has completed (they require that this other task has run prior to them running), giving us some precedence constraints on the tasks, which is represented using a directed acyclic graph. The graph represents immediate predecessors (need to run just before the task, e.g. task 3 can only run after task 2) and predecessors (need to run before but not directly before, e.g. task 1 needs to run before task 2 and task 2 needs to run before task 3, hence task 1 is predecessor to task 3).

Such graphs can then also represent __fork nodes__ points at which multiple tasks can start once a single task has completed and __join nodes__ which are tasks that require multiple tasks having completed. We also have __switch nodes__ that based on a decision have multiple tasks run (but not all their child nodes) and similarly __if-then nodes__ that have the same but just two child nodes of which one runs.
