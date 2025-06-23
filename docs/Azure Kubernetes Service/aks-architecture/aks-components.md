---
title: AKS Components and Architecture
---

# AKS Components and Architecture

AKS implements Kubernetes with Azure-specific components and integrations that enhance functionality and security.

## AKS Architecture Components

### 1. **Managed Control Plane**

- **API Server**: Azure-managed Kubernetes API server
- **etcd**: Azure-managed distributed key-value store
- **Scheduler**: Azure-managed pod scheduler
- **Controller Manager**: Azure-managed controllers

### 2. **Worker Node Pools**

- **Virtual Machine Scale Sets**: Auto-scaling node groups
- **Container Runtime**: containerd (default) or Docker
- **Kubelet**: Node agent for pod management
- **Kube-proxy**: Network proxy for services

### 3. **Azure-Specific Components**

- **Azure CNI**: Container Network Interface
- **Azure Disk CSI**: Container Storage Interface
- **Azure Monitor**: Built-in monitoring
- **Azure Key Vault**: Secret management

## Node Pool Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AKS Cluster                              │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │  System Node    │    │  User Node      │                 │
│  │  Pool           │    │  Pool           │                 │
│  │  (Required)     │    │  (Optional)     │                 │
│  └─────────────────┘    └─────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

### System Node Pool

- **Purpose**: Runs system pods (kube-system namespace)
- **Minimum**: 1 node required
- **Scaling**: Manual or auto-scaling
- **Taints**: Prevents user workloads

### User Node Pool

- **Purpose**: Runs application workloads
- **Scaling**: Manual, auto-scaling, or spot instances
- **Types**: General purpose, memory optimized, compute optimized
- **GPU**: GPU-enabled node pools available

## Networking Components

### 1. **Azure CNI**

- **Pod Networking**: Direct pod-to-pod communication
- **VNet Integration**: Pods get VNet IP addresses
- **Network Policies**: Kubernetes network policies
- **Service Mesh**: Istio and Linkerd support

### 2. **Load Balancer**

- **Azure Load Balancer**: Standard or Basic SKU
- **Application Gateway**: Layer 7 load balancing
- **Front Door**: Global load balancing
- **Traffic Manager**: DNS-based load balancing

## Storage Components

### 1. **Azure Disk CSI**

- **Managed Disks**: Premium and Standard SSDs
- **Dynamic Provisioning**: Automatic disk creation
- **Snapshot Support**: Point-in-time backups
- **Encryption**: Azure Disk Encryption

### 2. **Azure Files CSI**

- **SMB Shares**: Windows and Linux compatibility
- **NFS Support**: Network File System
- **Premium Tier**: High-performance storage
- **Backup**: Azure Backup integration

## Security Components

### 1. **Azure Active Directory**

- **RBAC**: Role-based access control
- **Service Principals**: Application authentication
- **Managed Identity**: Azure resource authentication
- **OIDC**: OpenID Connect integration

### 2. **Azure Key Vault**

- **Secret Management**: Kubernetes secrets
- **Certificate Management**: TLS certificates
- **Key Management**: Encryption keys
- **CSI Driver**: Direct integration

## Monitoring Components

### 1. **Azure Monitor**

- **Container Insights**: Pod and node monitoring
- **Log Analytics**: Centralized logging
- **Metrics**: Performance metrics
- **Alerts**: Automated alerting

### 2. **Prometheus Integration**

- **Custom Metrics**: Application metrics
- **Grafana**: Visualization dashboards
- **Alert Manager**: Advanced alerting
- **Service Discovery**: Automatic target discovery

## Next Steps

Explore [AKS Networking Architecture](aks-networking.md) to understand network configuration.
