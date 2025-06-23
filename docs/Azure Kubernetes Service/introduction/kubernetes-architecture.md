---
title: Kubernetes Architecture Overview
---

# Kubernetes Architecture Overview

Kubernetes follows a distributed system architecture with clear separation of concerns between control plane and worker nodes.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster                        │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │   Control Plane │    │   Worker Nodes  │                 │
│  │   (Master)      │    │   (Minions)     │                 │
│  └─────────────────┘    └─────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

## Control Plane Components

### 1. **API Server**

- **Purpose**: Entry point for all cluster operations
- **Function**: Validates and processes REST API requests
- **Communication**: All components communicate through API Server
- **Security**: Handles authentication and authorization

### 2. **etcd**

- **Purpose**: Distributed key-value store
- **Function**: Stores all cluster data and configuration
- **Consistency**: Ensures data consistency across cluster
- **Backup**: Critical for disaster recovery

### 3. **Scheduler**

- **Purpose**: Assigns pods to nodes
- **Function**: Watches for unscheduled pods
- **Criteria**: Considers resource requirements, constraints, policies
- **Optimization**: Balances load across nodes

### 4. **Controller Manager**

- **Purpose**: Manages cluster state
- **Components**:
  - Node Controller: Monitors node health
  - Replication Controller: Maintains pod replicas
  - Endpoints Controller: Populates endpoint objects
  - Service Account & Token Controllers: Handle authentication

## Worker Node Components

### 1. **Kubelet**

- **Purpose**: Primary node agent
- **Function**: Manages pod lifecycle on the node
- **Communication**: Reports node and pod status to API Server
- **Health**: Ensures containers are running

### 2. **Container Runtime**

- **Purpose**: Runs containers
- **Examples**: Docker, containerd, CRI-O
- **Function**: Pulls images, starts/stops containers
- **Interface**: Implements Container Runtime Interface (CRI)

### 3. **Kube-proxy**

- **Purpose**: Network proxy
- **Function**: Maintains network rules and connectivity
- **Services**: Enables service abstraction
- **Load Balancing**: Distributes traffic to pods

## Data Flow

### Pod Creation Process

1. **User submits** pod specification via kubectl
2. **API Server** validates and stores in etcd
3. **Scheduler** assigns pod to appropriate node
4. **Kubelet** on target node creates pod
5. **Container Runtime** starts containers
6. **Kube-proxy** sets up networking

### Service Discovery

1. **Service** created with selector
2. **Endpoints Controller** watches for pod changes
3. **Kube-proxy** updates iptables rules
4. **DNS** service provides name resolution

## Networking Model

### Pod Network

- **CNI**: Container Network Interface
- **Overlay**: Network virtualization
- **Pods**: Each pod gets unique IP
- **Services**: Virtual IP for service discovery

### Service Types

- **ClusterIP**: Internal cluster access
- **NodePort**: External access via node port
- **LoadBalancer**: Cloud load balancer integration
- **ExternalName**: DNS-based service discovery

## Next Steps

Now that you understand Kubernetes architecture, explore [AKS Architecture](../aks-architecture/aks-overview.md) to see how Azure implements it.
