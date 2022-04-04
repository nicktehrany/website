---
title: "Jitter"
date: 2022-04-04T16:33:40+02:00
publishdata: 2022-04-04T16:33:40+02:00
type: "notes"
mathjax: true
---

## Jitter Analysis

This is typically done for multi-processor systems but can also be used for single processor ones. Defines the **Finalization Jitter ($FJ$)** as the variation in finalization times, and the **Activation Jitter ($AJ$)** as the variation in activation times. The goal of jitter analysis is then to determine the schedulability in the context of jitter and with it be able to determine response times. We then look at the worst case response time and the best case response time, which depict the response time that tasks should fall in between, and not outside of.

An (a task can have multiple) **Optimal Instant** of a task assumes its best case response time and appears when the lowest amount of preemption by higher priority tasks appears.
