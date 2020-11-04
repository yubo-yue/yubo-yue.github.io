---
layout: post
title:  " Architecture Distilled "
---

# introduction
This post is personal summary of understanding of software architecture. 

# Big Ball of Mud (BBoM)
I am seeing too much examples of BBoM, in service oriented world, i do observe more and more Distributed BBoM. 

# DDD 
To handling the core complexity of problem with software. In short, DDD is primary about modeling.

## How
Strategic design and tactical design.

### Strategic design
Bound Context:
Ubiquitous Language: see how to develop a UL as your domain model within an explicity Bounded Context. 
Subdomain: 
Context Mapping: how to integrate multiple Bounded Context.

### Tactical Design
Entity
Value Object
Aggregate


# Architecture patterns observed

## Onion (Hexagonal) Architecture

## Port and Adapter

## Cell based architecture

### Problem statement
Each of our regional AWS services, including S3, DynamoDB and SQS, is built as a single region-scale instance. As a region grows, we scale up the single instance by adding capacity at multiple layers, and by making architectural changes where needed. Even our zonal services, like EC2 and EBS, have availability zones that have grown extremely large, and continue to grow. This scale-up approach has several major disadvantages. The most obvious is blast radius – a single operational mistake, infrastructure failure or software bug can take down an entire region. These scale-up services are also too big to test; testing them to their scale failure point is impractical and often not economically viable. Finally, many distributed systems architectures have non-linear scale factors, which become a problem with systems designed to scale up.

### Scale up and scale out
The differences between scale-up (vertically scaled) and scale-out (horizontally scaled) architectures are well understood, but it's worthwhile to take a moment to walk through the characteristics and consequences of each approach.

#### Scale up
Scaling up, or accommodating growth by increasing the size of a system's component (database, server, subsystem, etc.), is a natural and straightforward way to scale, but has limitations:

Maximum size limitations: The clearest limitation is the absolute or practical size cap the component can reach. In the early days of Amazon, the scale-up approach led the main Oracle database (“ACB”) to eventually run on HP Superdome hardware, which cost in excess of $1MM and had the dubious distinction of being listed on the “Top500” list of most powerful supercomputers in the world.
Non-linear scaling factors: Scaling up can expose non-linear scaling factors. An example seen in several large scale events is the communication and coordination overhead between nodes in a single logical subsystem, which can grow superlinearly as you add nodes to the system. Another non-linear scaling factor can be cost: a twice-as-powerful database server can cost more than twice-as-much.
Hidden contention points: The non-linear scaling factors and other implications of scale-up architectures can be challenging to reason about, leaving hidden contention points that don't manifest until you reach a certain level of scale. These can be catastrophic when the triggering scaling demands are non-transient.
System-wide shared fate: A scaled-up system is fundamentally a single system, meaning there is system-wide shared fate and risk of a complete system outage, even those not driven by non-linear scaling factors and hidden contention points. The larger the scale, the larger the blast radius.
Too big to test: Scale-up systems become large, complex, and expensive. It can be expensive to create a representative test environment. It can also be challenging to produce a workload in a test environment that simulates the demands on the live system and test the system to failure. The production DynamoDB installation that suffered an LSE in September 2015 consisted of thousands of nodes in the Request Router subsystem and thousands of nodes in the Storage subsystem.
"Every Day is a New Adventure": A single scale-up system that sees continuous growth is one where the load demands continue to reach new heights, causing the system to regularly operate under conditions never seen before (unless scale tested beyond current demand, but see Too big to test).

### Scale out
Scaling out, on the other hand, accommodates growth by increasing the number of system components (databases, servers, subsystems, etc.), and dividing the workload such that the load on any component stays bounded over time despite the overall increase in workload. The task of dividing the workload can make scaling out more challenging than scaling up, particularly for stateful systems, but has well-understood benefits:

Workload isolation: Dividing the workload across components means workloads are isolated from each other. This provides failure containment and narrows the impact of issues such as deployment failures, poison pills, misbehaving clients, data corruption, operational mistakes, etc.
Maximally-sized components: Accommodating growth by increasing the number of components rather than increasing component size means the size of each component can be capped to a maximum size. This reduces the risk of surprise from non-linear scaling factors and hidden contention points present in scale-up systems.
Not too big to test: With maximally-sized components, the components no longer have to be too big to test. These components can be stress tested and pushed past their breaking point to understand their safe operating margin. This does not address testing the overall scaled out system composed of these components, but if the majority of the complexity and risk of the system sits in stress-tested components (and it should), the level of of test coverage and confidence should be significantly higher.

### When Scale out becomes Scale up

