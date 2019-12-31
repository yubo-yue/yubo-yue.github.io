---
layout: post
title:  "All about micro service"
date: 2018-11-23 01:12:13
categories: Microservice
---

# Overivew
This page is all about Microservice. 

## What is microservice?
It is Small, and focus on and only one functionality. Next question is how small is small enough? The rule of thumb is we can rewrite one service in two weeks. 

Its pros:
* Technology heterogeneous
* Resilience of fault
* Scaling in finer granularity
* Ease of deployment
* Organization alignment
* Composability
* Optimization for replaceability 

- What is the difference/relationship between SOA and Microservice?

- What are the building blocks of microservice?

- What are closest related technologies about microservice?


# Service Mesh
The concept of using service mesh to manage service-to-service communication within a cluster burst onto the scene back in 2016, driven partly by the hype-driven adoption of microservice-based architectures, and also the acceptance of Kubernetes as the de facto way to run containers. 

## What is service mesh?
service mesh provided policy-based, network services for network connnected workload 

## Service Mesh control plane and data plane
https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc?gi=21fd4a17122

The initial microservices maxim of creating "smart endpoints and dumb pipes" has now evolved, and service meshes are vying to put just enough smart back into the piples. 

# The way to micro service / service mesh
- monolith (Big Ball of Mud BBoM)
- modular monolith
- distrisbuted monolith
- micro-service
- service mesh --> it is the future??

Two perspective to see them:
- Modularity
- Deployment Units

## The majestic monolith
https://m.signalvnoise.com/the-majestic-monolith/

## See how spotify doing 
Scaling Agile @ Spotify - https://blog.crisp.se/wp-content/uploads/2012/11/SpotifyScaling.pdf


# CNCF
- (CNCF)[https://www.cncf.io/]. 

## Istio/Envoy

* [What is Istio](https://istio.io/docs/concepts/what-is-istio/)
* Envoy

## Linkred2 (Conduit)
* [What is Linkred2](https://linkerd.io/2/overview/)

## K8s (Kubernates)
[K8s](https://kubernetes.io/) deserve its own territory, here only focus on how K8s contribute from service mesh perspective. 



# References
* [From Microliths to Microsystems: Jonas Bonér at QCon London](https://www.infoq.com/news/2017/03/microliths-microsystems)

## High-Availablity large-scale system

* Porter, Brad - Availability-Oriented Architecture
 * Architecting for Availability isn't about getting everything right, its about doing the basic very well
 * Think about your components as appliance performing a function at a cost
 * Don't make it overly complicated
 
* Porter, Brad - The Appliance Architecture
 * Service-Oriented Architecture focuses on interfaces and interconnections
 * Appliance-Architecture focus on task specific components
 * Think of you data-center components as appliances (Microwave, Tivo, Refrigerator)
 * Seperate transaction routing, transaction processing and persist data.
 * Eight concepts from Appliance-Architecture
  * Role Definition
  * Transaction Cost Model
  * Capacity Planning
  * Scaling
  * When to partition
  * "Out of Service" sign
  * When to upgrade
  * Security

## How to generate global unique id for DB, transaction, etc ....?
* auto incremental ID, like MySQL, Oracle; AWS Aurora, RDS ;
* UUID
* Facebook snowflake algorithm 

## In Amazon, Envoy is used in ECS App Mesh as the proxy implementation?
See Werner Vogels' [article](https://www.allthingsdistributed.com/2019/03/redefining-application-communications-with-aws-app-mesh.html) first. 




