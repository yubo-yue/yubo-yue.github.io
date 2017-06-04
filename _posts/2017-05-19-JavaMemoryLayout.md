---
layout: post
title:  "Java Memory Layout"
date: 2017-05-19 23:00:00
categories: Java Memory MessageQueue
---

## Overview
This wiki is focusing on how java object lives in memory, that is, java object memory layout. Why I dive it too deep? Since we are going to implement a worker library, which will continously pull task from AWS service, and assign task to picked worker at free to execute. Since worker is a long running java process, we have to consider how memory is going to utilize, how GC will impact the performance. 

To better understand the problem we are going to solve, we first need to understand how JVM arrange and utilize memory. 

## Reference
1. [Java Object Memory Layout](http://psy-lob-saw.blogspot.com/2013/05/know-thy-java-object-memory-layout.html)
2. [JOL Java Object Layout](http://openjdk.java.net/projects/code-tools/jol/)
3. [Single Producer Consumer Lock Free Queue](http://psy-lob-saw.blogspot.com/2013/03/single-producerconsumer-lock-free-queue.html)
4. [Lock Free Queue Series](http://psy-lob-saw.blogspot.com/p/lock-free-queues.html)
5. [JVM Mechanics by Gil Tene](https://www.infoq.com/presentations/JVM-Mechanics)
6. [Martin Thompson](https://www.blogger.com/profile/15893849163924476586)
