---
title: "Google Spanner: Google's Globally Distributed Database"
date: 2021-06-09T14:52:13+02:00
publishdata: 2021-06-09T14:52:13+02:00
type: "notes"
mathjax: true
---

Spanner provides a unique feature of providing distributed transactions without locks, through the property of Spanner
that distributed transactions are externally consistent. Instead of locking the database for reads to get the data (all
writes are locked), it works by generating a snapshot of the database from which the reads happen (writes can then still
happen on the original database), however now snapshots need to be synchronized across the distributed shards of the
database. Additionally, it uses transactions with strict 2PL (2 phase locking), where each transaction $T$ gets a
timestamp $s$ and data written by transaction $T$ is then timestamped with $s$.

### Synchronized Snapshots

In order to provide synchronized snapshots, Spanner needs to have some notion of a global clock, which it achieves
through external consistency. This is the property of the commit order respecting the global time order. For achieving
this, timestamp order is used as the commit order (timestamp\_order == commit\_order). For generating a timestamp, it
uses TrueTime.

### TrueTime

Presents a global time with a bounded uncertainty given `TT.now()` is given in a window with and `earliest` and
`lastest` time which falls in the interval of $2*\varepsilon$. Then picking a time `TT.now().latest` which is an upper
bound on time in the future gives that timestamp $s$ falls within the interval of the releasing of locks. This is done
by waiting out the uncertainty in clocks (waiting for actual releasing of clocks), called the _commit wait_, which in
turn is done by waiting until `TT.now().earliest` > $s$. Therefore the average commit wait is roughly $2*\varepsilon$.
