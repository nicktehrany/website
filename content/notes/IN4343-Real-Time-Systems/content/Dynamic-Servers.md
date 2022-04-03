---
title: "Dynamic Servers"
date: 2022-04-03T18:36:29+02:00
publishdata: 2022-04-03T18:36:29+02:00
type: "notes"
mathjax: true
---

## Total Bandwidth Server (TBS)

It is designed to be used with EDF, where each aperiodic request is assigned a deadline and aperiodic jobs are inserted into the ready queue which is then served with EDF. Periodic tasks are directly going into the ready queue. Deadlines are assigned based on calculation with the computation time of the aperiodic job, and find the minimal period we can assign to the job (also account for the maximum possible utilization of the server). 

![Total Bandwidth Server Deadline Assignment](/images/IN4343/tbs_deadline.png)

## Constant Bandwidth Server (CBS)

It works similar to TBS by assigning for assigning of deadlines, but also keeps track of the execution for jobs with a budget, such that they cannot overrun their computation time. On overruns we postpone the deadline and reapply EDF.
