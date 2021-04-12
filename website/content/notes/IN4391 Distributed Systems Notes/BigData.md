---
date: "2021-03-26"
description: "IN4150 Distributed Systems Notes"
linktitle: ""
publisdate: "2021-03-26"
title: "Big Data and Distributed ML"
mathjax: "true"
---

Three different models of parallel computing:

1. Shared memory with multiple CPUs connected to a cache-coherent interconnect to multiple RAM modules, to which all have access.
2. Distributed memory with multiple CPUs all being connected to their own RAM module and a non-cache-coherent interconnect.
3. OpenMP where data is used where the computation is, which is the interface for multi-plaotform shared memory multiprocessing.

### Big Data

#### First Generation Distributed File Systems

The first open-source system is HDFS, which is a block based file system with built in replication and metadata journaling. It typically keeps 3 data nodes that receive the writes and the name node receives the addBlock that the client has added a block of data, after which then the data nodes confirm to the name node that they have received the new data.

MapReduce on the other hand works with map workers that apply some mapping function on the data, and the reduce worker that reduce the data of the mappers (e.g. summing counts of words). It first splits the input into chunks, which then go to the mapper. A possible optimization is to pre-reduce data during the map phase to save network bandwidth (this can be done if the function is commutative and associative such as adding to each element in an array individually). Lastly the data of the mappers is shuffled, since the reduce node depends on the data of all the mappers.

#### Second Generation Systems

Apache Spark is the next gen system that uses Resilient Distributed Datasets (RDD) of which partitions can be kept in memory, and it works on course granular transformations instead of fine grained ones. For this Spark has a master node that splits up the tasks and multiple executor nodes of which one is the driver. Executor nodes are then also used for replication of partitions of the RDDs. RDDs are partitions that are immutable (read-only). However also here the reducer has a wide depency (it depends on all the data of all the mappers). To solve this we can shuffle using a hash function that then maps the result of each map operation on a partition to the same result partition for the reducer. The problem with this is that there are a lot of files for each mapper writing its results to the same partition. To solve this a consolidate hash function is used that creates only a single file for a partition to which the mappers write their result, but this can generate random I/O instead of sequential I/O, which is bad for performance.

To solve this a sortShuffle is used in which partitions are append only maps and as soon as they fill up (in memory) data is spilled to a file. These files are sorted and indexed such that the reducer can then use some sorting algorithm (e.g. minHeapMerge) to reduce based on already sorted values. This way there is only a narrow dependency from the resulting partitions to the data partitions from the mappers. The group by key would still have a wide dependency, therefore Spark uses execution stages which end at shuffle boundaries and allow for full synchronization between stages. Stages themselves will be parallelized.

### Distributed ML

There are two kinds of parallelism in distributed ML, data parallel in which data is split and trained by the same model in parallel, and model parallel where different models are used on the same data to then combine the model in the end.

Distributed ML can be done centralized with different ML models combining results in some ensembling node which then gives the final trained model, or in a tree structure where data goes to every node, each node sends its gradient to the root of the tree (or its forwarded there across other nodes) and the root combines it and sends it down the tree.

The same can be done with a parameter server (just a kv-store) to which all nodes send their results and at which point the final trained model is computed. Or this can be done fully distributed in a P2P network where nodes inform each other about the training and train based on this.
