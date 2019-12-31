---
layout: post
title:  " Data pipeline architecture "
---

# Introduction
About data platform, data pipeline. Also relates to data ownership organization, etc. 

# Data lake maturity model


# From Monolith centralized data lake to distributed data mesh
Frequently to see, it is centralized, monolithic and domain agnostic aka data lake.

The author proposes the the next enterprise data platform architecture is in the convergence of Distributed Domain Driven Architecture, Self-serve Platform Design, and Product Thinking with Data.

In order to decentralize the monolithic data platform, we need to reverse how we think about data, it's locality and ownership. Instead of flowing the data from domains into a centrally owned data lake or platform, domains need to host and serve their domain datasets in an easily consumable way.

This requires shifting our thinking from a push and ingest, traditionally through ETLs and more recently through event streams, to serving and pull model across all domains.


Proposed principles (tenets) for data mesh architecture:
* Serving over ingesting
* Discovering and using over extracting and loading
* Publishing events as streams over flowing data around via centralized pipelines
* Ecosystem of data products over centralized data platform

# Thinking about Big Data by Martin Fowler

"Big Data" has leapt rapidly into one of the most hyped terms in our industry, yet the hype should not blind people to the fact that this is a genuinely important shift about the role of data in the world. The amount, speed, and value of data sources is rapidly increasing. Data management has to change in five broad areas: extraction of data from a wider range of sources, changes to the logistics of data management with new database and integration approaches, the use of agile principles in running analytics projects, an emphasis on techniques for data interpretation to separate signal from noise, and the importance of well-designed visualization to make that signal more comprehensible. Summing up this means we don't need big analytics projects, instead we want the new data thinking to permeate our regular work.

# References

* The Data Dichotomy: Rethinking the way we treat Data and Service https://www.confluent.io/blog/data-dichotomy-rethinking-the-way-we-treat-data-and-services/


## Hadoop
HDFS, MapReduce, and YARN

* Resource Management Framework (YARN)
* Distribute computing framework (MapReduce)
* Cluster Storage Layer (like HDFS, HBase)
* Cluster Computing Layer (Yarn)
* Application on Cluster (MapReduce, Spark, Tez ....)
