+++
date = "2021-02-12"
description = "IN4150 Distributed Systems Notes"
linktitle = ""
publisdate = "2021-02-12"
title = "Functional Requirements"
type = "notes"
mathjax = "true"
markup = "mmark"
+++

### Remote Procedure Call (RPC)

RPC allows one program to call a procedure (subroutine or method) in another address space than its own, typically on
another system, without the programmer explicitly implementing this remote interaction but instead it is the same as
if a local process would invoke some method, except now it comes from a non-local process.

### Message Bus

Since distributed systems rely on message passing between different systems for sharing of data, the message bus enables
this with the addition of hotplugging systems to this at any point in time without affecting any other system.

### Lamport Timestamps

In order to provide ordering on messages in distributed systems, since different nodes are not synchronized, Lamport
timestamps can be used to timestamp events with a simple and lightweight algorithm.

### Vector Clocks

Vector clocks are a more advanced algorithm for generating causal ordering (adhering to the HB relation) in which each
message contains the state of the logical clock of the sender. This is an array the size of the number of processes (an
index for each process) of clocks for the state of each process.

### Flat Naming

It is vital for distributed systems to have some efficient naming scheme for the different systems/nodes, which is used
as their identifier or to determine their location and many more. With flat naming, the identifier is just a set of bits.

### Hierarchical Naming

With hierarchical naming the system or network is divided into subparts of one domain with a single top-level (root)
domain and ever expanding sub-domains until there are some leaf nodes.

### Domain Name System (DNS)

This is a hierarchical naming system for decentralized systems. It effectively implements efficient naming and allows
convenient partitioning, which in turn allows desired allocation, i.e. many nodes of the leaf domain and only a few
top-level domain nodes.

### Lightweight Directory Access Protocol (LDAP)

LDAP is a directory service similar to flat naming in order to provide lightweight directory naming by associating a
record in a directory entry with an associated attribute such that each record is represented as a (attribute, value)
pair.
