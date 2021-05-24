---
title: "Streams"
date: 2021-05-24T18:22:43+02:00
publishdata: 2021-05-24T18:22:43+02:00
type: "notes"
mathjax: true
---

Data can be static (it does not change, is written once and never updated), therefore most commonly used is streaming
data, which changes (changes at some point in time, does not have to be frequently or soon but it will eventually
change) and is updated. A data stream is a stream of events with each element having a timestamp.

Types of data:

- **Bounded data:** a dataset with a beginning and an end.
- **Unbounded data:** a dataset without an end or an end.

Types of processing:

- **Stream processing:** continuous stream of processing that continuously produces new results, systems such as Apache
    Flink or Apache Storm
- ** Batch processing:** processing which takes a finite amount of time to complete and for it to produce results,
    systems such as Apache Hadoop MapReduce and Apache Spark

### Stream Processing

**Streaming window** is a subset of a stream, such as the average value for a stream within a certain time frame, e.g.
10-12pm and another average for 9-11pm, which is then a _sliding window_ since they overlap. Another type of stream
windows is a _fixed/tumbling_ window which is consecutive windows, e.g. from 9-10pm and then 10-11pm. Lastly, there are
_jumping windows_ which just skip parts of the stream, e.g. from 4-5pm and then 9-10pm.

Session windows for users are then seen as a period of activity for a user followed by a period of inactivity, on which
processing can then be done.

### Definitions

- **Processing time:** the time measured by the machine that executes the operator
- **Event time:** the timestamp of an event given in its data record
- **Ingress/Ingestion time:** the timestap of the first operator in the program that sees this event
