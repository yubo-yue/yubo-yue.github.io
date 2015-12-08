---
layout: post
title:  "JVM Options Practices"
date: 2015-05-08 11:50:13
categories: JVM
---

## Reference

1. blog.sokolenko.me/2014/11/javavm-options-production.html
2. http://www.oracle.com/technetwork/java/javase/gc-tuning-6-140523.html
3. https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr001.html#BABCBIDC

## JVM < 8 Reference VM Paramters
{% highlight bash %}

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
    -Dcom.sun.management.jmxremote.local.only=false
    -Dcom.sun.management.jmxremote.port=<port> 
    -Dcom.sun.management.jmxremote.authenticate=false 
    -Dcom.sun.management.jmxremote.ssl=false

{% endhighlight %}

## Java >= 8

{% highlight bash %}
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
    -Dcom.sun.management.jmxremote.local.only=false
    -Dcom.sun.management.jmxremote.ssl=false
{% endhighlight %}

## Target

1. Throughput how many time JVM spent on garbage collection
2. Pause time - application appears unresponsive because GC is working
3. Footprint - footprint is the working set of a process, measured in pages and cache lines
4. Promptness - is the time between when an object becomes dead and when the memory becomes available

## Best Practises

### Make your Heap Explicit

{% highlight bash %}
    -Xms <heap size> -Xmx <heap size>
    -XX:PermSize -XX:MaxPermSize
    -Xmn <young size>[m|k|g]
    -XX:SurvivorRatio=<ratio>
    survivorRatio = (young size / survivor size) - 2
{% endhighlight %}

### Make GC right

1. Serial GC
2. Parallel GC
3. CMS GC
4. G1 GC

### Runtime Compiler

## Tools

- App Dynamics, New Relic
- Jps, Jstat VisualVM
- jClarity
- Gatling
- Jhat, jcmd

## Examples

### Throughput prefer

{% highlight bash %}

nohup java \
-server \
-Xms6144m \
-Xmx6144m \
-XX:NewSize=2048m \
-XX:MaxNewSize=3072m \
-XX:+HeapDumpOnOutOfMemoryError \
-XX:+UseParallelOldGC \
-verbose:gc \
-XX:+PrintGCDetails \
-Xloggc:"/home/scripts/jvm-gc.log" \
-Djava.rmi.server.hostname=182.92.68.193 \
-Dcom.sun.management.jmxremote.port=9193 \
-Dcom.sun.management.jmxremote.authenticate=false \
-Dcom.sun.management.jmxremote.ssl=false \
-Dcom.sun.management.jmxremote.local.only=false \
-jar datareceiver.jar &

{% endhighlight %}

### TBD in future

### Others
{% highlight bash %}

//JDK 8
-XX:+TraceClassLoading
-XX:+TraceClassUnloading
-XX:+UnlockDiagnosticVMOptions
-XX:MaxMetaspaceSize=16M
-XX:MetaspaceSize=1M
-XX:+HeapDumpOnOutOfMemoryError
-Djava.rmi.server.hostname=127.0.0.1
-Dcom.sun.management.jmxremote.port=9193
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.local.only=false
-verbose:gc
-verbose:class
-XX:+PrintGCDetails
-Xloggc:"/tmp/jvm-gc.log"

{% endhighlight %}




  


