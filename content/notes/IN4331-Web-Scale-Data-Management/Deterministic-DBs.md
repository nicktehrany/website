---
title: "Deterministic DBs"
date: 2021-06-21T13:25:36+02:00
publishdata: 2021-06-21T13:25:36+02:00
type: "notes"
mathjax: true
---

Calvin is a transactional scheduling and  data replication layer that runs alongside non-transactional storage systems.
It uses a deterministic locking protocol (DPL) which eliminates the need of distributed commit protocols (e.g. 2PC), it
also uses a shared nothing architecture. The model it uses is a deterministic database, which works by knowing the exact
read and write sets (which keys are going to be accessed) before starting the transaction. Then if we know what replicas
a transaction will touch, the individual operations can be sent to the replica individually and be serialized that way.
Therefore, clients will contact a sequencer which will create the order of operations for the transactions. If there are
multiple sequencers, the sequencers will individually use Paxos to decide on the order (if clients send requests to two
different sequencers for example), and sequencers in Calvin will talk to the scheduler every 10ms to schedule a batch of
operations. The order of operations is created by the sequencer, it will pass this to the scheduler, which then applies
additionally needed procedures, such as 2PL (2 phase locking) if it is needed by the order of operations to be safe.
