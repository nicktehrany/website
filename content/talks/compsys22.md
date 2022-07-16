+++
date = "2022-06-08"
description = ""
featuredalt = ""
featuredpath = "date"
linktitle = "compsys22"
title = "CompSys'22: Understanding NVMe Zoned Namespace (ZNS) Devices"
slug = "Compsys'22 Talk"
type = "talk"
aliases = "/talks/"
+++

### Abstract

The standardization of NVMe Zoned Namespaces (ZNS) in the NVMe 2.0 specification presents a unique new addition to storage devices. Unlike traditional SSDs, where the flash media management idiosyncrasies are hidden behind a flash translation layer (FTL) inside the device, ZNS devices push certain operations regarding data placement and garbage collection out from the device to the host. This allows the host to achieve more optimal data placement and predictable garbage collection overheads, along with lower device write amplification. Thus, additionally increasing flash media lifetime. As a result, ZNS devices are gaining significant attention in the research community.
    
However, with the current software stack there are numerous ways of integrating ZNS devices into a host system. In this work, we begin to systematically analyze the integration options, report on the current software support for ZNS devices in the Linux Kernel, and provide an initial set of performance measurements. Our main findings show that larger I/O sizes are required to saturate the ZNS device bandwidth, and configuration of the I/O scheduler can provide workload dependent performance gains, requiring careful consideration of ZNS integration and configuration depending on the application-workload and its access patterns.
Our dataset and code are available at \url{https://github.com/nicktehrany/ZNS-Study}.

### Slides

Slides are available [here](/misc/compsys22_talk.pdf)
