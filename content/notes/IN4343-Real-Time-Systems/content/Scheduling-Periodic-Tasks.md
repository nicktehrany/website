---
title: "Scheduling Periodic Tasks"
date: 2022-02-22T13:49:26+01:00
publishdata: 2022-02-22T13:49:26+01:00
type: "notes"
mathjax: true
---

## Concepts

Again, tasks are defined with $\Gamma={T_1,T_2,...,T_n}$ and each task has a $C_I$ worst-case computation time, $T_i$ activation period, $D_i$ relative deadline, and $\Phi_i$ initial arrival time (phase).

### Hyperperiod

This depicts the minimum time period after which the schedule repeats. Therefore, it is the least common multiple of the periods of all the tasks.

### Schedulability

Feasibility is defined similar to aperiodic tasks, however each task is required to be able to be executed for its defined $C_i$ time in every interval $[a_{ik},d_{ik}],k=1,2,...$. 

Then we can have schedulability tests that check if a scheduling policy is schedulable for the given tasks and constraints. However, some of these tests are required to show that it is __certainly__ not schedulable, or other tests that show that it __certainly__ is schedulable. Hence, some of these tests are exact and give a direct answer to if it is schedulable or not, however not all tests necessarily are. 

For example, when testing if it is schedulabe for a single hyperperiod, is that enough to state that it is always schedulable?

### Processor Utilization

It is given as the fraction of processor time that is spent on a task,

$$U_i=\frac{C_i}{T_i}$$

Then the processor utilization is the sum over all task utilization.

![Processor Utilization](/images/IN4343/processor_utilization.png)

## Proportional Share Algorithm

With this algorithm time slots are of length that is the greatest common divisor (GCD) of all the tasks. Then each slot for a task is proportional to its utilization, meaning time slots are the utilization of the task ($\frac{C_I}{T_i}$) for the task multiplied by the calculated GCD. With $\delta$ being the GCD, we have time slot time being

$$\delta_i=U_i * \Delta$$

Then an algorithm is feasible if

$$\sum_i \delta_i \leq \Delta \text{ or } \sum_i U_i \leq 1$$

A drawback of this algorithm is that if $\Delta$ is small, there is a lot of context switching, hence possibly a lot of overheads introduced.

## Work-and-Sleep Algorithm

Here a task is running for $C_i$ units and suspends for $T_i - C_i$ units if another task with higher priority needs  to be executed. This is very easy to implement, however, its drawback is that it can starve low priority tasks.
