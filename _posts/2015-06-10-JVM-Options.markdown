---
layout: post
title:  "JVM Options"
date: 2015-05-08 11:50:13
categories: Architecture Pattern POSA
---
## refer to blog.sokolenko.me/2014/11/javavm-options-production.html

## refer to http://www.oracle.com/technetwork/java/javase/gc-tuning-6-140523.html

## Java < 8

    -server
        -Xms<heap size>[g|m|k] -Xmx<heap size>[g|m|k]
        -XX:PermSize=<perm gen size>[g|m|k] -XX:MaxPermSize=<perm gen size>[g|m|k]
        -Xmn<young size>[g|m|k]
        -XX:SurvivorRatio=<ratio>
        -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled
        -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=<percent>
        -XX:+ScavengeBeforeFullGC -XX:+CMSScavengeBeforeRemark
        -XX:+PrintGCDateStamps -verbose:gc -XX:+PrintGCDetails -Xloggc:"<path to log>"
        -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M
        -Dsun.net.inetaddr.ttl=<TTL in seconds>
        -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<path to dump>`date`.hprof
        -Djava.rmi.server.hostname=<external IP>
        -Dcom.sun.management.jmxremote.port=<port> 
        -Dcom.sun.management.jmxremote.authenticate=false 
        -Dcom.sun.management.jmxremote.ssl=false


## Java >= 8
    -server
        -Xms<heap size>[g|m|k] -Xmx<heap size>[g|m|k]
        -XX:MaxMetaspaceSize=<metaspace size>[g|m|k]
        -Xmn<young size>[g|m|k]
        -XX:SurvivorRatio=<ratio>
        -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled
        -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=<percent>
        -XX:+ScavengeBeforeFullGC -XX:+CMSScavengeBeforeRemark
        -XX:+PrintGCDateStamps -verbose:gc -XX:+PrintGCDetails -Xloggc:"<path to log>"
        -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M
        -Dsun.net.inetaddr.ttl=<TTL in seconds>
        -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<path to dump>`date`.hprof
        -Djava.rmi.server.hostname=<external IP>
        -Dcom.sun.management.jmxremote.port=<port> 
        -Dcom.sun.management.jmxremote.authenticate=false 
        -Dcom.sun.management.jmxremote.ssl=false

## Target

1. Throughput how many time JVM spent on garbage collection
2. Pause time - application appears unresponsive because GC is working
3. Footprint - footprint is the working set of a process, measured in pages and cache lines
4. Promptness - is the time between when an object becomes dead and when the memory becomes available

## Measurements



## solutions

### Make your Heap Explicit
    -Xms <heap size> -Xmx <heap size>

    -XX:PermSize -XX:MaxPermSize

    -Xmn <young size>[m|k|g]

    -XX:SurvivorRatio=<ratio>
    survivorRatio = (young size / survivor size) - 2

### Make GC right

### Runtime Compiler

