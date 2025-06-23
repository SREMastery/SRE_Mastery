---
title: What is Kubernetes?
---

# What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

## Key Concepts

### Container Orchestration

Kubernetes manages containers across multiple hosts, providing:

- **Automated deployment and scaling**
- **Load balancing and service discovery**
- **Self-healing capabilities**
- **Configuration management**

### Why Kubernetes?

- **Portability**: Run anywhere - on-premises, public cloud, or hybrid
- **Scalability**: Scale applications up or down automatically
- **High Availability**: Ensure applications are always running
- **Resource Efficiency**: Optimize resource utilization

## Kubernetes Architecture

Kubernetes follows a master-worker architecture:

### Control Plane (Master Node)

- **API Server**: Entry point for all cluster operations
- **etcd**: Distributed key-value store for cluster data
- **Scheduler**: Assigns pods to nodes
- **Controller Manager**: Manages cluster state

### Worker Nodes

- **Kubelet**: Agent that runs on each node
- **Container Runtime**: Runs containers (Docker, containerd)
- **Kube-proxy**: Network proxy for service communication

## Next Steps

Continue to [Why Kubernetes?](why-kubernetes.md) to understand the benefits in detail.
