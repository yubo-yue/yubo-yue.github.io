---
layout: post
title:  "Big Data 杂烩"
date: 2015-07-10 17:08:13
categories: BigData 
---
## BDAS - Berkeley Data Analytics Stack

- Data processing Layer
- Resource Management Layer
- Storage Layer


{% highlight bash %}

Spark Streaming | BlinkDB <- SharkSQL | GraphX | MLBase <- MLlib
Spark
Mesos
Tachon <- HDFS, S3

{% endhighlight %}

## Hadoop Stack

- Hive Pig <- Hadoop MR | Impala | Storm
- Hadoop YARN
- HDFS, S3
 
## Hadoop YARN

Make Hadoop Stack fit together with BDAS

## Apache Mesos

Enable multiple frameworks to share same cluster resources (eg Hadoop, Storm , Spark)

## Apache Spark

- Distributed Execution Engine
    1. Fault tolerant , efficient in-memory storage
    2. low-latency large-scale task scheduler
    3. Powerful prog model and APIs: Java, Python, Scala

    
## Fancy Terms, Technologies, Concepts
- Lambda Architecture [How to beat the cap theorem](http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html)
- Questioning Lambda Architecture [link](https://www.oreilly.com/ideas/questioning-the-lambda-architecture)
- Jay Krep's Kappa Architecture [link](http://milinda.pathirage.org/kappa-architecture.com/)
- Jay Krep's Log what every software engineer should know about real time datas unifying [link](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)

## References
1. http://datasys.cs.iit.edu/events/CCGrid2014/CCGrid-May25-Stoica.pdf - from BDAS, very good introduction material


