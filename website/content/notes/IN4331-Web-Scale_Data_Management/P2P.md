---
date: "2021-04-22"
description: "IN4331 Web-Scale Data Management Notes"
linktitle: ""
publisdate: "2021-04-22"
title: "P2P Storage"
type: "notes"
mathjax: "true"
---

Databases have some sort of model to represent the data in this model,

## Relational databases

Based on the relational model, it typically presented in tables that have some relation to each other among which tables can be joined, and includes indices.

### Application DB

The benefit of relational DBs is that they can easily be incorporated into multiple applications since they have defined schemas for the tables and a universal interface. Each application has its own data model and applications then communicate via some sort of messaging model. However now since each application has its own data model there is no unique model for all databases (no central data model as there wold be with integration DBs that integrate multiple applications into a single central database).

These types of databases are also easier to manage since there is no one central point where all data needs to go and there is no need for synchronization between the different points accessing the central point (easier to scale, distribute, and load balance).

## P2P Storage

The goal with P2P systems is to scale out (distribute the data) that also have resilience to failures, such that failing nodes can easily be handled (and replaced).

There are several different P2P systems:

- **Client-Server**: A central server that provides some service or content and clients directly connect to the central server.
- **Centralized P2P**: A central entity to provide a service that is used for indexing of data/groups of where data is located on other peers, and peers are connected to each other. They can request a location of some data from the central server and receive where the data is located and can request the data from that node. This poses a difficulty of load balancing of hot and cold data (what is hot? when is it how? how often replicate it?), and the system still relied on a central server for indexing.
- **Pure P2P**: All nodes are connected to each other and there is no single entity. This allows for leaving and joining to the system without any loss, however it is difficult to manage indexing and what data is on what node, since each node would need to broadcast the data it has to all other nodes to let them know that they have this data.
- **Hybrid P2P**: This aims to solve the problem of pure P2P by having super peers to which individual peers are connected and super peers are connected to each other. This allows for dynamic central entities that host indexing.
- **Pure P2P (DHT Based)**: Distributed hash tables (DHT) where searching and routing (and having information about other machines) can all be done in $\mathcal{O}(\log{}n)$. Can only be hashed by a single value, so if for example you want to share songs you need multiple DHTs to allow for hashing only by song, by song with artist, and so forth, so that songs can be found without all the information (valid existing hashes can be created without all the info). DHTs have a ring structure (which is where the $\log{}n$ comes from). Solving load balancing can be done by hashing yourself when joining, then have a resulting hash range and have yourself routed along the network to the range where you ended up in (this works since the hash function is uniformly distributed and have the avalanche effect). Finding a hash is done by each node maintaining information of $\log{}n$ other nodes in a _finger table_ where different distances to on the ring are stored and which nodes are responsible for that. The addresses you need are

    $$targedId = (currId + 2^i) \text{ mod } 2^m$$

    Routing requests for a node can then be directly forwarded to the node by sending it to the most distant known node which is below the value that is being looked up (always cutting the search space in half).
