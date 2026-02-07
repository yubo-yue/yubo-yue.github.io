---
layout: post
title:  "读Hadoop Application architecture随笔"
date: 2015-11-04 14:30
categories: Hadoop Ecosystem
---
## 前言
最近在Amazon上买了一本书，Hadoop Application Architecture, 在读的过程中有很多收获和理解，在次随笔记下以待后期参考和加深理解。

## Data Model


## Data Movement

## Data Processing

## Orchestration

### Candidates
1. Oozie (apache)
2. Azkaban
3. Luigi
4. Chronos (Mesos)

### How to choose workflow engine
1. Ease of installation
2. Community involvement and uptake
3. User interface support
4. Testing
5. Logs
6. Workflow management
7. Error handling

### Orchestration 之 Oozie 
- 主要概念：***Workflow Action***, ***Workflow***, ***Coordinator*** , ***Bundle***.

- workflow pattern: ***point-to-point***, ***fan-out(fork-join)***, ***Capture-and-Decide***

## Near-real-time Processing

### Candidates
***Spark Streaming***<br/>
***Storm*** <br />


### NRT(near real time) use cases
- Aggregation
- Windowing average
- Record level enrichment
- Record level alerting/validation
- Persistence of transient data
- Support for lambda architectures
- High-level function
- Integration with HDFS
- Integration with HBase

### Storm



## Lambda Architecture
From Nathan Marz and James Warren described in their book ***Big Data(Manning)*** .

    The Lambda Architecuture is designed to provide the capability to compute arbitrary functions on arbitrary data in real time. It archives this by splitting the architecture into 3 layers. 
    Batch Layer
        This layer store a master copy of the data set. This master data is an immutable copy of the raw data entering the system. The batch layer also precomputes the batch views, which are essentially precomputed queries over the data.
    Serving Layer
        This layer indexes the batch views, loads them, and makes them available for lower latency querying.
    Speed Layer
        This is essentially the real-time layer in the architecture. This layer creates views on data as it arrived in the system. 




