---
layout: post
title:  "Spring Petclinic - do u really understand it??"
date: 2016-04-22 13:16:13
categories: Java SSH Maven Git
---

## Overview

I thought myself know a lot technologies, java, spring, hibernate, jdbc, ... But really? These years, i learned a lot of new techs, best practises, and a lot of other skills i can't remember where and how it happened. But recently i felt void, void than ever before. 

Think more on this, i thought i am lost in those so-called new techs, frameworks, ...  totally. I need to recall who i am and why i learn those things. 

So i think the best way to do it is from where you start your programming career. But... I can't remember where i start, but i feel like it is about building some really cool stuff web application. 

Then, at last, i decide to go back here. Do i really understand and know how to build a petclinic application in Spring. Here i go!!

[petclinic GitHub](https://github.com/yubo-yue/spring-petclinic)

## Organize the dependencies

For dependencis, as the name explains itself, but there are different types of dependencies based on its usage and its lifetime. 

- build dependencies
- runtime dependencis
- test dependencies
- indirect depenedencis 

## Domain Models, Data Models & Tools

The first imporatant thing when implements your business logic is how to scope and understand your domain realm. The you need to extract the domain models and shape them into well forms. This is the prerequisite for the next steps, define your data model. 

Actually, in real world, it is hard to implement a 1 to 1 mapping between domain object to data object(persistence layer). 

So there is another concern on how to organize your domain logic and map them to data model in your persistence layer. Mostly used one may be the data mapper which very suitable for case that domain logic is based on domain model.

May not very exact, per my shallow understanding, the MyBatis, Hibernate(it should be more than the data mapper functionalities) are all good framework which reduct huge words on defining your own data mapper layer. 


## Connection Pool 

There are a lot of options currently, DBCP, C3P0, HikariCP and  Tomcat JDBC Connection Pool and others i can't remember the name... For which one is better, i don't think there is a clear answer here, may best way is test it under your scenario if you really concern about the difference, otherwise, i prefer to C3P0 because it already by Amazon for a few years and tested many times.

## Metrics, Monitors

### JMX

## Tiers vs Layers

Logical layers are merely a way of organizing your code. Typical layers include Presentation, Business and Data – the same as the traditional 3-tier model. But when we’re talking about layers, we’re only talking about logical organization of code. In no way is it implied that these layers might run on different computers or in different processes on a single computer or even in a single process on a single computer. All we are doing is discussing a way of organizing a code into a set of layers defined by specific function.

Physical tiers however, are only about where the code runs. Specifically, tiers are places where layers are deployed and where layers run. In other words, tiers are the physical deployment of layers.





