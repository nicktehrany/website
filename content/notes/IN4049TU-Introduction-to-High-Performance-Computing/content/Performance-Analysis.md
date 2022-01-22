---
title: "Performance Analysis"
date: 2022-01-22T18:15:51+01:00
publishdata: 2022-01-22T18:15:51+01:00
type: "notes"
mathjax: true
---

### Metrics

We have the following

- $T_S$ is the Serial execution time, or the part that is done sequentially and how long that takes
- $T_P$ the parallel execution time,
- $T_O$ The overhead, given as %T_O = p * T_P - T_S$ with $p$ parallel processes. $T_O$ is equal to 0 in the optimal case.
- Speedup is given as $S=\frac{T_{serial_best}}{T_P}$

Then we have efficiency given as multiple equivalent options

$$E=\frac{S}{p}=\frac{T_S}{p * T_P}=\frac{1}{1+\frac{T_O}{T_S}}$$

Efficiency is simply just the achieved unit divided by the optimal unit (this gives the percentage of what you can maximally achieve).

We also have for simple performance measuring

- $T_S = number\_ops * t_{op}$
- $T_P = (number\_ops / p) * t_{op}+T_{comm}$
- $T_{comm} = number\_comm * t{comm}$ + an additional overhead for setting up communication each time

The __arithmetic intensity__ is given as the number of arithmetic floating point operations that are executed per byte of memory accessed. __Operational intensity__ similarly gives the number of operations per byte of memory accessed.

The __attainable performance__ is given as _min(Peak floating point performance, peak memory BW * operation intensity)_.
