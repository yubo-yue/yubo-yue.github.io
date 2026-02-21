---
layout: post
title:  "Kubernetes image service and runtime service"
date: 2026-02-21 01:12:13
categories: kubernetes image service and runtime service
---

Below is a concise view of the **kubelet** architecture and where **`remoteRuntimeService`** fits.

### High-level architecture

- **Kubelet** is the node agent. It reconciles **desired Pod state** (from the API server) with **actual state** (from the container runtime).
- It relies on **CRI** (Container Runtime Interface) over **gRPC**.
- **`remoteRuntimeService`** is the client-side adapter that talks to the runtime (containerd, CRI-O, etc.) via gRPC.

````mermaid
graph TD
  A[API Server] -->|Watches Pods| B[Kubelet]
  B --> C[Pod Workers]
  B --> D[SyncPod / Reconcile Loop]
  D --> E[Status Manager]
  D --> F[Probe Manager]
  D --> G[Volume Manager]
  D --> H[Runtime Manager]

  H --> I[remoteRuntimeService ]
  I -->|gRPC| J[Container Runtime]
  J --> K[Containers]

  subgraph CRI
    I
    J
  end
````

---

````mermaid
flowchart LR
  subgraph Node["Kubernetes Node"]
    Kubelet["kubelet<br>(pkg/kubelet + kuberuntime)"]

    subgraph CRIClient["CRI client (k8s.io/cri-client/pkg)"]
      RRS["remoteRuntimeService<br>implements internalapi.RuntimeService<br>(gRPC client)"]
      RIS["remoteImageService<br>implements internalapi.ImageManagerService<br>(gRPC client)"]
    end

    Kubelet -->|Runtime ops: <br> RunPodSandbox, Create/Start/Stop,<br>Exec, Logs...| RRS
    Kubelet -->|Image ops:<br>PullImage, ListImages,<br>ImageStatus, RemoveImage,<br>ImageFsInfo| RIS

    RRS -->|gRPC over unix socket<br>--container-runtime-endpoint| CRIServer
    RIS -->|gRPC over unix socket<br>--image-service-endpoint | CRIServer

    subgraph Containerd["containerd (CRI server + runtime mgmt)"]
      CRIServer["containerd CRI plugin<br>RuntimeService + ImageService"]
      ImgStore["Image/content store + snapshotter"]
      Shim["containerd-shim-runc-v2"]
    end

    CRIServer --> ImgStore
    CRIServer --> Shim --> Runc["runc (OCI runtime)"] --> Kernel["Linux kernel<br>namespaces/cgroups/mount"]
  end
````

Kubelet、Image Service、Runtime Service 与 containerd / runc 的关系
kubelet 视角：我只认 CRI

kubelet 通过配置的 endpoint 去连 CRI：

--container-runtime-endpoint：runtime service 的地址（Linux 下几乎都是 unix socket）

--image-service-endpoint：image service 的地址；很多 runtime 把 image/runtime 都放同一个 socket 上（containerd 就是典型）。

containerd 视角：我既是 CRI Server，也是 OCI 管家

常见链路是：

kubelet（CRI client）→ containerd（CRI server）→ containerd-shim-runc-v2 → runc → Linux 内核（namespaces/cgroups）

其中：

containerd：负责镜像管理（content store、snapshotter）、容器元数据与生命周期编排，并提供 CRI 插件对外说“我就是 RuntimeService/ImageService”。（CRI 客户端仓库说明里也强调 CRI 是 kubelet 和 runtime 之间的 gRPC API）。

runc：OCI runtime，干的是“把容器真正创建出来”的脏活累活（clone、setns、cgroup、mount…）。

### Where `remoteRuntimeService` sits

- It **creates and manages the gRPC connection** (timeouts, backoff, tracing).
- It **validates the runtime connection** during initialization.
- It **exposes the CRI RuntimeService API** used by kubelet’s runtime manager.

### Gotchas (common pitfalls)

- **Backoff + timeout**: Dial timeouts and backoff settings can hide slow startup issues.
- **Tracing**: With a `nil` tracer provider, propagation still occurs, but no spans are recorded.
- **Log reduction**: `logReduction` prevents log spam on repeated errors—useful but can hide repeated failures if you’re not looking at metrics.

If you want a deeper call-chain diagram (e.g., `SyncPod()` → `RuntimeService.RunPodSandbox()`), say which path you want traced.