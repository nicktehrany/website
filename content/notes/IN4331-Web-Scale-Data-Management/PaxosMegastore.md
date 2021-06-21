---
title: "Paxos Megastore"
date: 2021-05-25T17:05:50+02:00
publishdata: 2021-05-25T17:05:50+02:00
type: "notes"
mathjax: true
---

Paxos is a distributed systems consensus algorithm that can tolerate non-malicious failures. It provides safety that
only a value that has been proposed by some node will be the chosen one, only a single value is chosen, and only chosen
values are communicated further to all nodes. Liveness is provided by guaranteeing that some proposed value will
eventually be chosen, and that if a value has been chosen, every node will eventually know about this choice.

### Phase 1

The _prepare_ phase in which a proposer (which is acting as the leader) creates a proposal, with number _n_, and sends a
prepare request to all acceptors. The acceptors receive the request and if _n_ is the largest proposal they know of,
they accept and will not accept any proposal with a lower value by sending a _promise_.

### Phase 2

The _accept_ phase proceeds with if the proposer has enough promises from acceptors (from the quorum), it sets a value
for its proposal. It then sends an _accept_ message to all acceptors. The acceptors then accept the proposal iff it has
promised to accept this proposal. Acceptors can then send the learned value to all the learners (e.g. the clients).
