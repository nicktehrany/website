---
title: "Non-Preemptive Scheduling"
date: 2022-04-05T19:43:19+02:00
publishdata: 2022-04-05T19:43:19+02:00
type: "notes"
mathjax: true
---

While preemption allows to scheduler urgent tasks quicker, since we do not have to wait for other tasks to finish but rather directly scheduler the urgent task, it also has the disadvantage of added overhead for context switching. This added overhead can also cause more preemption, since we increase the execution time with the additional time required for context switching, and this can cause more preemptions to happen during that prolonged time. We also use cache locality, since it's replaced with the new task and again when the task is preempted, these are called cache-related preemption delays (CRPD). Therefore, with preemption we have less predictable WCET (worst case execution time) due to all the overheads.

Without preemption we could also have the disadvantage that it could reduce the schedulability due to blocking.

## Hybrid NP Schemes

### Preemption Threshold

With this only tasks with a very high priority can preempt tasks with a low priority, by defining a minimum priority threshold for every task stating the tasks it can preempt.

### Deferred Preemption

We define for each task the longest time period during its execution that it cannot preempted.

### Task Splitting (or Fixed Preemption Points)

We split the tasks into non-preemptable code chunks, which preemption can then only happen in specific code sections.
