---
title: "Parallel and Distributed Architecture"
date: 2021-09-07T16:56:12+02:00
publishdata: 2021-09-07T16:56:12+02:00
type: "notes"
mathjax: true
---

#### Memory Classifications

Based on memory organization:

- **SM** - Shared Memory
- **DM** - Distributed Memory

Based on access times:

- **UMA** - Uniform Memory Access
- **NUMA** - Non-Uniform Memory Access
- **SMP** - Symmetric Multi-Processor

Based on data and control flows with **Flynn's taxonomy**:

- **SISD** - Single Instruction Single Data
- **MISD** - Multiple Instruction Single Data
- **SIMD** - Single Instruction Multiple Data
- **MIMD** - Multiple Instruction Multiple Data

Extensions of memory systems:

- Shared cache, where processors share caches and memory.
- Bus-based shared memory (Symmetric Multiprocessors - SMP), where processors share the same memory and bus to it.

