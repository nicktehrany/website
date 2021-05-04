---
date: "2021-05-04"
description: "IN4331 Web-Scale Data Management Notes"
linktitle: ""
publisdate: "2021-05-04"
title: "Scalable Data Integration and Discovery in Data Lakes"
type: "notes"
mathjax: "true"
---

## Data Lakes

The problem starts in the management of big data, which has the following requirements:

- **Volume:** the data is typically large
- **Variety:** the type of the data varies
- **Velocity:** the data needs to be accessed and processed quickly
- Veracity: the quality of the data needs to be maintained and guaranteed
- Value: the data is used for intended purposes and holds value to that purpose

Data lakes are a way of storing large amounts of data without strict structure and guidelines, and all data is just "dumped" there. However, this can quickly lead to data swamps, therefore the data lake is more of a set of centralized repositories of raw data, which can be structure or unstructured. There must be metadata to describe and organize the data and be able to quicky query or process the data.

## Data Integration

Data integration is a unified and transparent way of providing access to the raw data in a data lake or other data sources. These data sources can be heterogeneous (structure, content, sources, etc.). There are two types of architectures for data integration:

1. **Data Warehouse:** a single data store where the data from different sources is stored
2. **Virtual Integration:** queries are applied online after query reformulation on the mediated data schema. This way the data does not need to leave the database.
