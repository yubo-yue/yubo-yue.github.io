---
layout: post
title:  "JVM"
date: 2015-09-18 13:16:13
categories: JVM Performance tuning
---
## Overview
JVM performance tunig

- JRE = JVM + JAVA API
- JVM = class loader, byte code verifier, GC, security Mgr, execution engine, JIT code generator

## Cheetsheet & reference
1. HotSpot-JVM-GC-options cheatsheet
http://blog.ragozin.info/2013/11/hotspot-jvm-garbage-collection-options.html
2. http://www.oracle.com/technetwork/java/javase/gc-tuning-6-140523.html
3. http://www.cubrid.org/blog/dev-platform/how-to-tune-java-garbage-collection/

## Tunig
- Memory tuning
- CPU usage tuning
- Lock contention tuning
- I/O tuning 

### Areas of memory performance tuning
- memory footprint tuning

    So u get OOM 
    1 may be you just have too much data
    2 may be you data representation is fat
    3 you can also have genuine memory leak


```
Too much data

Run with -verbosegc
Observe numbers in "Full GC" messages
[Full GC $before->$after($total), $time sec]

CAN u give JVM more memory?
Do u need all data in memory? consider using: 1. a LRU cache 2. soft reference
```


- allocation rate tuning
- garbage colleciton tuning



## Issues
### Latency
- the biggest contributor to latency is GC
- others : in-process locking, thread scheduling; IO; application algorithmic ineffiencies


