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

## Dataset Discovery

Due to the heterogeneity in data lakes data scientists need to find relevant data from known data sets (find related data to the ones that the data scientist has). _Schema Matching_ is component for finding related data sets.

Tabular datasets can present relatedness which means that tables are joinable if there are overlapping columns which can be joined. Unionable tables store the same information but also have other data points, then extending data sets is just adding more data points on the same key from different datasets.

In order for finding overlaps in metrics the Jaccard similarity can be used which is given as for two domains $X$ and $Y$,

$$s(X,Y)=\frac{\lvert X \cap Y \rvert}{\lvert X \cup Y \rvert}$$

The higher the Jaccard similarity is the more similarities the two domains have, however it is biased for smaller datasets, as there the Jaccard similarity can more easily reach a higher value.
For resolving this issue we can use set containment, which is given as for two domains $X$ and $Y$,

$$t(X,Y)=\frac{\lvert X \cap Y \rvert}{\lvert X \rvert}$$

as this will ignore the size of the set, and only include the actual overlap.

Additionally, we have the set overlap given as,

$$o(X,Y)=\lvert X \cap Y \rvert$$

which is just the intersection of two sets and can be used for finding joinable tables.

### Dataset Discovery Optimizations

As this is very slow (due to having to do many calculations in data leaks), there have been several proposals for accelerating this procedure

#### LSH Ensemble

Use a Minwise hash function that will return the minimum hash value of all the hashes of a column, which can then be used to approximate the Jaccard similarity.

Additionally, there is Locality Sensitive Hashing which prunes the search space of the dataset by mapping signatures of domains to different buckets (not hashing all columns individually).

#### Lazo

Using One-Permutation Hashing (OPH) it improves on MinHash LSH by using a single hash function instead of one hash function per domain as in LSH.

#### JOSIE

Extracts the top most joinable tables given a query column and finding the data in the data lake with the highest overlap.

#### SilkMoth

Makes related set discovery faster by finding set similarity, set containment, and given a reference set $R$ it can find all related sets
