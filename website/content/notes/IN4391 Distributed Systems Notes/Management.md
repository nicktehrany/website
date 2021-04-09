+++
date = "2021-02-22"
description = "IN4150 Distributed Systems Notes"
linktitle = ""
publisdate = "2021-02-22"
title = "Resource Management and Scheduling"
type = "notes"
mathjax = "true"
+++

## Reading Material Before Lecture

### Google Borg

Google Borg is a management system for clusters that run several hundreds of thousands of jobs from many different applications, on many different clusters with each having several thousands machines.

### Resource Management in Cloud

Clouds contain large quantities of shared resources that provide provide these to large pools of data and computing  with a variety of different interfaces. An important part of cloud computing is resource management, as there only are finite resources available.

### Slurm

Slurm is a cluster management and scheduling system for Linux clusters that are fault-tolerant and highly scalable, and it is open source.

### Energy Efficient Resource Management

In order to fulfill the needs of demand on computational resources, cloud providers have to implement energy efficient hardware and find the tradeoff between performance and energy efficiency, in order to not use enormous amounts of energy, while still meeting the quality of service (QoS) requirements.

## Lecture Notes

### Scheduling

The goal of scheduling is to achieve some form of mutual agreement between the resource providers (operators) and consumers (e.g clients). It also aims to maximize the resource utilization and hence will schedule workloads in a certain way.

The scheduling **mechanism** explains what to do, (i.e. use this policy for making a decision), and the scheduling **policy** explains how to do something (schedule with FIFO or randomly).

There are many different scheduling policies such as FIFO (does not always achieve minimal avg RT: average return time that is the addition of the return times of all tasks divided by the number of tasks), or shortest job first (suffers from starvation if a continuous stream of short jobs get scheduled when a long job is waiting to be run), or more advanced policies such as time slicing, which is used in OS, gives jobs a certain time slice and interrupts them once the time is done. Time slices can be assigned in round robin fashion. There are other techniques such as proportional share (jobs get resources proportional to what they require). Time slicing is efficient in OS but not for cluster scheduling.

The scheduler has the jobs to manage the environment, goals and other metrics for optimization, and based on the information schedule incoming workloads efficiently. Typically schedulers consume 5% of the cycles in datacenters (relatively low overhead considering the overhead of more advanced optimizations).

### Portfolio Scheduling

Since datacenters cannot work without a scheduler, here is proposed to use a set of schedulers, and apply some scheduler for some period, analyze its performance, and then use the next scheduler and repeat this.

### Types of Jobs in DS

- Parallel jobs: high-performance computing
- Bags-of-tasks: bag tasks together (e.g. parameter sweep computing (PSC), high throughput computing)
- Workflows: MapReduce, event streaming and applying filters

### Architecture for Distributed Management

The goal is to execute the full workloads while ensuring the service level agreement (SLA) and objectives (SLO) are met. The issues come from how resources are allocated, the complexity of the software that manages the architecture, load imbalance of different machines being used much more than others, having central points (single point of failure), and having the ability to scale significantly.

Single Cluster Architecture: Uses a single headnode and several nodes to which the headnode then assigns jobs (master and worker nodes).
