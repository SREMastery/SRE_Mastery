---
title: Advanced Pod Management
---

# Advanced Pod Management

Advanced pod management techniques for production Kubernetes environments, including scheduling, security, and optimization strategies.

## Advanced Pod Scheduling

### 1. **Node Selectors**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  nodeSelector:
    disk: ssd
    environment: production
  containers:
    - name: nginx
      image: nginx:latest
```

### 2. **Node Affinity**

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: kubernetes.io/e2e-az-name
                operator: In
                values:
                  - e2e-az1
                  - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 1
          preference:
            matchExpressions:
              - key: another-node-label-key
                operator: In
                values:
                  - another-node-label-value
```

### 3. **Pod Affinity and Anti-Affinity**

```yaml
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: security
                operator: In
                values:
                  - S1
          topologyKey: failure-domain.beta.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 100
          podAffinityTerm:
            labelSelector:
              matchExpressions:
                - key: security
                  operator: In
                  values:
                    - S2
            topologyKey: kubernetes.io/hostname
```

## Advanced Pod Security

### 1. **Security Context**

```yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
    - name: nginx
      image: nginx:latest
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop:
            - ALL
```

### 2. **Pod Security Standards**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
    - name: nginx
      image: nginx:latest
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop:
            - ALL
```

### 3. **Pod Disruption Budgets**

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: nginx-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: nginx
```

## Advanced Resource Management

### 1. **Resource Quotas**

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
    pods: "10"
```

### 2. **Limit Ranges**

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: limit-range
spec:
  limits:
    - default:
        memory: 512Mi
        cpu: 500m
      defaultRequest:
        memory: 256Mi
        cpu: 250m
      type: Container
```

### 3. **Quality of Service Classes**

```yaml
# Guaranteed QoS
spec:
  containers:
  - name: nginx
    image: nginx:latest
    resources:
      requests:
        memory: "100Mi"
        cpu: "100m"
      limits:
        memory: "100Mi"
        cpu: "100m"

# Burstable QoS
spec:
  containers:
  - name: nginx
    image: nginx:latest
    resources:
      requests:
        memory: "100Mi"
        cpu: "100m"

# BestEffort QoS
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

## Advanced Pod Lifecycle

### 1. **Graceful Shutdown**

```yaml
spec:
  terminationGracePeriodSeconds: 30
  containers:
    - name: nginx
      image: nginx:latest
      lifecycle:
        preStop:
          exec:
            command:
              [
                "/bin/sh",
                "-c",
                "nginx -s quit; while killall -0 nginx; do sleep 1; done",
              ]
```

### 2. **Post-Start Hooks**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      lifecycle:
        postStart:
          exec:
            command:
              - /bin/sh
              - -c
              - echo "Container started" > /tmp/startup.log
```

### 3. **Pod Priority and Preemption**

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "High priority pods"
---
apiVersion: v1
kind: Pod
metadata:
  name: high-priority-pod
spec:
  priorityClassName: high-priority
  containers:
    - name: nginx
      image: nginx:latest
```

## Advanced Pod Networking

### 1. **Host Aliases**

```yaml
spec:
  hostAliases:
    - ip: "192.168.1.100"
      hostnames:
        - "foo.local"
        - "bar.local"
  containers:
    - name: nginx
      image: nginx:latest
```

### 2. **DNS Configuration**

```yaml
spec:
  dnsPolicy: ClusterFirst
  dnsConfig:
    nameservers:
      - 1.2.3.4
    searches:
      - ns1.svc.cluster-domain.example
      - my.dns.search.suffix
    options:
      - name: ndots
        value: "2"
      - name: edns0
  containers:
    - name: nginx
      image: nginx:latest
```

### 3. **Network Policies**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

## Advanced Pod Monitoring

### 1. **Custom Metrics**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      env:
        - name: METRICS_PORT
          value: "9090"
```

### 2. **Resource Monitoring**

```bash
# Monitor pod resources
kubectl top pod nginx-pod

# Monitor pod metrics
kubectl get --raw /apis/metrics.k8s.io/v1beta1/namespaces/default/pods/nginx-pod

# Monitor pod events
kubectl get events --field-selector involvedObject.name=nginx-pod
```

### 3. **Health Check Endpoints**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx:latest
      livenessProbe:
        httpGet:
          path: /health
          port: 80
          httpHeaders:
            - name: Custom-Header
              value: "health-check"
        initialDelaySeconds: 30
        periodSeconds: 10
        timeoutSeconds: 5
        failureThreshold: 3
        successThreshold: 1
```

## Advanced Pod Troubleshooting

### 1. **Debugging Tools**

```bash
# Debug pod with ephemeral container
kubectl debug nginx-pod -it --image=busybox --target=nginx

# Check pod status in detail
kubectl describe pod nginx-pod

# Get pod logs with timestamps
kubectl logs nginx-pod --timestamps --tail=100

# Execute into pod with specific shell
kubectl exec -it nginx-pod -- /bin/bash
```

### 2. **Pod Events Analysis**

```bash
# Get all events for a pod
kubectl get events --field-selector involvedObject.name=nginx-pod

# Get events by type
kubectl get events --field-selector type=Warning

# Get events by namespace
kubectl get events -n default
```

### 3. **Resource Usage Analysis**

```bash
# Check pod resource usage
kubectl top pod nginx-pod

# Check pod resource requests and limits
kubectl get pod nginx-pod -o jsonpath='{.spec.containers[*].resources}'

# Check pod QoS class
kubectl get pod nginx-pod -o jsonpath='{.status.qosClass}'
```

## Best Practices

### 1. **Pod Design**

- **Single Responsibility**: One primary container per pod
- **Resource Limits**: Always set resource requests and limits
- **Security**: Run containers as non-root users
- **Health Checks**: Implement comprehensive health checks

### 2. **Pod Management**

- **Use Deployments**: Don't create pods directly
- **Labels**: Use consistent labeling strategy
- **Namespaces**: Organize pods in namespaces
- **Monitoring**: Monitor pod health and performance

### 3. **Pod Security**

- **Security Context**: Configure appropriate security contexts
- **Network Policies**: Implement network policies
- **RBAC**: Use role-based access control
- **Secrets**: Use Kubernetes secrets for sensitive data

## Next Steps

Learn about [Advanced Deployment Strategies](deployment-strategies.md) for managing application deployments.
