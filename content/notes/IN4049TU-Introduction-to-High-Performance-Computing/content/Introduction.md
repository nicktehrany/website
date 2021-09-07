---
title: "Introduction"
date: 2021-08-31T16:27:34+02:00
publishdata: 2021-08-31T16:27:34+02:00
type: "notes"
mathjax: true
---

#### Why do we need powerful computer systems?

Because more and more computation is moving to computer systems, experiments, proofs and other theoretical engineering
done traditionally on paper. Said engineering is too slow on paper and computationally limited, as well as dangerous
sometimes (in scenarios where simulation is needed, e.g. earthquakes, car crashes).

**Computational Science Triangle** presents the combination of computer systems, applications and algorithms, out of
which a model is then constructed. This model is then used to fulfill the computational science paradigm of the
previously mentioned theoretical engineering scenarios.

Three new kinds of HPC have emerged with the need for Big Data:

- **Volume** - large volumes of data being processed
- **Velocity** - new data is being generated quickly
- **Variety** - there are a large variety of data formats

Additionally, HPC systems require large amounts of memory, as the data is stored in memory for processing.

HPC is also widely used for web applications, such as web crawling, indexing, and sorting (e.g. with MapReduce).


#### Requirements of HPC

Modern HPC relies on:

- Powerful supercomputers (enough resources to do the computation)
- Parallelism in the problem (how far can the problem be parallelized?)
- Models and Simulation (how is the model formulated)

#### Principles of parallel computing

Parallel applications contain of multiple computational tasks which are run concurrently.
With parallel computing we want to use local data (in the caches) more, for better locality. The task size is also very
important (what parallelism granularity?), as well as scheduling and load balancing across the different processors and
tasks.

#### Performance Metrics

Important metrics with HPC:

- **Execution Time**
- **Speed-up** typically compared to sequential
- **Utilization** $time * \\#\text{CPU}$ in use

#### Parallelism

All programs have an intrinsically sequential part which cannot be parallelized. Therefore with Amdahl's law we can find
the maximal speedup of the program. However, the speedup is bound by the sequential part of the program. With $s$ being
the sequential part, $p$ the number of processors and $S$ the speedup, we have

$$S=T_{seq}/T_{par}=\frac{1}{s+\frac{(1-s)}{p}}\le \frac{1}{s}$$

where $1-s$ is the parallelizable part of the program.

##### Data Locality

Algorithms should be implemented to enable smart data distribution which results in best data locality for quick
accesses. If locality is not possible other techniques such as prefetching are used.
