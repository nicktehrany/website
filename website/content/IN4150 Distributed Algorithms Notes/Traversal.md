+++
date = "2021-01-21"
description = "IN4150 Distributed Algorithms Notes"
linktitle = ""
publisdate = "2021-01-21"
title = "Traversal Algorithms"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

Information in a DS may need to be propagated from a single node to all other nodes and the originator may want to receive information back. In a _spanning tree_ of an undirected network, which is a sub-network of the original network that contains all nodes of the original network and that has the structure of a tree (the sparsest type of sub-network that keeps the system connected), messages need to traverse the tree efficiently. In all algorithms the originating node starts by sending a _TOKEN_ on any of its edges, which then traverses the whole system and returns to the originator. The node from which a node receives the token is its _parent_.

### Tarry's Traversal Algorithm in a General Undirected Network

Every node maintains the set of edges it has not yet sent the token to, except their parent. If that set is not empty when a node receives the token it sends it along one of the edges in the set, and otherwise sends it back to its parent.

**Message complexity** is $2|E|$ since the token travels along all edges twice.

### Cheung's Depth-First Search Algorithm

Every node that receives the _TOKEN_ for the first time forwards it to any of its unused edges, if there are any, and if no edge exists it sends it back to its parent. If a node receives the _TOKEN_ when it had already received it before it immediately returns it.

**Message complexity** is also $2|E|$.

### Awerbuch's Depth-First Search Algorithm

When a node receives the _TOKEN_ it sends a _VISITED_ message to all its neighbors, except the parent, stopping them from ever sending it the token again (removing it from the set of unvisited neighbors), and hence cutting down on unnecessary token sends to already visited nodes. The node then waits for an _ACK_ from all the neighbors (except the parent of course) before forwarding the token to an unvisited neighbor, or back to its parent if all neighbors are visited.

**Message complexity** is $2(|V|-1)$ ($V$ number of nodes). _VISITED_ and _ACK_ messages are bounded by $4|E|$ since max 2 of each can be sent along each link

### Cidon's Depth-First Search Algorithm

When a node receives the _TOKEN_ for the first time it immediately forwards it to a node it assumes has not been visited yet and sends a _VISITED_ message too all its neighbors except for that node and its parent. This means the token can be sent to a node that had already been visited (the visited message of that token might still be in transit), and once that visited message shows up the node knows it made a mistake, regenerates the token and sends it to another node. The already visited node that received the token does nothing with it.

**Message complexity** is $3|E|$ since visited message is sent both ways across any edge and at most one _TOKEN_ message can be sent along it.
