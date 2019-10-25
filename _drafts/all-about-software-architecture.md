---
layout: post
title:  "All About Software Architecture"
date: 2018-11-23 01:12:13
categories: Architecture
---

## Introduction

Software architecture is core part of complex software development. It is also one of most hardest part in software development lifecycle. Depends on the requirements, it requires author to describe it in different view with various concerns. But, as coding, it still has patterns/practices. 

## Agile Inception Deck
Copied from [The Agile Inception Desk](https://agilewarrior.wordpress.com/2010/11/06/the-agile-inception-deck/). The learn is ask those questions before the start of every project . 

- 10 questions to ask at the start of your next project
	- Why are we here?
	- Create the elevator pitch. A good elevator pitch tells people what your product is, who it's for, and why it's special in about the time takes to take a ride in an elevator.
	- Design a product Box. Think hard from your customer's point of view.
	- Create a NOT List. In Scope, Out of Scope and Unresolved categories.
	- Meet your neighbors. 
	- Show the solution
	- Ask what keeps us up at night.
		- Stuff we lose sleep over like Availability of the customer.
	- Size it up
	- Be clear on what's going to give. (Trade-off slider)
	- Show what it's going to give. (construction|UAT|Training|Ship it)

## Architecting for Scaling

- Reliability : The ability of your system to perform the operations it is intended to perform w/o making a mistake
- Availability: The ability of your system to be operational when needed in order to perform those operations.

### What causes poor Availability?
- Resource Exhaustion
- Unplanned load-based changes
- Increase number of moving parts
- Outside Dependencies
- Technical Debt.

### Five Focuses to Improve Application Availability
- Build with failure in mind
- Always think about scaling
- Mitigate risk
  - There is risk that a server will crash
  - There is risk that a database will become corrupted
  - There is risk that a returned answer will be incorrect
  - There is risk that a network connection will fail
  - There is risk that a newly deployed piece of software will fail.
- Monitor Availability
- Respond to availability issues in a predictable and defined way.

#### Risk Management
What your risk is? How much risk is acceptable? what can you do to mitigate risk?
- Risk Categorization (Likelihood/Severity - Low Medium and High)
	- H/H
	- L/H
	- H/H
	- L/L
- Risk Matrix (How to manage risks)
- Risk Mitigation : Risk Mitigation is about reducing the impact of a risk by either reducing the likelihood that the risk will occur, or reducing the severity of the problem if the risk does occur.

### Building systems with Reduced Risk

#### Redundancy
- Design your application so that it can run on multiple independent hardware components simultaneously.
- Design your application so that can run task independently.
- Design your application so that can run task asynchronously.
- Localize state into specific areas. 
- Utilize idempotent interfaces wherever possible.
- Downsides
	- Redundancy improvements that increase Complexity.

#### Independence
Multiple components utilizing shared capabilities or components may present themselves as independent components, but in fact, they are all dependent on a common component. 

#### Security
Bad actors have always been a problem in software systems. Security and security monitoring has always been a part of building systems.

#### Simplicity

#### Self-Repair
LoadBalancer, "Hot Standby" Database instance, Retry, queue system, etc....

#### Operation Process
Humans are involved in our software systems and humans make mistakes. The more you can automate the processes that humans normally perform in your production systems, the fewer mistakes that can be introduced and the higher the likelihood that the tasks will work. 


## Scalability Rules : 50 Principles for Scaling web sites

### Rule 37 - Never trust Single Points of Failure

### Rule 46 - Be wary of Scaling through Third party
* Why : Own your destiny, keep your architecture simple and reduce your total cost of ownership.

### Rule 47 - Purge, Archive and Cost-Justify Storage
* RFM - Recency, Frequency, Monetization...

## Patterns

### Definitions

- From POSA: A system of patterns. A pattern for software architecture describes a particular recurring design problem that arises in specific design contexts, and presents a well-proven generic scheme for its solution. The solution scheme is specified by describing its constituent components, their responsibilities and relationships, and the ways in which they collaborate.

### How to descripe Pattern
Pattern is not invented, actually it is discovered. So the important thing is how to describe it.

#### Template from POSA vol.1
 
1. Name
2. Also Known as
3. Example
4. Context
5. Problem - The problem this pattern resolves, including a discussion of its associated forces.
6. Solution - fundamental principle.
7. Structure
8. Dynamics 
9. Implemations
10. Variants
11. Known Uses
12. Consequences
13. See Also 


## Templates

- [SEI View and Beyond](http://www.iso-architecture.org/42010/applications.html)
- [Architecture Template](http://www.iso-architecture.org/42010/templates/)
