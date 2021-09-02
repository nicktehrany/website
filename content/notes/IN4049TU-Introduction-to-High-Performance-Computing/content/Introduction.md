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
