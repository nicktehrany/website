---
title: "Bigtable"
description: ""
date: 2021-05-19T16:03:09+02:00
publishdata: 2021-05-19T16:03:09+02:00
type: "notes"
mathjax: true
---

Bigtable is a high-performance proprietary database which is used by multiple google services (or was used, has several
newer versions mostly replacing it). It is meant for large throughput with very large files. Requirements are that
processes update continuously and asynchronously. Additionally, it is required to be fault tolerant, persistent, and can
use and work efficiently on cheap hardware, and scale well.


### Google File System (GFS)

A file system for very large files. Typical systems have block size of 4KiB (PageSize) and GFS has larger blocks of
minimally 64MB such that storing smaller files below 64MB is a waste of space and very large files can be stored much
more efficiently. GFS works with a master node and several replica nodes (to store each file 3 times). The master node
is there to maintain information on where blocks are located, and managing defragmentation and other file system jobs.
Additionally, GFS is an append-only file system, which is beneficial for google since files are rarely modified.
