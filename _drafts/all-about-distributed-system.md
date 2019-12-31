---
layout: post
title:  "All About Software Architecture"
date: 2019-12-02 01:12:13
categories: Architecture Data intensive Application, Distributed System
---

# Introduction
The paper is for understanding of core abstraction and theory in distributed system domain.

## What's is distributed system
A colloction of sytem works as one system.

* Working concurrently
* Can fail independently
* No centralized clocks. 

# ACID

## Serializability
For concurrency case. 

# Consistency & Concensus

## Linearizability
It is for recency guaranteed. Behave as though there is only a single copy of data, and all operations are atomic on it. 

## Zookeeper
Zookeep was robust services, by its well defined APIs, it enables developers to implement common coordination tasks, like:

* Synchronization: leader election & lock
* Managing group membership
* Managing metadata, metadata storage
* Service discovery

# Data plane and Control plane
## The question is What is data plane and control plane?
Let's say you and I are in charge of public transportation for a small city.

Before we send bus drivers out, we need to have a plan.

Control Plane = Learning what we will do

Our planning stage, which includes learning  which paths the buses will take, is similar to the control plane in the network.   We haven't picked up people yet, nor have we dropped them off, but we do know the paths and stops due to our plan.  The control plane is primarily about the learning of routes.

In a routed network, this planning and learning can be done through static routes, where we train the router about remote networks, and how to get there. We also can use dynamic routing protocols, like RIP, OSPF and EIGRP to allow the routers to train each other regarding how to reach remote networks.  This is all the control plane.

Data Plane = Actualy moving the packets based on what we learned.

Now, after the routers know how to route for remote networks, along comes a customers packet and BAM! this is were the data plane begins. The data plane is the actual movement of the customers data packets over the transit path.   (We learned the path to use in the control plane stage earlier).