Scale-up architectures are often easy to identify. The most obvious is when a relational database has to be moved to bigger, stronger, faster hardware. But sometimes it's not as obvious. When a single node Oracle instance runs out of gas on the biggest available hardware, and is migrated to Oracle RAC, a multi-server clustering solution, is that scaling out or scaling up? The hardware is scaling out (increasing the number of components rather than the size of the component), but it is still a single database instance. That means from a logical component perspective, the database is still being scaled up. It's getting larger, experiencing superlinear scaling factors (e.g. cross-node lock coordination), getting closer to hidden contention points, getting harder to test, and increasing in blast radius.

The same is true of service fleets. We scale service fleets by adding servers, which is scaling out. But viewed as a component, that service fleet is scaling up, with all the same attendant risks.

### Cell
Rather than build out services as single-image systems, we propose a different approach: break our services down internally into cells and build thin layers to route traffic to the right cells.

({{ site.url }}/assets/cell-based-architecture.png)

Cells change our approach from scaling up to scaling out. Each cell, a complete independent instance of the service, has a fixed maximum size. Beyond the size of a single cell, regions grow by adding more cells. This change in design doesn’t change the customer experience of our services. Customers can continue to access regional services as they do today.

### Benefits

Cell-based architectures have a number of benefits:

Lower Blast Radius: Breaking a service up into cells reduces blast radius. Cells represent bulkheaded units that provide containment for many common failure scenarios. When properly isolated from each other, cells have failure containment similar to what we see with regions. It is highly unlikely for a service outage to span multiple regions. It should be similarly unlikely for a service outage to span multiple cells.
Higher Scalability: Cell-based architectures scale-out rather than scale-up, and are inherently more scalable.
Higher Testability: The capped size of cells allows for well-understood and testable maximum scale behavior.
Higher MTBF: Not only is the blast radius of an outage reduced with cells, so should the probability of an outage. Cells have a consistent capped size that is regularly tested and operated, eliminating the “every day is a new adventure” dynamic.
Lower MTTR: Cells are also easier to recover, because they limit the number of hosts that need to be analyzed and touched for problem diagnosis and the deployment of emergency code and configuration.
Higher Availability: A natural conclusion is that cell-based architectures should have the same overall availability as monolithic systems, because a system with n cells will have n times as many failure events, but each with 1/nth of the impact. But the higher MTBF and lower MTTR afforded by cells means fewer shorter failures events per cell, and higher overall availability.
Safer Deployments: Like one-box and single-AZ deployments, cells provide another dimension in which to phase deployments and reduce blast radius from problematic deployments. Further, the first cell deployed to in a phased cell deployment can be a canary cell with synthetic and other non-critical workloads, to further reduce the impact of a failed deployment.

### Cell Sizing
Cell-based architectures benefit from capping the maximum size of a cell, and using consistent cell sizes across different installations (e.g. regions). Rather than using bigger cells in bigger installations, configure bigger installations to have more cells and smaller installations to have fewer cells, as depicted here:

There are tradeoffs to be considered in how to select the maximum size of a cell.

Keeping cells small has the benefit of reducing blast radius in the event of a cell-level failure. Small cells are easier to test to failure than larger cells, as you need less state and a smaller workload to simulate stress events. Finally, operational issues in a cell are easier to diagnose and resolve as there are fewer log entries to investigate, fewer hosts to log into, etc.

Allowing larger cells can have the benefit of better capacity utilization. Larger cells make it easier to accommodate "whales", i.e. workloads that disproportionately stress a particular cell. Finally, larger cells means fewer cells for a system installation, which makes the system as a whole easier to operate: fewer deployment environments, fewer cell-specific graphs in a dashboard, etc.

The maximum cell size will vary per-service. The optimum point will depend on the service and customer behavior, but needn’t be extremely large for any service. EBS, for example, found that utilization improvements diminish for cells bigger than ~0.5% of global capacity. This is a decision that can be made with usage data.


### Implementation
The best pattern for implementing cells depends on the service. The critical decisions are choice of a partition key (on what dimension are cells divided), partition mapping algorithm (how partition keys are mapped to cells), and the design of the thinnest possible layer.

#### Partition key

#### Partitino mapping algorithm

There are a variety of partitioning algorithms that can be used to map keys to cells. Regardless of algorithm, there need to be

a mechanism to serve or distribute state used by these algorithms,
accomodations for gracefully handling migration when cells are added and removed.
The following is a non-exhaustive list presented without specific recommendations.

* Naive Modulo Mapping
Naïve Modulo Mapping uses modular arithmetic to map keys to cells, typically on a cryptographic hash of the key. This scheme has an effective zero peak-to-average ratio (very even distribution) and requires minimal state (just the count of cells). But it suffers from high churn (cell reassignment) when adding or removing cells.

