---
title: "Modeling Part 2"
date: 2022-02-15T16:29:20+01:00
publishdata: 2022-02-15T16:29:20+01:00
type: "notes"
mathjax: true
---

## Scheduling Problem

Given a set of tasks, a set of processors, a set of resources, and a performance objective, the goal is to find assignments for the processors and resources to the tasks such that it results in a __maximum-value__ schedule under the constraints.

Common definitions used are $\sigma$ for the schedule and $\Gamma$ for the tasks, and $A$ for the algorithm that generates the schedule.

The scheduling problem is shown to be NP-Hard (non-deterministic polynomial), meaning that finding a feasible schedule grows exponentially with the number of tasks for the schedule.

## Computational Complexity

We are mainly interested in the execution time that the algorithm needs to finish, including doing the computations. Therefore, we take the worst case execution time of a larger problem (since these show worst case much better than small problems) and compare using the Big-O notation. Then an algorithm is in __polynomial time__ if it has a time complexity of the number of tasks to some exponent, specifically $T(n)=O(n^a), a>1$.

## Taxonomy of Algorithms

### Preemptive vs. Non-Preemptive

These are based on if the tasks can be interrupted (and restored or restarted afterwards).

### Static vs. Dynamic

Static scheduling is based on fixed parameters that are assigned to the tasks prior to the creation of a task. Dynamic on the other hand is where these parameters might change at runtime.

### Online vs. Offline

With online scheduling the schedule is created in the beginning or at the arrival of a new task. Offline scheduling uses a pre-determined table driven schedule, which we can only run if we know what tasks will arrive in the future.

### Optimal vs. Heuristic

Optimal maximizes the value of the performance criteria. Heuristic uses a heuristical approach to maximize, however this will not be optimal.

### Admission Control

This tells a task whether it is accepted or rejected. This is necessary to avoid the domino effect of failed jobs. For instance, we have a job that is running, it gets interrupted by a higher priority one, this happens again a couple of times until we have one job that finishes before its deadline. Then continuing the interrupted jobs (in order of highest priority to lowest) will often mean that each of them will finish after their deadline.

![Admission Control](/images/IN4343/admission_control.png)

## Optimal Criteria

Upon what criteria do we optimize? We need to ensure the schedule is feasible (if there is one), minimize the maximum lateness or deadline misses, maximize the cumulative value of the tasks run, and include the cost for consuming resources.

![Optimal Criteria](/images/IN4343/optimal_criteria.png)

## Graham's Notation

This notation is used to systematically describe the problem and algorithm.

![Graham's Notion](/images/IN4343/graham_notion.png)
