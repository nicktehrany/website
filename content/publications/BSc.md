+++
date = "2020-08-25"
description = "BSc Computer Science Thesis"
featuredalt = ""
featuredpath = "date"
linktitle = "thesis"
title = "Evaluating Performance Characteristics of the PMDK Persistent Memory Software Stack"
slug = "bsc-thesis"
type = "thesis"
aliases = "/thesis/"
+++

### Abstract

Society nowadays revolves more and more around data. Data Science, Machine Learning,
and Artificial Intelligence depend on large amounts of data to conduct research, and
train machine learning models and agents. With the ever increasing amount of data,
comes the need for faster storage. The quest for new storage devices has resulted in the
development of non-volatile memories, that run alongside conventional memory and are
directly accessible by the CPU, with large capacity of persistent storage. This new kind of
non-volatile memory is shifting storage technology in a new direction, triggering numerous
changes in the software stack on how we store and access data.  
One such change is the addition of the direct access (DAX) extension for file systems.
It allows using these devices as efficient storage devices with file systems. DAX eliminates
obstacles that conventional file systems present to non-volatile memories. Another such
change is the development of the Persistent Memory Development Kit (PMDK), a software
infrastructure, consisting of a number of libraries and tools for using these devices as
memories. The PMDK provides libraries for using non-volatile memory in a persistent
way, maintaining data through power loss, and in a volatile way, losing data on power
loss.  
We present an evaluation of the performance of the DAX extension, and provide one of
the first systematic studies analyzing PMDK software overheads. We conduct our study
using emulated non-volatile memory on DRAM, and all benchmarks we designed and
implemented, as well as function traces collected are open source. Our findings show that
DAX bypassing the page cache increases performance for I/O bandwidth by up to 32.85%,
showing that this new generation of file systems can outperform conventional file systems.
Additionally, we identify that the average cost of persistence with the PMDK is 3x higher
than volatile use, and provide guidelines for minimizing persistence software overheads.

[Full Thesis](/thesis/Nick_Tehrany_VU_BSc_Thesis.pdf)
