---
layout: post
title:  "Kubernetes scheduling assume/forget"
date: 2026-02-12 01:12:13
categories: kubernetes scheduling assume patterns
---

## Assume Pattern in Kubernetes Scheduler (with Diagrams)

### What It Is

The **Assume pattern** is a critical optimization in Kubernetes's scheduler that **pre-commits pod placements in memory before the API confirms them**. It's the scheduler's way of saying: "I've decided this pod goes on node X, so let me immediately reflect that in my cache—don't wait for the API server to confirm it."

### Pod State Machine

```mermaid
stateDiagram-v2
    [*] --> Initial
    Initial --> Assumed: AssumePod()
    Assumed --> Added: AddPod()\n(confirmed from API)
    Assumed --> Expired: timeout
    Expired --> Added: AddPod()\n(re-add if needed)
    Assumed --> Forgotten: ForgetPod()\n(binding failed)
    Forgotten --> [*]
    Added --> Updated: UpdatePod()
    Updated --> Added
    Added --> Deleted: RemovePod()
    Deleted --> [*]
    
    note right of Assumed
        Pod is in cache with
        assumed resource usage
        on target node
    end note
    
    note right of Added
        Pod is confirmed by
        API server - real state
    end note
```

### Scheduling Workflow Timeline

```mermaid
sequenceDiagram
    participant Scheduler
    participant Cache
    participant API Server

    Scheduler->>Scheduler: Decision: Pod A → Node 1
    Scheduler->>Cache: AssumePod(Pod A, Node 1)
    activate Cache
    Cache->>Cache: Update Node 1 capacity<br/>(subtract Pod A resources)
    Cache-->>Scheduler: ✓ assumed
    deactivate Cache

    par Parallel Processing
        Scheduler->>Scheduler: Schedule Pod B<br/>(use cache with Pod A's usage)
        Scheduler->>Scheduler: Schedule Pod C<br/>(continue...)
        
        Scheduler->>API Server: Binding Request for Pod A<br/>(async, non-blocking)
        activate API Server
    end

    Scheduler->>Scheduler: Schedule Pod D<br/>(all use fresh cache)

    API Server->>Cache: Pod Added Event (Pod A confirmed)
    activate Cache
    Cache->>Cache: Convert Assumed → Added state<br/>(was already accounted for)
    Cache-->>Scheduler: ✓ confirmed
    deactivate Cache
    deactivate API Server
```

### Failure Scenario

```mermaid
sequenceDiagram
    participant Scheduler
    participant Cache
    participant API Server

    Scheduler->>Cache: AssumePod(Pod A, Node 1)
    Cache-->>Scheduler: ✓ assumed

    Scheduler->>Scheduler: Schedule Pod B, C, D...
    Scheduler->>API Server: Binding Request for Pod A

    API Server-->>Scheduler: ✗ Binding Failed<br/>(node full, pod evicted, etc)
    
    Scheduler->>Cache: ForgetPod(Pod A)
    activate Cache
    Cache->>Cache: Restore Node 1 capacity<br/>(add back Pod A resources)
    Cache-->>Scheduler: ✓ removed
    deactivate Cache

    Scheduler->>Scheduler: Re-queue Pod A<br/>(for next scheduling cycle)
```

### Cache State Comparison

```mermaid
graph TB
    subgraph Before["Without Assume Pattern (Blocking)"]
        B1["Scheduler decides<br/>Pod A → Node 1"]
        B2["Submit binding<br/>to API server"]
        B3["⏳ WAIT for response<br/>10-100ms latency"]
        B4["API confirms"]
        B5["Update cache with Pod A"]
        B6["Now schedule Pod B"]
        
        B1 --> B2 --> B3 --> B4 --> B5 --> B6
    end
    
    subgraph After["With Assume Pattern (Non-blocking)"]
        A1["Scheduler decides<br/>Pod A → Node 1"]
        A2["AssumePod()<br/>cache updates immediately"]
        A3["Schedule Pod B, C, D...<br/>using fresh cache"]
        A4["Binding happens<br/>asynchronously"]
        A5["AddPod() confirms<br/>when API responds"]
        
        A1 --> A2 --> A3
        A4 -.->|parallel| A3
        A4 --> A5
    end
    
    Before -.->|throughput: ~50 pods/sec| After
    After -.->|throughput: ~1000+ pods/sec| Before
```

### The Core Methods Flow

```mermaid
graph LR
    A["🚀 SchedulePod<br/>Makes decision"]
    
    B["AssumePod()<br/>Optimistic commit<br/>to cache"]
    
    C{"Binding in<br/>API Server"}
    
    D["AddPod()<br/>Confirms<br/>assumption"]
    
    E["ForgetPod()<br/>Rolls back<br/>assumption"]
    
    F["FinishBinding()<br/>Marks binding<br/>complete"]
    
    A --> B
    B --> C
    C -->|Success| D
    C -->|Failure| E
    D --> F
    E --> A
    
    style B fill:#90EE90
    style D fill:#87CEEB
    style E fill:#FFB6C6
    style F fill:#FFD700
```

### Problem It Solves

| Aspect | Without Assume | With Assume |
|--------|---|---|
| **Throughput** | ~50 pods/sec | ~1000+ pods/sec |
| **Bottleneck** | API latency (10-100ms) | CPU-bound scheduling |
| **Cache Accuracy** | Stale during binding | Up-to-date during binding |
| **Double-booking** | Yes (if binding slow) | No (cached immediately) |
| **Example Timeline** | Schedule → Wait 50ms → Next pod | Schedule → Assume → Schedule (parallel) |

### Detailed Flow: 3 Pods in Sequence

```mermaid
timeline
    title Scheduler Processing with Assume Pattern
    
    section Assume Pattern (Fast)
        0ms : Pod A scheduled : Node 1
        : AssumePod(A) : cache updated
        1ms : Pod B scheduled : Node 2
        : (uses cache with A's usage)
        2ms : Pod C scheduled : Node 3
        : (uses cache with A+B's usage)
        : ...
        50ms : API confirms Pod A binding
        : AddPod(A) : transition to real
    
    section Without Assume (Slow)
        0ms : Pod A scheduled : Node 1
        : Submit binding to API
        50ms : API confirms Pod A
        : Update cache
        51ms : Pod B scheduled : Node 2
        102ms : Pod C scheduled : Node 3
```

### Key Invariant: State Expiration

```mermaid
graph LR
    subgraph TTL["TTL Example (even if ttl=0)"]
        T1["AssumePod()<br/>deadline = nil"]
        T2["FinishBinding()<br/>deadline = now + ttl"]
        T3{"deadline reached?"}
        T4["Expiration<br/>cleanup routine"]
        
        T1 --> T2 --> T3
        T3 -->|Yes| T4
        T3 -->|No, AddPod arrives| T2
    end
    
    style T1 fill:#90EE90
    style T2 fill:#FFD700
    style T4 fill:#FFB6C6
```

---

**Summary**: The Assume pattern decouples the **logical scheduling decision** (cache update) from the **physical API confirmation** (binding), preventing head-of-line blocking and achieving **20x throughput improvement** in pod scheduling.