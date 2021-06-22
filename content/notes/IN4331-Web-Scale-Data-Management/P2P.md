---
date: "2021-04-24"
description: "IN4331 Web-Scale Data Management Notes"
linktitle: ""
publisdate: "2021-04-24"
title: "P2P Storage"
type: "notes"
mathjax: "true"
---

Databases have some sort of model to represent the data in this model,

## Relational databases

Based on the relational model, it is typically presented in tables that have some relation to each other among which tables can be joined, and include indices.

### Application DB

The benefit of relational DBs is that they can easily be incorporated into multiple applications since they have defined schemas for the tables and a universal interface. Each application has its own data model and applications then communicate via some sort of messaging model. However now since each application has its own data model there is no unique model for all databases (no central data model as there wold be with integration DBs that integrate multiple applications into a single central database).

These types of databases are also easier to manage since there is no one central point where all data needs to go and there is no need for synchronization between the different points accessing the central point (easier to scale, distribute, and load balance).

## P2P Storage

The goal with P2P systems is to scale out (distribute the data) that also have resilience to failures, such that failing nodes can easily be handled (and replaced).

There are several different P2P systems:

- **Client-Server (Not P2P!)**: A central server that provides some service or content and clients directly connect to the central server.
- **Centralized P2P**: A central entity to provide a service that is used for indexing of data/groups of where data is located on other peers, and peers are connected to each other. They can request a location of some data from the central server and receive where the data is located and can request the data from that node. This poses a difficulty of load balancing of hot and cold data (what is hot? when is it hot? how often replicate it?), and the system still relied on a central server for indexing.
- **Pure P2P**: All nodes are connected to each other and there is no single entity. This allows for leaving and joining to the system without any loss, however it is difficult to manage indexing and what data is on what node, since each node would need to broadcast the data it has to all other nodes to let them know that they have this data.
- **Hybrid P2P**: This aims to solve the problem of pure P2P by having super peers to which individual peers are connected and super peers are connected to each other. This allows for dynamic central entities that host indexing.
- **Pure P2P (DHT Based)**: Distributed hash tables (DHT) where searching and routing (and having information about other machines) can all be done in $\mathcal{O}(\log{}n)$. Can only be hashed by a single value, so if for example you want to share songs you need multiple DHTs to allow for hashing only by song, by song with artist, and so forth, so that songs can be found without all the information (valid existing hashes can be created without all the info). DHTs have a ring structure (which is where the $\log{}n$ comes from). Solving load balancing can be done by hashing yourself when joining, then have a resulting hash range and have yourself routed along the network to the range where you ended up in (this works since the hash function is uniformly distributed and has the avalanche effect). Finding a hash is done by each node maintaining information of $\log{}n$ other nodes in a _finger table_ where different distances to on the ring are stored and which nodes are responsible for that. The addresses you need are

    $$targedId = (currId + 2^i) \text{ mod } 2^m$$

    Routing requests for a node can then be directly forwarded to the node by sending it to the most distant known node which is below the value that is being looked up (always cutting the search space in half).

### Chord

Is a generic DHT interface implementation that uses a fixed-size hash space of $2^m-1$ which limits the maximum number
of peers and uses SHA1 for hashing (160 bits which is still a very large number). Nodes maintain a finger table with
nodes that increase exponentially, distances of $2^0,2^1.2^2,...,2^{m-1}$ which allows to always split the key space in
half on lookups. Finger tables store the distance, the hash ID of the target, and the node responsible for that ID.
(Function for constructing finger table is same as above). Routing on lookup is then done by going to the most distant
node ID in the finger table which is below the value looked for (check TargetID column). Then start routing from that
node again. Repeat until there is no smaller TargetID anymore.

Newly joining nodes just hash themselves, to obtain a new ID then contact necessary DHT nodes via discovery, contact the
new node that is responsible for the new node ID, split the arc responsibility, and construct the finger table.

### Amazon Dynamo

Assumes that there are no malicious nodes (no need for fraud detection), and implements a `put()` and `get()` for the storage system based on a DHT. In order to answer queries fast, each node maintains a full finger table (therefore the ring is fully connected) and there are no varying response times in the network. For load balancing each node also creates an additional virtual server so that if a node is under heavy load the virtual server (its partitions it is responsible for) can just be moved to a new node.

Updates (after `put()`) are propagated asynchronously, resulting in eventual consistency. It also uses read/write quorums such that the `put()` operation does not return until the majority of replicas have been updated (or however many nodes the quorum requires).
