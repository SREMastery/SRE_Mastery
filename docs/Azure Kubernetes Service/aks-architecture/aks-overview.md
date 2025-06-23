---
title: Azure Kubernetes Service Overview
---

# Azure Kubernetes Service (AKS) Overview

Azure Kubernetes Service (AKS) is Microsoft's managed Kubernetes service that simplifies deploying, managing, and scaling containerized applications.

## What is AKS?

AKS is a fully managed Kubernetes service that:

- **Eliminates operational overhead** of managing Kubernetes control plane
- **Provides enterprise-grade security** and compliance
- **Integrates seamlessly** with Azure services
- **Offers cost optimization** through managed infrastructure

## Key Benefits

### 1. **Managed Control Plane**

- **Zero Management**: Azure handles control plane updates and maintenance
- **High Availability**: Multi-AZ control plane with 99.95% SLA
- **Security**: Automated security patches and updates
- **Monitoring**: Built-in monitoring and logging

### 2. **Azure Integration**

- **Identity**: Azure Active Directory (AAD) integration
- **Networking**: Azure Virtual Network integration
- **Storage**: Azure Disk and Azure Files support
- **Security**: Azure Key Vault integration

### 3. **Cost Optimization**

- **Pay for Nodes**: Only pay for worker nodes
- **Auto-scaling**: Cluster and node auto-scaling
- **Spot Instances**: Use spot instances for cost savings
- **Reserved Instances**: Long-term cost commitments

## AKS Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Azure Kubernetes Service                  │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │  Managed        │    │  User Managed   │                 │
│  │  Control Plane  │    │  Worker Nodes   │                 │
│  │  (Free)         │    │  (Paid)         │                 │
│  └─────────────────┘    └─────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

## Service Tiers

### 1. **Free Tier**

- **Control Plane**: Fully managed by Azure
- **Worker Nodes**: User manages and pays for
- **SLA**: 99.95% uptime SLA
- **Support**: Community support

### 2. **Premium Tier**

- **Control Plane**: Enhanced management
- **Worker Nodes**: Advanced features
- **SLA**: 99.95% uptime SLA
- **Support**: Microsoft support plans

## Regional Availability

AKS is available in:

- **60+ regions** worldwide
- **Multi-region** deployment support
- **Disaster recovery** capabilities
- **Compliance** with local regulations

## Use Cases

### 1. **Microservices Applications**

- **Scalable**: Auto-scaling based on demand
- **Resilient**: Self-healing capabilities
- **Isolated**: Namespace-based isolation

### 2. **DevOps and CI/CD**

- **Integration**: Azure DevOps integration
- **Automation**: Automated deployment pipelines
- **Testing**: Blue-green and canary deployments

### 3. **Machine Learning**

- **GPU Support**: GPU-enabled node pools
- **Batch Processing**: Large-scale data processing
- **Model Serving**: Real-time inference

## Next Steps

Learn about [AKS Components and Architecture](aks-components.md) to understand the technical implementation.
