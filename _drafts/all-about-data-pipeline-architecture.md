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



# References
* [Data Monolith to Data Mesh](https://martinfowler.com/articles/data-monolith-to-mesh.html)