---
layout: post
title:  "Learning Spark"
date: 2015-06-12 11:50:13
categories: spark
---


## Spark Core
1. task scheduling
2. Memory management
3. fault recovery
4. interacting with storage systems

    RDD - Resilient Distributed Datasets

## Users

### Data scientist task
1. Interactive shell for ad-hoc query

### Data processing application

## How to start

{% highlight bash %}

# Master development branch
git clone git://github.com/apache/spark.git

# 1.4 maintenance branch with stability fixes on top of Spark 1.4.0
git clone git://github.com/apache/spark.git -b branch-1.4

# Building with build/mvn, if u r in China, what a pity u need VPN.
build/mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package

{% endhighlight %}
