---
title: "Scheduling Aperiodic Tasks"
date: 2022-02-19T14:33:05+01:00
publishdata: 2022-02-19T14:33:05+01:00
type: "notes"
mathjax: true
---

## Earliest Due Date (EDD)

With this algorithm, we select the task with the earliest __relative__ deadline to run first. However, the system assumes that all tasks arrive simultaneously, hence it knows all deadlines, and preemption is not used. The resulting performance is to minimize the maximum lateness.


### Optimality

![EDD Optimality 1](/images/IN4343/edd-optimality-1.png)]

![EDD Optimality 2](/images/IN4343/edd-optimality-2.png)]

Based on this, if we continue swapping, we end up with the plan that minimizes that maximum lateness. Overall, EDD takes $O(n\text{log}n)$ for queue ordering.

## Earliest Deadline First (EDF)

With this algorithm we use the earliest absolute deadline. Now the tasks can arrive at any time, and we use preemption (assuming no overheads for switching). It again minimizes the maximum lateness. So the schedule checks each time a new task arrives, if it should be scheduled.

Optimality is shown by turning a feasible schedule to an EDF schedule. For this we essentially just iterate through the tasks and check if that has the earliest deadline, if not we swap it with the task that has the earliest deadline.

![EDF Optimality](/images/IN4343/edf_optimality.png)

EDF requires $O(n)$ to insert a task in the queue.

## Non-Preemptive Scheduling

It becomes more difficult now if we cannot preempt tasks, but tasks arrive asynchronously. This is because if one task runs a longer time, and another with a short deadline arrives, the long running cannot be interrupted to have the other run, possibly causing the shorter one to miss its deadline.

Given the construction of all possible schedules in a tree, the complexity for finding a feasible solution is $O(nn!)$ with $n!$ leaves and $n$ depth of the tree.

To improve it we can prune the tree if a feasible schedule is found or the addition of a node to the current schedule causes deadline misses. However, this still leads to bad worst case time complexity.

Therefore, we can use a heuristical approach. This selects the step to take at any level by minimizing an optimization function (custom function that can put different weight on variables, e.g. constraints, and minimizes the resulting value). If the leaf contains a infeasible schedule, we go back one level and go to the second best option.

![Non-Preemptive Scheduling Optimization](/images/IN4343/np-schedule-optimization.png)

precedence constraints are then handled by assigning the appropriate weights to the heuristical function.

## Latest Deadline First (LDF)

This algorithm makes use of the precedence graph (DAG) based off which it constructs the schedule. It solves it in polynomial time on the number of tasks and minimizes the maximum lateness. It works by taking the DAG, constructing from the tail of it, and for tasks with no successor (or already selected ones) it takes the task with the latest deadline to execute last. Then, at runtime it takes the head of the queue to execute first.
Start with the leaf nodes that have no children (no task dependency beyond themselves) and place the ones with largest absolute deadline at the head of the queue. This constructs a heap where the tasks with dependencies and later deadlines are further in the heap. __Important here that first means first on heap, such that it is at the bottom and hence last to be executed__

![LDF](/images/IN4343/ldf.png)

## Scheduling with Precedence Constraints (EDF*)

With this algorithm, it is required to know the arrival times in advance, and then construct a task set that is equivalent but has no precedence constraints, on which we then apply EDF. This transformation requires to postpone the arrival time of successors and advance the deadlines of predecessors (not their real deadlines but the one the system uses to transform).

Transformations are as follows, where $k$ depicts the predecessors of a task.

![EDF Star](/images/IN4343/edf_star.png)

## Aperiodic Scheduling Algorithms Summary

![Aperiodic Scheduling Algorithms Overview](/images/IN4343/aperiodic_scheduling_overview.png)
