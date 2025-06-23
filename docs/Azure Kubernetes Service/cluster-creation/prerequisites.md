---
title: Prerequisites for AKS
---

# Prerequisites for AKS

Before creating an Azure Kubernetes Service (AKS) cluster, ensure you have the necessary prerequisites in place.

## Azure Account Requirements

### 1. **Azure Subscription**

- **Active Subscription**: Valid Azure subscription
- **Resource Provider**: Microsoft.ContainerService registered
- **Quotas**: Sufficient vCPU and memory quotas
- **Billing**: Payment method configured

### 2. **Azure CLI Installation**

```bash
# Install Azure CLI
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Verify installation
az --version

# Login to Azure
az login
```

### 3. **Kubectl Installation**

```bash
# Install kubectl
az aks install-cli

# Verify installation
kubectl version --client
```

## Resource Requirements

### 1. **Resource Group**

- **Location**: Choose appropriate Azure region
- **Permissions**: Contributor or Owner role
- **Naming**: Follow naming conventions

### 2. **Virtual Network**

- **Subnet**: Dedicated subnet for AKS nodes
- **Address Space**: Sufficient IP range
- **Network Security Groups**: Configured rules
- **Route Tables**: Custom routing if needed

### 3. **Service Principal**

```bash
# Create service principal
az ad sp create-for-rbac --name "aks-sp" --skip-assignment

# Output includes:
# - appId (client_id)
# - password (client_secret)
# - tenant (tenant_id)
```

## Network Requirements

### 1. **IP Address Planning**

- **Pod CIDR**: Default 10.244.0.0/16
- **Service CIDR**: Default 10.96.0.0/12
- **DNS Service IP**: Default 10.96.0.10
- **Docker Bridge**: Default 172.17.0.1/16

### 2. **Network Connectivity**

- **Internet Access**: For pulling container images
- **Azure Services**: For Azure integrations
- **On-premises**: VPN or ExpressRoute if needed
- **DNS**: Proper DNS resolution

## Security Requirements

### 1. **Azure Active Directory**

- **Tenant**: Active Azure AD tenant
- **Users**: Users with appropriate permissions
- **Groups**: Security groups for RBAC
- **Service Principals**: For automation

### 2. **Key Vault** (Optional)

- **Secrets**: Application secrets
- **Certificates**: TLS certificates
- **Keys**: Encryption keys
- **Access Policies**: For AKS access

## Storage Requirements

### 1. **Container Registry**

- **Azure Container Registry**: For private images
- **Docker Hub**: For public images
- **Quay**: Alternative registry
- **Authentication**: Registry credentials

### 2. **Persistent Storage**

- **Azure Disks**: For persistent volumes
- **Azure Files**: For shared storage
- **Storage Classes**: Dynamic provisioning
- **Backup Strategy**: Data protection

## Monitoring Requirements

### 1. **Log Analytics Workspace**

- **Location**: Same region as AKS
- **Retention**: Configure retention period
- **Access**: Proper access controls
- **Cost Management**: Monitor usage

### 2. **Application Insights** (Optional)

- **Instrumentation**: Application monitoring
- **Metrics**: Custom metrics
- **Alerts**: Performance alerts
- **Dashboards**: Custom dashboards

## Cost Considerations

### 1. **Node Sizing**

- **CPU**: Minimum 2 vCPUs recommended
- **Memory**: Minimum 4 GB recommended
- **Storage**: OS disk size
- **Spot Instances**: For cost optimization

### 2. **Reserved Instances**

- **1-year RI**: 20-40% savings
- **3-year RI**: 60-80% savings
- **Flexible**: Size flexibility
- **Regional**: Region-specific

## Checklist

### Before Creating AKS Cluster

- [ ] Azure subscription active
- [ ] Azure CLI installed and configured
- [ ] Kubectl installed
- [ ] Resource group created
- [ ] Virtual network configured
- [ ] Service principal created
- [ ] Network security groups configured
- [ ] Container registry access
- [ ] Log Analytics workspace created
- [ ] Cost budget set

## Next Steps

Once prerequisites are met, proceed to [Creating AKS Cluster via Azure Portal](azure-portal.md).
