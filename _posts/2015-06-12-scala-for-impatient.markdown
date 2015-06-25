---
layout: post
title:  "Scala for Impatient"
date: 2015-06-12 12:08:13
categories: Scala
---

## Overview
Learn Scala, which is a functional programming lauguage upper JVM.

- Scala is hybrid imperative/functional programming
- Direction move to functional, ***no vars*** and no ***side effects***

## For impatient myself
- Operator in Scala actually is ***method*** not ***built-in operator***, which can be overrided
{% highlight scala %}

//a.method(b)
//a method b
//1.to(10)
//1 to 10

val x : BigInt = 1234567890

{% endhighlight%}

- Call Functions and Methods
{% highlight scala %}

import scala.math._ // In Scala, the _ character is a"Wildcard", like * in Java

scala.math.sqrt(2)
sqrt(2)

{% endhighlight %}

- For loops
{% highlight scala %}

// to , until return range. <- iterate over range
for (i <- 0 until s.length())
    sum += s(i)
printf("sum : %d\n", sum)

sum = 0
for (ch <- s)
    sum += ch
printf("sum : %d\n", sum)

{% endhighlight %}
