---
title: "Fixed Priority Servers"
date: 2022-04-03T16:50:10+02:00
publishdata: 2022-04-03T16:50:10+02:00
type: "notes"
mathjax: true
---

## Hybrid Task Sets

These task sets contain both periodic and aperiodic tasks, for example aperiodic tasks arising from exceptions or interrupts. The periodic tasks often have hard deadlines, compared to the aperiodic ones with soft deadlines.

The goals of scheduling in such a system are to:

1. Minimize the response times for the aperiodic tasks, requiring to process them as soon as they arrive. 
2. Period tasks should be executed before their deadline such that they do not miss deadlines because of the aperiodic tasks.

### Opportunistic Scheduling

Often also referred to as background scheduling, we execute the aperiodic job when the CPU is idle from running periodic tasks.

This gives good performance for periodic tasks, but response time for aperiodic jobs is bad.

On the other hand prioritizing the aperiodic job could easily cause the periodic task to miss its deadline, also making it an undesirable scheduling approach for hybrid task sets.

## Server Concept

A server is responsible for controlling the execution of aperiodic tasks, it itself is a process that is run as a periodic task that when active schedules the ready aperiodic tasks. For this it uses a *service queue* of aperiodic tasks, from which it gets tasks and places them in the *ready queue* whenever it runs. In addition the periodic tasks are going directly into the ready queue.

For all the servers, we assume that scheduling is using RM for the periodic tasks.

### Polling Server

Given a period $T_S$ and a computation budget $C_S$ the server at the beginning of each period initializes the budget and over the time of the period consumes this budget as aperiodic tasks are executed. The execution of jobs will end when the budget reaches 0. When no aperiodic tasks are there, the budget is set to 0 and the token returns to the periodic tasks.

![Polling Server Schedulability](/images/IN4343/PS_schedulability.png)

In order to find the parameters of the polling server (computation time and period) we can find the bounds that still ensure feasibility.

![Polling Server Dimensioning](/images/IN4343/ps_dimensioning.png)

### Deferrable Server

This server is the same as the polling server, but if there are no pending jobs the budget is not set to 0 (it stays). This means that when an aperiodic task arrives, it can start immediately, hence there are lower delays with them. While this helps with the response times of the aperiodic tasks, it can produce deadline misses for the periodic tasks. This is due to the possibility that computation is deferred, which means treating it as a periodic task is not valid (therefore it does not actually violate the feasibility of RM since it is using RM and can violate schedulability with deadline misses).

## Slack Stealer

This approach uses a passive task that attempts to create a time budget for aperiodic tasks by taking time from periodic tasks. As there is no benefit to finish a periodic task before its deadline, we can steal the free time for it and push it as far as possible to finish.
