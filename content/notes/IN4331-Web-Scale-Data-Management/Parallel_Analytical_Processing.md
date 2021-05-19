---
date: "2021-05-11"
description: "IN4331 Web-Scale Data Management Notes"
linktitle: ""
publisdate: "2021-05-11"
title: "Parallel Analytical Processing"
type: "notes"
mathjax: "true"
---

### Relational DBs vs. Modern Big Data Processing Stack

Relational dbs such as SQL go through a stack that starts with the SQL compiler & optimizer and then executes them on a
relational dataflow engine, on top of the transaction manager, which then is on top of the storage and the OS. Big data
on the other hand, also have the SQL compiler & optimizer which then runs of top of different dataflow engines (Flink,
Spark, Hadoop). Essentially running a dataflow engine on top of a distributed file system.

### Parallelism in Databases

- **Inter-query Parallelism (concurrent queries):** Multiple queries can be run in parallel
- **Pipeline Parallelism (concurrent operators):** Multiple parts of a query plan can be executed in parallel
- **Data Parallelism:** Multiple cores/threads can work on the same operation at the same time but on different parts of
    the data

The maximum degree of data parallelism is the maximum number of possible data partitions.

### Forms of Data Partitioning

Due to **round-robin** partitioning, in which there is no grouping (then can only do filter operation, not join
aggregate or other operations) but it is random and balanced in size of each partition, therefore **hash partitioning**
can be used, which groups data by the hash (which can then be on some data that should be joined or aggregated by).
Another approach is **range partitioning** which splits the data by ranges (e.g. from 0-9 is 1 partition, 10-19 another
etc.).
