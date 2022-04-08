---
title: "Handling Overloads"
date: 2022-04-08T11:47:49+02:00
publishdata: 2022-04-08T11:47:49+02:00
type: "notes"
mathjax: true
---

When the computation load that we have on the system exceeds the available capacity.

We can measure intensity of soft aperiodic tasks as $p=\lambda C~$ with $p$ traffic intensity, $\lambda$ the average job arrival rate and $C~$ the average service time. However, this is not good enough for RTS with hard time constraints.

**Transient Overload** is where on average we have a load smaller than 1 and a maximum over 1, meaning on average we are fine and sometimes we are overloaded.

**Permanent Overload** is where the average is above 1.

## Handling Transient Overloads

We can use Resource Reservation (RR) to reserve a fraction of the CPU for some task.
