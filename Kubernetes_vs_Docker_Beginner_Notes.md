# Kubernetes (K8s) vs Docker

## What is Kubernetes?

**Kubernetes (K8s)** is an open-source **container orchestration platform** used to deploy, manage, scale, and maintain containerized applications.

Docker helps you **create and run containers**.

Kubernetes helps you **manage many containers across one or more machines**.

## What Problem Does Kubernetes Solve?

Docker works very well for running individual containers:

```bash
docker run my-app
```

But imagine an application with:

```text
100 containers
10 servers
Multiple application versions
Automatic recovery
Load balancing
```

Managing this manually becomes difficult. Kubernetes automates these responsibilities.

## What Docker Does

Docker is primarily concerned with containers. It can:

- Build container images
- Run containers
- Stop and start containers
- Package applications with dependencies
- Manage individual containers

Example:

```bash
docker build -t my-app .
docker run my-app
```

## What Kubernetes Does

Kubernetes manages containers at a larger scale. It can:

- Deploy containers
- Scale applications
- Restart failed containers
- Perform rolling updates
- Provide service discovery
- Load balance traffic
- Manage multiple machines
- Maintain the desired number of application instances

## Docker vs Kubernetes

| Docker | Kubernetes |
|---|---|
| Container platform | Container orchestration platform |
| Builds and runs containers | Manages containers |
| Commonly used on individual machines | Designed for clusters |
| Scaling is mostly manual | Supports automated/command-based scaling |
| Basic container networking | Service discovery and load balancing |
| Manual recovery | Automatic self-healing |
| Great for development and small deployments | Great for large/production deployments |

## Example: Application Without Kubernetes

Suppose your application needs 3 instances:

```bash
docker run my-app
docker run my-app
docker run my-app
```

If one container crashes, you need to notice the failure and start another one.

If traffic increases and you need 10 instances, you have to manage that scaling yourself.

## The Same Example With Kubernetes

You can tell Kubernetes:

```yaml
replicas: 3
```

Kubernetes maintains the desired state.

If one Pod crashes:

```text
Before:

Pod 1
Pod 2
Pod 3

Pod 2 crashes

Pod 1
Pod 3
```

Kubernetes creates a replacement:

```text
Pod 1
Pod 3
Pod 4
```

## Scaling

If your application normally needs 3 replicas:

```bash
kubectl scale deployment my-app --replicas=10
```

Kubernetes creates additional Pods.

## Rolling Updates

Suppose the application uses:

```text
my-app:v1
```

and you want:

```text
my-app:v2
```

Kubernetes can perform a rolling update:

```text
v1 v1 v1 v1
    ↓
v2 v1 v1 v1
    ↓
v2 v2 v1 v1
    ↓
v2 v2 v2 v1
    ↓
v2 v2 v2 v2
```

This can reduce downtime during deployments.

## Self-Healing

Suppose Kubernetes is configured to maintain 5 Pods:

```text
Desired = 5

Pod 1
Pod 2
Pod 3
Pod 4
Pod 5
```

If Pod 3 fails, Kubernetes detects that the actual state differs from the desired state and creates a replacement.

```text
Pod 1
Pod 2
Pod 4
Pod 5
Pod 6  <- replacement
```

## What Problem Did Kubernetes Solve?

Docker is excellent at **containerization**. The challenge appears when you have many containers and multiple machines.

For example:

```text
10 Servers
100 Containers
Multiple Services
Multiple Versions
Thousands of Users
```

You need to answer:

- Where should containers run?
- What happens if a container crashes?
- How many replicas should run?
- How do users reach the correct application?
- How do containers communicate?
- How do we deploy new versions?
- How do we distribute traffic?
- How do we manage containers across multiple machines?

Kubernetes provides a platform to automate these responsibilities.

## Important Clarification

Kubernetes did **not simply replace Docker**.

They solve different problems.

A simplified architecture is:

```text
Kubernetes
     ↓
Container Runtime
     ↓
Containers
```

Modern Kubernetes commonly uses container runtimes such as **containerd** or **CRI-O**.

Docker remains widely used for building images and local development.

## Docker Compose vs Kubernetes

Docker Compose is useful for running multiple containers on a single machine:

```text
Frontend
   ↓
Backend
   ↓
Database
```

Kubernetes is designed for larger environments:

```text
Server 1 → Pods
Server 2 → Pods
Server 3 → Pods
Server 4 → Pods
       ↓
  Kubernetes
```

## Simple Analogy

Think of Docker as a **shipping container system**.

Docker gives you standardized containers that package your applications.

Kubernetes is like the **port and logistics management system** that decides:

- Where containers go
- How many are needed
- What happens when one fails
- How traffic is distributed

## Important Kubernetes Concepts

### Pod

The smallest deployable unit in Kubernetes. A Pod usually contains one application container.

```text
Pod
└── Container
```

### Deployment

Defines how an application should be deployed and maintained.

Example:

```yaml
replicas: 3
```

### Service

Provides a stable way to access a group of Pods.

```text
Users
  ↓
Service
  ↓
├── Pod
├── Pod
└── Pod
```

### Cluster

A collection of machines managed by Kubernetes.

```text
Kubernetes Cluster
│
├── Node
│   ├── Pod
│   └── Pod
│
├── Node
│   ├── Pod
│   └── Pod
│
└── Node
    └── Pod
```

## Final Comparison

```text
Docker

Application
     ↓
Container
     ↓
Run Container
```

```text
Kubernetes

Application
     ↓
Container Image
     ↓
Pod
     ↓
Deployment
     ↓
Service
     ↓
Kubernetes Cluster
     ↓
Multiple Machines
```

## Key Takeaways

- **Docker** is primarily used to build and run containers.
- **Kubernetes** is used to orchestrate and manage containers.
- Docker is excellent for local development and individual containers.
- Kubernetes becomes valuable when applications require many containers, replicas, or multiple machines.
- Kubernetes provides scaling, self-healing, service discovery, load balancing, and rolling deployments.
- Kubernetes does not simply replace Docker; they operate at different layers of the container ecosystem.
