---
title: Introduction to Kubectl
---

# Introduction to Kubectl

Kubectl is the command-line tool for interacting with Kubernetes clusters. It's the primary interface for managing Kubernetes resources and applications.

## What is Kubectl?

Kubectl (kube-control) is:

- **Command-line interface** for Kubernetes
- **Primary tool** for cluster administration
- **Resource management** utility
- **Debugging and troubleshooting** tool

## Kubectl Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Kubectl       │───▶│   API Server    │───▶│   Kubernetes    │
│   Client        │    │                 │    │   Cluster       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Communication Flow

1. **Kubectl** sends HTTP requests to API Server
2. **API Server** validates and processes requests
3. **Cluster** executes the requested operations
4. **Response** returned to kubectl

## Basic Syntax

```bash
kubectl [command] [TYPE] [NAME] [flags]
```

### Command Types

- **get**: List resources
- **describe**: Detailed resource information
- **create**: Create resources
- **apply**: Apply configuration files
- **delete**: Delete resources
- **edit**: Edit resources
- **exec**: Execute commands in containers
- **logs**: View container logs
- **port-forward**: Forward ports to local machine

## Resource Types

### Workload Resources

- **pods**: Individual containers
- **deployments**: Replicated applications
- **replicasets**: Pod replicas
- **statefulsets**: Stateful applications
- **daemonsets**: Node-level workloads
- **jobs**: One-time tasks
- **cronjobs**: Scheduled jobs

### Network Resources

- **services**: Network endpoints
- **ingress**: HTTP/HTTPS routing
- **networkpolicies**: Network security

### Storage Resources

- **persistentvolumes**: Storage volumes
- **persistentvolumeclaims**: Storage requests
- **storageclasses**: Storage provisioners

### Configuration Resources

- **configmaps**: Configuration data
- **secrets**: Sensitive data
- **namespaces**: Resource isolation

## Context and Configuration

### Kubeconfig File

```yaml
apiVersion: v1
kind: Config
clusters:
  - name: my-cluster
    cluster:
      server: https://my-cluster.com
contexts:
  - name: my-context
    context:
      cluster: my-cluster
      user: my-user
current-context: my-context
users:
  - name: my-user
    user:
      token: my-token
```

### Context Management

```bash
# List contexts
kubectl config get-contexts

# Switch context
kubectl config use-context my-context

# View current context
kubectl config current-context
```

## Common Commands

### 1. **Getting Information**

```bash
# List all pods
kubectl get pods

# List pods in specific namespace
kubectl get pods -n kube-system

# Get detailed information
kubectl describe pod my-pod

# Get resource in YAML format
kubectl get pod my-pod -o yaml
```

### 2. **Creating Resources**

```bash
# Create from file
kubectl apply -f deployment.yaml

# Create from URL
kubectl apply -f https://example.com/deployment.yaml

# Create from stdin
kubectl apply -f -
```

### 3. **Managing Resources**

```bash
# Scale deployment
kubectl scale deployment my-deployment --replicas=3

# Update image
kubectl set image deployment/my-deployment my-container=nginx:1.19

# Rollout status
kubectl rollout status deployment/my-deployment
```

## Output Formats

### 1. **Wide Output**

```bash
kubectl get pods -o wide
```

### 2. **YAML Output**

```bash
kubectl get pod my-pod -o yaml
```

### 3. **JSON Output**

```bash
kubectl get pod my-pod -o json
```

### 4. **Custom Columns**

```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

## Best Practices

### 1. **Use Namespaces**

- Organize resources logically
- Apply resource quotas
- Implement RBAC policies

### 2. **Use Labels and Selectors**

- Organize resources with labels
- Use selectors for targeting
- Implement consistent labeling

### 3. **Use Configuration Files**

- Version control configurations
- Apply consistent settings
- Enable rollback capabilities

### 4. **Use Resource Limits**

- Set CPU and memory limits
- Prevent resource exhaustion
- Enable fair resource sharing

## Next Steps

Learn about [Kubectl Installation and Setup](installation.md) to get started with kubectl.
