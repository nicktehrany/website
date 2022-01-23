---
title: "Machine Learning"
date: 2022-01-23T15:01:11+01:00
publishdata: 2022-01-23T15:01:11+01:00
type: "notes"
mathjax: true
---

- __Supervise Learning__: Given the model output to learn on (with the input of course)
- __Unsupervised Learning__: Not giving an output to the model, this is for example useful for figuring out clustering in the input data.

Then we have

- __Regression__: Predicting quantitative output or a continuous output
- __Classification__: Predicting non-numerical values, e.g. categories

## Parallelizing ML

### Data parallelism 

With this we distribute the input data across nodes. This can be done as batches in __batch parallelism__ where data is just split into smaller batches which are distributed, or as __domain parallelism__ where data points or features of samples are divided and distributed.

#### Batch Parallelism 

Here we have some master server that hands out data batches to workers and the workers send back their achieved gradients to the master. All can be synchronous or asynchronous.

This can also be done without a master server in a peer-to-peer network where gradients are gossiped for example.

### Model Parallelism

Here the neural network is distributed, such that the learning is split up. This can be done by distributing the weights of the network for example.
