---
layout: post
title:  "Class Loader Java - Best Practice"
date: 2015-12-13 21:34:13
categories: Java, Class Loader
---

## Overview
This blog explains the structure of classloader hierarchy, the principle of classloaders. And it also shows how to create a new classloader, and pitfalls when working with classloaders.

## Class and Data
A class represents the code to be executed, whereas data represents the state associates with the code. 
When we associate a particular state to a class, we have an instance of that class.
In java, a class usually have its code contained in .class file. In runtime, each and every class will have its code also available in the form of first-class java object, which is an instance of java.lang.Class. 
Whenever we compile any java class file, the compiler will embed a public, static, final field named class, of the type java.lang.Class, in the emitted byte code. Since this field is public, we can access it using dotted notation, like this:

```java

java.lang.Class klass = Myclass.class;

```

## Class Loaders

- Bootstrap Class Loader, native implementation in JVM

- ExtClassLoader, _java.ext.dirs_ defines where to load *.jar

- AppClassLoader, _java.class.path_ defines which *.jar to be loaded.

- Except Bootstrap Class Loader, everyone has a parent Class Loader.

({{ site.url }}/assets/classloader.png)


## References