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
 ({{ site.url }}/assets/mongo-sharding-arch.jpg)
2. http://www.oracle.com/technetwork/java/javase/gc-tuning-6-140523.html
3. http://www.cubrid.org/blog/dev-platform/how-to-tune-java-garbage-collection/
4. netty - http://normanmaurer.me/presentations/2013-wjax-netty/#/35

## Tunig
- Memory tuning
- CPU usage tuning
- Lock contention tuning
- I/O tuning 

### Areas of memory performance tuning
- Memory footprint tuning

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


## Find which thread in JAVA jvm use the most cpu resource

```bash
//find which java process , which thread in java process use the most cpu resources.
ps -ef -M //this is for mac, need check when play with RHEL or other linux distributions, JVM versions.

// find where code is consuming a lot cpu resource, thru the TID find be previous cmd.
jstack pid

```

## Diagnosing Tools

- jcmd

```bash
//All Java process's uptime
jcmd 0 VM.uptime

jcmd pid VM.command_line

//Show available configurations
jcmd pid help

jcmd pid Thread.print //same as jstack pid

//print counter
jcmd 5580 PerfCounter.print

```

- jps

```bash

jps -v //output includes arguments passed to main function
jps -m //output includes arguments passed to JVM

```

- jstack

jstack  prints  Java  stack traces of Java threads for a given Java process or core file or a remote debug server. For each Java frame, the full class name, method name, 'bci' (byte code index) and line number, if available, are printed. With the -m option, jstack prints both  Java  and native frames of all threads along with the 'pc' (program counter). For each native frame, the closest native symbol to 'pc', if available, is printed. C++ mangled names are not demangled. To demangle C++ names, the output of this command may be piped to c++filt.

```
jstack pid
jstack -h
```


- jstat

```bash

jstat -gcnew pid // ask for gc new data for pid {pid}

// each 1s, show JIT compilation for 5580
jstat -printcompilation 5580 1s

jstat -options
	-class
	-compiler
	-gc
	-gccapacity
	-gccause
	-gcmetacapacity
	-gcnew
	-gcnewcapacity
	-gcold
	-gcoldcapacity
	-gcutil
	-printcompilation



```
