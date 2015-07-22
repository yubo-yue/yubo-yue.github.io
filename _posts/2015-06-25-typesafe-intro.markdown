---
layout: post
title:  "Type Safe Introduction"
date: 2015-04-16 11:50:13
categories: Typesafe Activator Akka Play Scala
---

## environment 

1. Typesafe Activator version 1.3.5
2. JDK 8/7

## activator installation

1. Download the newest activator bundles

## activator console

{% highlight bash %}

activator new

{% endhighlight %}

## activator ui
{% highlight bash %}
activator ui
{% endhighlight %}

## activator bash

{% highlight bash %}
activator new PROJECTNAME reactive-stocks
{% endhighlight %}

## activator eclipse plugin

1. the global file (for version 0.13 and up) at ~/.sbt/0.13/plugins/plugins.sbt
2. the project-specific file at PROJECT_DIR/project/plugins.sbt

Add below configuration

 addSbtPlugin("com.typesafe.sbteclipse" % "sbtecplise-plugin" % "4.0.0")

## update dependencies

reload, update, eclipse

## typesafe official repository
http://repo.typesafe.com/typesafe/releases/
