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

## Timeline (Cyclic) Scheduling

The total time is divided into equal length slots, where we define a small cycle $\delta$ equal to he GCD, and a large cycle $T$ equal to the LCM (largest common multiple). Tasks are then placed into this smaller and larger periods, since we define the smallest period $\delta$ in which the most frequent task always needs to appear once, and apply the same for larger cycles. The schedule is then feasible if the sum of the computation times of the necessary tasks is less than or equal to $\delta$ (we only car about the required conditions, e.g. if task $A$ and $C$ never happen in a minor cycle together, we don't care if their sum is less than $\delta$).

Advantages are that it is easy to implement and schedules can be hard coded in the main loop, meaning tasks will have consistently low jitter. However, it cannot handle overloading and is sensitive to application changes.

## Priority Scheduling

We assign priorities to the tasks based on their timing constraints (their deadline). There are different ways of assigning priority to a task given as,

- **Rate Monotonic (RM)**: $P_i \frac{1}{T_i}$ for task $i$ and $T_i$ the period of the task. Hence, it is inversely proportional to the period, the larger the period, the lower its priority is.
- **Deadline Monotonic (DM):** $P_i \frac{1}{D_i}$ for task $i$ and $D_i$ the relative deadline of the task.
- **Earliest Deadline First (EDF):** $P_i \frac{1}{d_{ik}$ for task $i$ and $d_{ik}$ the absolute deadline of the task.

### Critical Instant

The critical instant of a task is the arrival time that induces the largest response time. It is essentially the worst case scenario when the task arrives concurrently with all higher priority tasks. If all tasks are schedulable in their critical instant, then the schedule is feasible.

## Least Upper Bound (LUB)

Given all possible task sets if we modify the computation times of the task, we find the upper bound of a task set (utilization that cannot get any higher, and which can be lower than 1 but never above 1), then one of these utilizations will give us a lower bound (lowers possible utilization). Then given a task set that has a utilization below this LUB we know that it is schedulable.

### RM Algorithm

Since we don't want to do the calculation for finding the LUB for all scheduling algorithms and all possible task combinations, we have an algorithm to analytically quantify the LUB. Given any task set, for scheduling the LUB with RM

$$U_{lub}^{RM}=n(2^{\frac{1}{n}}-1), n\rightarrow \infty \implies \text{ln}2=0.69$$

### RM Test with Hypberbolic Bound (HB)

It says that given a task set $\Gamma$ of $n$ periodic tasks, and each task has processor utilization $\tau_i$ it is schedulable with RM if

$$\prod_{i=1}^{n}(U_i+1)\leq 2$$

### Comparison

HB gives a tighter bound than LL.

![LL compared to HB](/images/IN4343/RM.png)
