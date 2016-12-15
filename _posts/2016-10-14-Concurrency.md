---
layout: post
title:  "Things to be know about concurrency in JAVA"
date: 2016-10-14 11:50:13
categories: java thread concurrency
---

## Overview
There is a long journey to get accustom of Java thread and concurrency. The main reason is find a great book on this topic is quite hard. Even though there are a lot of book try to clarify this topic directly by using easy understanding words, but few can do that. Brian Goetz's __Java Concurrency in Practice(JCIP)__ is one of the winner who describe this topic so well, with enough depth on concurrency scope.

## What is definition of thread safe class? 
I think to better understand and grasp of concurrency programming, the first question you should ask yourself is What is thread safe?.  If you can't give out a clear answer for this question, i don't think you can write a correct and efficient java concurrent program because you are lost at the beginning. 

Here is one definition from JCIP:

A class is thread‐safe if it behaves __correctly__ when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.

From the previous definition of thread-safe, the core to be thread safe is correctness, so __what is the correctness of class__? 

	Correctness means that a class conforms to its specification. A good specification 
	defines invariants constraining an object's state and post‐conditions describing 
	the effects of its operations.

So at the end, what a thread-safe class can help u???

	Thread‐safe classes encapsulate any needed synchronization so that clients need not 
	provide their own.

## Atomicity 

* When we say this operation is atomic operation, what is actually meaning? in other words, what does **not atomic** means?

	it is not atomic, which means that it does not execute as a single, indivisible operation.

* Race condition, what is it ?

	A race condition occurs when the correctness of a computation depends on the relative 
	timing or interleaving of multiple threads by the runtime; in other words, when getting 
	the right answer relies on lucky timing.


* Race condition type
	
	Common ones are __read-modify-write__ and __check-then-act__ . 

## Locking

Lock is one method to solve race condition by make non-atomic operation to be atomic. 

## Liveness and Performance - synchronization is not the only concern

	There is frequently a tension between simplicity and performance. When implementing
	 a synchronization policy, resist the temptation to prematurely sacrifice simplicity
	  (potentially compromising safety) for the sake of performance.

## Sharing Objects
There are two way to design thread-safe class, or more broad speaking, design correct concurrent program. Atucally, there are two primary ways:
One is using synchroniztion to prevent multiple threads from accessing the same data at the same time; 
The other is how to share and publish object can be accessed safely by multiple threads at the same time.

* Below all about visibility of shared states

### Stale Data (visibility issue)

At least it provide __out-of-the-thin-air safety__.

### Non-atomic 64bit operations

Even can't meet the requirement of __out-of-thin-air safety__.

### Locking and Visibility

	Intrinsic locking can be used to guarantee that one thread sees the effects
	 of another in a predictable manner

	Locking is not just about mutual exclusion; it is also about __memory visibility__. 
	To ensure that all threads see the most up‐to‐date values of shared mutable 
	variables, the reading and writing threads must synchronize on a common lock.

### What is volatile keyword used for?

	The Java language also provides an alternative, weaker form of synchronization, 
	volatile variables, to ensure that updates to a variable are propagated predictably 
	to other threads. When a field is declared volatile, the compiler and runtime are 
	put on notice that this variable is shared and that operations on it should not 
	be reordered with other memory operations. Volatile variables are not cached in 
	registers or in caches where they are hidden from other processors, so a read of 
	a volatile variable always returns the most recent write by any thread.

_What is more:_
	
	The visibility effects of volatile variables extend beyond the value of 
	the volatile variable itself. When thread A writes to a volatile variable and 
	subsequently thread B reads that same variable, the values of all variables that 
	were visible to A prior to writing to the volatile variable become visible to
	B after reading the volatile variable.


_At last, you remember that:_

	Locking can guarantee both visibility and atomicity; 
	volatile variables can only guarantee visibility.

## Publication and Escape

### What is publishing object means?

	Publishing an object means making it available to code outside of its current scope.

	An object that is published when it should not have been is said to have escaped

### So how to publish in safe way? or what is an safe publication way?

_Safe Construction Practises_
	
	Don't allow this reference to escape during construction.

## Thread Confinement

	Accessing shared, mutable data requires use synchronization. One way to avoid this
	requirement is to not share. If data is only accessed from a single thread, no 
	synchronization is required.  Thread confinement is one of the simplest way to achieve 
	thread safety. Once an object is confined to thread, such usage is automatically 
	thread-safe even if the confined object itself is not.


### Thread confinement
	Stack confinement or ThreadLocal confinement 

_Stack Confinement_ 
	
	Stack confinement is a special case of thread confinement in which an object 
	can only be reached through local variables. Just as encapsulation can make 
	it easier to preserve invariants, local variables can make it easier to confine 
	objects to a thread.

_ThreadLocal_
	
## Immutability

	Immutable objects are always thread safe.

### What is an immutable object? 

1. its states can not be modified after construction;
2. all its fields are final;
3. It is properly constructed(its this reference doesn't escape during construction);


## Safe Publication
	
	To publish an object safely, both the reference to the object and the object's 
	state must be made visible to other threads at the same time. A properly 
	constructed object can be safely published by:

	• Initializing an object reference from a static initializer;
	• Storing a reference to it into a volatile field or AtomicReference;
	• Storing a reference to it into a final field of a properly constructed object; or
	• Storing a reference to it into a field that is properly guarded by a lock.

## Design a thread safe class

### Gathering synchronization requirements

Making a class thread-safe means ensuring that its invariants hold under concurrent access. This requires reasoning about its states.

1. state space
2. invariant identify centain states is valid or invalid. 
3. state ownership

### Adding functionalities to existing thread-safe class

1. Extension
2. Client-side lock or external locking
3. Composition

## Building Block

	The best way to implement new thread-safe class is by using exisint thread safe components.

### Synchronized Collection and Concurrent Collections

### How is ConcurrentHashMap implemented?




