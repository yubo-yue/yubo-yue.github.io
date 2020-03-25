---
layout: post
title:  " Enterpise integration "
---

## Introduction {#abc}

This paper is about learning from "Enterprise Integration Pattern". 
Enterpises are comprised of hundreds, or even thousands of appplications that are custom built, third party , or legacy system. 

The reality is building enterpises system is hard. Name a few, SAP, oracle, peoplesoft. 

Different roles involved, enterpise, vendor, customer, seller, etc....
Business requirement is ambiguous, instable, never finalized. There are also accidental complexity introduced, like vendor cater to customer's needs, which is over-design and over complex. 

## What's challenge of Enterpise Integration ?
* Enterpise integration requires a significant shift in corporate politics


## Common Integration Styles

* File
* Shared database
* RPC
* Messaging

File and Shared database are sharing data, RPC is functionality sharing. 


## Messaging Systems 

Messaging system, like middleware. Channel is addresable which allow transit message from sender to receiver. 

### Message Channel
A message channel can be thought of as a pipe, a conduit from one application to another.

### Pipe and Filters

Fundamental architectural style for messaging system: Individual processing steps (filter) are chained together through the messaging channel. Many other patterns are based on this Pipes and Filters architectural style. This lets you easily combine individual patterns into larger solutions. 

### Message Router

## Messaging Channels