int mapToCell(int keyHash) {
    return keyHash % lookupNumCells();
}

* Consistent Hashing

Consistent Hashing is a family of algorithms that map keys to buckets (cells) with a small amount of fairly stable state and a minimal amount of churn when adding or removing buckets.

int mapToCell(int keyHash) {
    return consistentHash(keyHash, numCells);
}
The Ring Consistent Hash (Karger, et. al.) [1] is the consistent hashing scheme used in the Chord Distributed Hash Table, and is probably the best known. It can suffer from significant high peak-to-average ratios (uneven spread), although this can be offset with the tradeoff of additional state.

More recent algorithms improve upon the peak-to-average ratio and state requirements of the Ring Consistent Hash:

Jump Consistent Hash (Lamping and Veach) 
Multi-probe Consistent Hashing (Appleton and O’Reilly)

* Full Mapping

A highly flexible yet expensive approach is to explicitly map every key to a cell. This comes with the downsides of a critical read and write dependency on the mapping table, a read-your-writes consistency requirement, and a large amount of state.


* Prefix and Range-based Mapping
Prefix and range-based mapping map ranges of keys (or hashes of keys) to cells, and serves to offset the downsides of the full mapping approach while providing flexibility.


* Fully Mapped Logical Buckets
This scheme is a hybrid of naïve modulo and full mapping. A system is configured with a fixed large number (e.g. tens of thousands) of logical buckets which are explicitly mapped to much smaller number of physical cells. Mapping a key to a cell is a two-step process. First, the key is mapped to its logical bucket using naïve module mapping. Then the cell for that bucket is located using a bucket to cell mapping table.


* Mapping Overrides
Regarding of partition mapping approach, it's important to also use an override table to force specific keys to specific cells (except for Full Mapping, which natively provides this support). This can be useful for testing, quarantining, and special-case routing for particularly heavy partition keys.


#### Cell migration
Stateful cell-based architectures will almost certainly require online cell migration to adjust placement when cells are added or removed.

One consideration of online cell migration is handling mapping decisions during the transitionary period. This may involve cross-cell redirects and/or performing multiple iterations of the mapping algorithm when necessary, against different versions of the mapping algorithm state.

Another consideration is how to safely migrate the state. This will be system-dependent but at a high-level will likely consist of the following phases:

Clone the data from the current location into the new location, as a non-authoritative copy
Flip the new location copy to be “authoritative”
Redirect from old location to new location
Forget the data from the old location

### Thinnest Possible Layer
For most services, the top layer will be a routing load balancer or proxy fleet. There is an opportunity to build features into JLB and ELB that make implementing these layers easier. EC2 Networking is building an advanced “API Gateway” for internal services called DimensionX which plans to include cell routing support.

This “thinnest possible layer” is the sole remaining shared component in cell-based architectures and needs to be as simple as possible, but no simpler. Operational issues at this layer can have a much higher blast radius than issues in individual cells. This layer needs to have request dispatching isolation between cells, to prevent a slowdown or other issue in one cell from impacting the ability to dispatch requests to other cells. It’s also important to minimize the amount of business logic in this layer. A good analog to consider is the OnlineProxyFleet used by the retail website. This is a fleet of Apache servers that use simple regex-based rules for most request routing. The owners of OPF place considerable scrutiny on any changes and a bias toward rejecting proposals for more complex routing logic.

There are also opportunities to avoid routing all calls, which can help reduce latency, and reduce the scale and blast radius of the routing layer. For example, clients communicating with SASSY streams only require routing at session initiation time. HTTP forwarding and response headers can direct cooperative clients directly to their cell for improved availability and reduced latency. For some services with named resources, like DynamoDB tables/tablespaces and S3 buckets, per-resource DNS could point requests directly at the right cell.

### Operational tooling
Effectively operating multiple cells for each of our services will also require improved deployment and operational tooling. We need to build tools that make it fast and safe to do staggered deployments across cells. Monitoring will also likely need to support cells as a first-class concept, allowing us insight into both the performance of individual cells and our region-scale customer experience.

There is a "Scaling Operational Load" working group in AWS that is working with Monitoring and Builder Tools to add capabilities to make it easier to operate in a world of cell-based architectures and many regions. Stay tuned for more updates in this area in the coming months.

## Layer architecture

## Pipe and Filter architecture

## Event-Drive architecture

## Reactive Messaging Pattern with the Actor Model.

## Microservice

## Service Mesh

Service meshes provide policy-based, network services for network-connected workloads by enforcing desired behavior of the network in the face of constantly changing conditions and topology. Conditions that change can be load, configuration, resources (including those affecting infrastructure and application topology of intracluster and intercluster resources coming and going), and workloads being deployed.

