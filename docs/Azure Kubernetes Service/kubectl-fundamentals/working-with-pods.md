---
title: Working with Pods
---

# Working with Pods

Pods are the smallest deployable units in Kubernetes. Understanding how to work with pods is fundamental to Kubernetes administration.

## What are Pods?

Pods are:

- **Smallest unit** in Kubernetes
- **One or more containers** sharing resources
- **Ephemeral** - can be recreated at any time
- **Atomic unit** - all containers in a pod live and die together

## Pod Lifecycle

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Pending   │───▶│  Running    │───▶│  Succeeded  │    │   Failed    │
│             │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### Pod States

- **Pending**: Pod accepted, containers not yet running
- **Running**: Pod bound to node, all containers running
- **Succeeded**: All containers terminated successfully
- **Failed**: At least one container terminated in failure
- **Unknown**: Pod state cannot be determined

## Creating Pods

### 1. **Imperative Commands**

```bash
# Create a simple pod
kubectl run nginx-pod --image=nginx:latest

# Create pod with specific namespace
kubectl run nginx-pod --image=nginx:latest -n my-namespace

# Create pod with labels
kubectl run nginx-pod --image=nginx:latest --labels=app=nginx,env=prod
```

### 2. **Declarative YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    env: production
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

### 3. **Apply Configuration**

```bash
# Create pod from file
kubectl apply -f pod.yaml

# Create from URL
kubectl apply -f https://example.com/pod.yaml
```

## Managing Pods

### 1. **Viewing Pods**

```bash
# List all pods
kubectl get pods

# List pods with more details
kubectl get pods -o wide

# List pods in specific namespace
kubectl get pods -n kube-system

# Watch pods in real-time
kubectl get pods -w
```

### 2. **Describing Pods**

```bash
# Get detailed information
kubectl describe pod nginx-pod

# Get pod events
kubectl get events --field-selector involvedObject.name=nginx-pod
```

### 3. **Pod Logs**

```bash
# Get logs from pod
kubectl logs nginx-pod

# Follow logs in real-time
kubectl logs -f nginx-pod

# Get logs from specific container
kubectl logs nginx-pod -c nginx

# Get logs with timestamps
kubectl logs nginx-pod --timestamps
```

## Pod Configuration

### 1. **Resource Limits**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

### 2. **Environment Variables**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      env:
        - name: NGINX_PORT
          value: "80"
        - name: NGINX_HOST
          valueFrom:
            configMapKeyRef:
              name: nginx-config
              key: host
```

### 3. **Volume Mounts**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      volumeMounts:
        - name: nginx-config
          mountPath: /etc/nginx/conf.d
  volumes:
    - name: nginx-config
      configMap:
        name: nginx-config
```

## Multi-Container Pods

### 1. **Sidecar Pattern**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-app
spec:
  containers:
    - name: web
      image: nginx:latest
      ports:
        - containerPort: 80
    - name: log-aggregator
      image: fluentd:latest
      volumeMounts:
        - name: logs
          mountPath: /var/log
  volumes:
    - name: logs
      emptyDir: {}
```

### 2. **Init Containers**

```yaml
spec:
  initContainers:
    - name: init-db
      image: busybox:latest
      command:
        [
          "sh",
          "-c",
          "until nslookup mysql; do echo waiting for mysql; sleep 2; done;",
        ]
  containers:
    - name: app
      image: my-app:latest
```

## Pod Health Checks

### 1. **Liveness Probe**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      livenessProbe:
        httpGet:
          path: /health
          port: 80
        initialDelaySeconds: 30
        periodSeconds: 10
```

### 2. **Readiness Probe**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      readinessProbe:
        httpGet:
          path: /ready
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 5
```

### 3. **Startup Probe**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      startupProbe:
        httpGet:
          path: /startup
          port: 80
        failureThreshold: 30
        periodSeconds: 10
```

## Pod Networking

### 1. **Port Configuration**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - name: http
          containerPort: 80
          protocol: TCP
        - name: https
          containerPort: 443
          protocol: TCP
```

### 2. **Host Networking**

```yaml
spec:
  hostNetwork: true
  containers:
    - name: nginx
      image: nginx:latest
```

## Troubleshooting Pods

### 1. **Common Issues**

```bash
# Check pod status
kubectl get pods

# Check pod events
kubectl describe pod nginx-pod

# Check pod logs
kubectl logs nginx-pod

# Execute into pod
kubectl exec -it nginx-pod -- /bin/bash
```

### 2. **Debugging Commands**

```bash
# Copy files from pod
kubectl cp nginx-pod:/etc/nginx/nginx.conf ./nginx.conf

# Port forward to pod
kubectl port-forward nginx-pod 8080:80

# Check pod resources
kubectl top pod nginx-pod
```

## Best Practices

### 1. **Pod Design**

- **Single Responsibility**: One primary container per pod
- **Resource Limits**: Always set resource requests and limits
- **Health Checks**: Implement liveness and readiness probes
- **Security**: Run containers as non-root users

### 2. **Pod Management**

- **Use Deployments**: Don't create pods directly
- **Labels**: Use consistent labeling strategy
- **Namespaces**: Organize pods in namespaces
- **Monitoring**: Monitor pod health and performance

## Next Steps

Learn about [Working with Deployments](working-with-deployments.md) for managing pod replicas.