Service meshes are an addressable infrastructure layer that allow you to manage both modernizing existing monolithic (or other) workloads as well as wrangling the sprawl of microservices. 

This uniform layer of infrastructure combined with service deployments is commonly referred to as a service mesh. Istio turns disparate microservices into an integrated service mesh by systemically injecting a proxy among all network paths, making each proxy cognizant of one another, and bringing these under centralized control; thus forming a service mesh.

A service mesh is a dedicated infrastructure layer for making service-to-service communication safe, fast, and reliable, at times relying on a container orchestrator or integration with another service discovery system. 

## MVC

## BDD  Behavior Driven Development practice

## Reactive system

In 2013, the Reactive Manifesto was created to do exactly this. This manifesto was conceived with the aim of condensing all of the knowledge we had accumulated as an industry in designing and building highly reliable and scalable applications.  It then distilled this knowledge into a set of required architectural characteristics that would make any application flexible, loosely coupled, and elastic. It also carved out a defined vocabulary to enable efficient and clear communication between all participants, including developers, project leaders, architects, and CTOs.

The Reactive Manifesto lays out four key high-level characteristics of reactive systems: they are responsive, elastic, resilient, and message driven (see Figure 2-1). Although there are dependencies between them, these characteristics are not like the hierarchical tiers in a standard layered application architecture; instead, they describe design properties that should be applied across the entire technology stack.

Reactive system follow the reactive manifesto:
REsponsive
Resilient: this is about failure handling.
Elastic: this is about load
Message driven: message is for recipient, event is for source.

reactive programming

Reactive streaming
Reactive streaming is a specification designed to provide a standard for asynchronous stream processing with nonblocking back pressure (in other words, flow control).
The Reactive Streams specification was created to govern the exchange of stream data across an asynchronous boundary while ensuring that the receiving side is not forced to buffer arbitrary amounts of data. When handling “live” data, it is almost impossible to predict the volume of data passing through the system at any moment. This means that resource consumption has to be carefully controlled to ensure that a fast data source does not overwhelm the destination of the stream.

### Toolbox to reactive

* Getting Responsiveness: Concurrency and parallelism

Two tasks are said to be running concurrently when they begin, run, and finish within the same time window. Parallelism is when two (or more) tasks are running at the same time.


##### Reactor Model
The reactor pattern, illustrated in Figure 3-2 in its most basic form, approaches both concurrency and parallelism. In the typical implementation of the pattern, asynchronously received requests are demultiplexed (in a sense, serialized) for processing. The event loop, running on one thread, cycles through the incoming events and handles them. Callback functions are registered for requests that will result in a long-running task or blocking operation. The handle for the event gets added to a queue. The event loop iterates through the queue and will eventually observe the completion of the long-running task, trigger a callback, and return the result to the application.

##### Actor Model

In the actor model, actors are the fundamental unit of computation, and they have some important qualities that make them especially suitable to a distributed systems environment. First, actors are lightweight, loosely coupled, and maintain their own state. Second, message-passing between actors is completely asynchronous and without restriction to message ordering, making computations done throughout a system of actors inherently concurrent. Also, because interaction between actors is limited to message passing, they can be distributed across nodes (servers, virtual machines, containers). The result is a computational model that achieves concurrency, parallelism, and an inherent ability to scale horizontally.

* AKKA

Akka is an open source “toolkit for building highly concurrent, distributed, and resilient message-driven applications” running on a JVM. In addition to providing a hierarchical actor implementation (essential for failure detection/recovery), Akka includes libraries for actor cluster management, sharding, and persistence (invaluable for distributing applications across compute resources).

Let’s go back to our discussion of concurrency, parallelism, and threads.  In Akka, a message dispatcher is central to how threads are managed within an actor system. The dispatcher defines the executor service to be used, the size of the thread pool, and how many messages an actor may process before it relinquishes a thread. The dispatcher assigns a thread to an actor only when it has a message in its queue. The actor processes the messages and then gives back the thread. The obvious advantage is that threads are consumed only when there is actual work to be done. A less obvious advantage is that idle actors remain in memory, meaning they are immediately available for execution at all times. This presents little impact to system resources given that actors are quite small (less than one kilobyte), even if there are hundreds of thousands of them on a given node. The result is a highly efficient use of processor resources.

## Big Data

### Data Mesh

https://martinfowler.com/articles/data-monolith-to-mesh.html

### Lambda architecture
batch layer
speed layer
serving layer

### Kappa architecture
http://milinda.pathirage.org/kappa-architecture.com/
no batch layer, with conanical immutable data store as record of system. 




