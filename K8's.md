Here are the notes based on your summary. You've perfectly captured the historical evolution of deployment.

## The Evolution of Deployment

### 1. Bare Metal Servers

- **How it worked:** You bought a physical server ("bare metal"). You installed an OS, configured the network, and manually installed all application dependencies (like Java, Postgres, etc.).
    
- **The Problem:**
    
    - **Scaling:** To scale, you had to manually buy and install more hardware (RAM, CPU). This was slow and expensive.
        
    - **Maintenance:** Required 24/7 human monitoring. If a hard drive failed, someone had to physically replace it.
        
    - **Environment:** Inflexible. Difficult to run multiple applications on one server if they had conflicting dependencies.
        

---

### 2. Cloud & Virtualization (AWS, VMs)

- **The Change:** Cloud providers (like AWS) and Virtualization (VMs) solved the hardware problem.
    
- **How it worked:**
    
    - AWS managed the physical hardware. You could "rent" a Virtual Machine (VM) in minutes.
        
    - VMs (like VirtualBox or VMware) bundled the application, its dependencies, _and a full Guest OS_ (e.g., a 10GB Windows image) into a single snapshot.
        
- **The New Problem ("It works on my machine"):**
    
    - **Heavyweight:** VMs were huge (8-10 GB) because _each one_ included a full OS. They were slow to start and wasted resources.
        
    - **Inconsistency:** Even with the cloud, developers still built on their local OS, which could be different from the cloud VM, leading to the "it works on my machine" issue.
        

---

### 3. Containerization (Docker)

- **The Change:** Docker solved the "works on my machine" problem and the heavyweight VM problem.
    
- **How it worked:**
    
    - A container packages _only_ the application and its dependencies (e.g., Java).
        
    - It **shares the Host OS kernel** instead of bundling a full Guest OS.
        
- **The Benefit:** Containers are lightweight (megabytes instead of gigabytes), start in seconds, and run identically everywhere—on a laptop or in the cloud.
    
- **The New Problem (Orchestration):**
    
    - Docker is great for running _one_ container. But what about a real application with 4-5 containers (like your `Consumer`, `enricher`, `s4`, and Kafka)?
        
    - If one container crashed, you had to restart it manually.
        
    - If you needed more capacity, you had to deploy new containers manually.
        
    - Managing ("orchestrating") all these pieces became the new bottleneck.
        

---

### 4. Container Orchestration (Kubernetes)

- **The Change:** Google (who ran everything in containers internally) solved the orchestration problem.
    
- **How it worked:**
    
    - Google created a system (internally called Borg) to manage massive numbers of containers automatically.
        
    - They saw everyone else in the industry was facing the same problem, so they released an open-source version called **Kubernetes (K8s)**.
        
- **The Benefit:** K8s is the "brain" that manages your containers. You just tell it the "desired state" (e.g., "I want 3 copies of `enricher` running"), and K8s handles the rest:
    
    - **Self-healing:** If a container crashes, K8s automatically restarts it.
        
    - **Scaling:** You can tell K8s to "scale `enricher` to 10 copies," and it does it for you.
        
    - **Updates:** It handles rolling out new versions of your code with zero downtime.

---
# 1. Kubernetes High-Level Architecture

Kubernetes has **two main parts**:

```
Kubernetes Cluster
   ├ Control Plane (Brain)
   └ Worker Nodes (Machines running containers)
```

---

# 2. Control Plane (Cluster Brain)

The **control plane manages the cluster** and makes decisions.

Main components:

```
API Server
etcd
Scheduler
Controller Manager
```

---

## API Server (Entry Point)

You are correct: **API Server is the starting point of everything.**

Every component talks to Kubernetes through the **API Server**.

Example flow:

```
kubectl apply deployment.yaml
        ↓
API Server receives request
```

Responsibilities:

* Validates requests
* Updates cluster state
* Communicates with etcd
* Exposes REST API

Example request:

```
Create Deployment
Replicas = 3
Image = nginx
```

---

## etcd (Cluster Database)

You also correctly said:

> etcd stores the desired state.

More precisely:

```
etcd = distributed key-value database
```

It stores:

```
cluster configuration
pod definitions
replica counts
secrets
service configs
```

Example stored data:

```
deployment nginx
replicas = 3
image = nginx:latest
```

Important:

```
etcd stores the desired cluster state
```

---

## Scheduler

You said:

> scheduler find the best nodes

Correct.

The **scheduler decides where a pod should run**.

Example:

Cluster nodes:

```
Node1 (2 CPU free)
Node2 (6 CPU free)
Node3 (1 CPU free)
```

New pod arrives:

```
Pod requires 2 CPU
```

Scheduler decides:

```
Best node = Node2
```

Scheduler considers:

* CPU
* memory
* node labels
* taints/tolerations
* affinity rules

---

## Controller Manager

This is often missed but important.

Controllers ensure **desired state = actual state**.

Example:

Desired:

```
replicas = 3
```

Actual:

```
2 pods running
```

Controller fixes it:

```
creates 1 new pod
```

Types of controllers:

```
ReplicaSet Controller
Deployment Controller
Node Controller
Job Controller
```

---

# 3. Worker Nodes

Worker nodes are the **machines that actually run containers**.

Each worker node has:

```
Kubelet
Kube-proxy
Container Runtime
```

---

## Container Runtime

You mentioned:

> Container Runtime pulls the image and upload it

Correct idea.

Container runtime actually **runs containers**.

Examples:

```
containerd
CRI-O
Docker (older)
```

Flow:

```
Pod scheduled
↓
Runtime pulls image
↓
Container started
```

Example:

```
docker pull nginx
```

---

## Kubelet

You said:

> kubelet is main thingk it will provide instruction to api server

Almost correct, small correction.

Kubelet **receives instructions from API Server**, not the other way.

Kubelet is the **node agent**.

Responsibilities:

```
watches API server
ensures containers are running
reports node status
```

Flow:

```
API Server
     ↓
Kubelet
     ↓
Container Runtime
```

Example:

```
API Server: Run nginx pod
Kubelet: Start container using runtime
```

---

## Kube-proxy

You said:

> kube proxy is network assigned to each node

Not exactly. Slight correction.

Kube-proxy handles **network routing for services**.

Responsibilities:

```
service networking
load balancing
pod communication
```

Example:

Service:

```
order-service
```

Pods:

```
pod1
pod2
pod3
```

Kube-proxy routes traffic:

```
Client → Service → Pod1/Pod2/Pod3
```

---

# 4. Pods

Pods are the **smallest deployable unit in Kubernetes**.

```
Pod = container + networking + storage
```

Example:

```
Pod
 └ nginx container
```

Pods are **ephemeral**.

Meaning:

```
Pods can die anytime
Kubernetes recreates them
```

Example:

```
Pod crashes
ReplicaSet creates new Pod
```

Important:

```
Pods are not permanent
```

---

# 5. Namespaces

Namespaces help **organize resources inside a cluster**.

Example:

Cluster:

```
default namespace
dev namespace
prod namespace
kube-system namespace
```

Example:

```
dev
 ├ api-service
 └ database

prod
 ├ api-service
 └ database
```

Same resource names allowed.

```
dev/api-service
prod/api-service
```

Namespaces help with:

```
environment separation
resource quotas
security
```

---

# Full Request Flow (Important)

Example command:

```
kubectl apply deployment.yaml
```

Flow:

```
User
 ↓
API Server
 ↓
etcd (store desired state)
 ↓
Scheduler chooses node
 ↓
Kubelet receives instruction
 ↓
Container runtime pulls image
 ↓
Pod starts running
 ↓
Kube-proxy handles networking
```

---

# Quick Interview Summary

### Control Plane

```
API Server → entry point
etcd → cluster database
Scheduler → assigns nodes
Controller Manager → maintains desired state
```

### Worker Node

```
Kubelet → node agent
Container Runtime → runs containers
Kube-proxy → networking
```

### Pods

```
Smallest deployable unit
Ephemeral
Contain containers
```

### Namespaces

```
Logical isolation in cluster
```

---
These concepts are about **how Kubernetes manages Pods in production**. I’ll connect them logically so you see how they relate.

---

# 1. ReplicaSet

A **ReplicaSet ensures a fixed number of pods are always running**.

Example:

```
Replicas = 3
```

Kubernetes guarantees:

```
Pod1
Pod2
Pod3
```

If a pod crashes:

```
Pod2 dies
```

ReplicaSet creates a new one:

```
Pod4 created
```

So again:

```
3 pods running
```

### Example YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```

But in real production **we rarely create ReplicaSets directly**.

Because **Deployments manage ReplicaSets**.

---

# 2. Deployment

A **Deployment manages ReplicaSets and provides advanced features**.

Example:

```
Deployment
   ↓
ReplicaSet
   ↓
Pods
```

Deployment responsibilities:

```
create ReplicaSets
manage rolling updates
rollback versions
scale pods
```

Example structure:

```
Deployment
   └ ReplicaSet
        ├ Pod1
        ├ Pod2
        └ Pod3
```

If you update the image:

```
nginx:1.20 → nginx:1.21
```

Deployment creates **a new ReplicaSet**.

---

# Deployment YAML Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.20
```

---

# Deployment vs ReplicaSet

| Feature            | ReplicaSet | Deployment  |
| ------------------ | ---------- | ----------- |
| Maintain pods      | ✔          | ✔           |
| Rolling updates    | ❌          | ✔           |
| Rollback           | ❌          | ✔           |
| Version management | ❌          | ✔           |
| Production usage   | Rare       | Very common |

Rule:

```
ReplicaSet maintains pods
Deployment manages ReplicaSets
```

---

# 3. Scaling Pods

Scaling means **increasing or decreasing number of pods**.

Example:

```
replicas = 3
```

Scale to:

```
replicas = 5
```

New pods created:

```
Pod1
Pod2
Pod3
Pod4
Pod5
```

Command:

```
kubectl scale deployment nginx-deployment --replicas=5
```

Scaling down:

```
kubectl scale deployment nginx-deployment --replicas=2
```

Kubernetes deletes extra pods.

---

# 4. Rolling Updates

Rolling update means **updating pods gradually without downtime**.

Example:

Current pods:

```
Pod1 nginx:1.20
Pod2 nginx:1.20
Pod3 nginx:1.20
```

New version:

```
nginx:1.21
```

Rolling update process:

```
Step1:
Pod1 (1.20)
Pod2 (1.20)
Pod3 (1.20)

Step2:
Pod4 (1.21) created
Pod1 removed

Step3:
Pod5 (1.21) created
Pod2 removed

Step4:
Pod6 (1.21) created
Pod3 removed
```

Final state:

```
Pod4 (1.21)
Pod5 (1.21)
Pod6 (1.21)
```

Users experience **zero downtime**.

Command:

```
kubectl set image deployment/nginx nginx=nginx:1.21
```

---

# 5. Rollbacks

If the new deployment breaks production, Kubernetes can **rollback**.

Example problem:

```
nginx:1.21 has bug
```

Rollback command:

```
kubectl rollout undo deployment nginx-deployment
```

Kubernetes restores previous ReplicaSet.

Example:

```
ReplicaSet v1 → nginx:1.20
ReplicaSet v2 → nginx:1.21
```

Rollback removes v2 and returns to v1.

You can see history:

```
kubectl rollout history deployment nginx-deployment
```

---

# 6. StatefulSets (Important for Databases)

Deployments are designed for **stateless apps**.

Examples:

```
Spring Boot API
Node.js app
Frontend service
```

But databases require:

```
stable identity
persistent storage
ordered startup
```

Example systems:

```
Kafka
MySQL
PostgreSQL
MongoDB
Redis cluster
```

These use **StatefulSets**.

---

# StatefulSet Characteristics

Each pod gets:

```
Stable hostname
Persistent storage
Ordered deployment
```

Example:

```
kafka-0
kafka-1
kafka-2
```

Even if pods restart:

```
kafka-0 stays kafka-0
```

Storage also stays attached.

Example volumes:

```
kafka-0 → volume0
kafka-1 → volume1
kafka-2 → volume2
```

---

# StatefulSet Example

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: kafka
spec:
  serviceName: kafka
  replicas: 3
  selector:
    matchLabels:
      app: kafka
```

Pods created sequentially:

```
kafka-0
kafka-1
kafka-2
```

Deletion also happens **in reverse order**.

---

# Deployment vs StatefulSet

| Feature            | Deployment | StatefulSet |
| ------------------ | ---------- | ----------- |
| Stateless apps     | ✔          | ❌           |
| Databases          | ❌          | ✔           |
| Pod identity       | Random     | Stable      |
| Persistent storage | Optional   | Built-in    |
| Pod ordering       | No         | Yes         |

---

# Real Production Example

Microservices system:

```
Kubernetes Cluster
```

Workloads:

```
Deployment
   ├ order-service
   ├ payment-service
   └ user-service

StatefulSet
   ├ Kafka
   ├ MySQL
   └ Redis cluster
```

---

# Quick Interview Summary

### Deployment

```
Manages ReplicaSets
Supports rolling updates
Supports rollback
Used for stateless apps
```

### ReplicaSet

```
Ensures fixed number of pods
```

### Scaling

```
Increase/decrease pod replicas
```

### Rolling Updates

```
Update pods gradually with zero downtime
```

### Rollbacks

```
Revert to previous version if deployment fails
```

### StatefulSet

```
Used for stateful systems like Kafka, databases
Stable pod identity
Persistent storage
```

---

These concepts explain **how traffic reaches your Pods in Kubernetes**.
Because **Pods are ephemeral and their IPs change**, Kubernetes uses **Services and Ingress** for networking.

Let's go step-by-step.

---

# 1. Why Kubernetes Services Exist

Problem:

Pods are **temporary**.

Example:

```
Pod1 IP → 10.1.2.3
Pod2 IP → 10.1.2.4
```

If a pod crashes:

```
Pod1 dies
New Pod → 10.1.2.9
```

So clients cannot rely on **Pod IPs**.

Solution:

```
Kubernetes Service
```

A Service provides:

```
Stable IP
Load balancing
Service discovery
```

Example:

```
order-service
    ↓
Pod1
Pod2
Pod3
```

Service distributes traffic.

---

# 2. ClusterIP (Internal Service)

**ClusterIP is the default service type.**

It exposes the service **only inside the cluster**.

Example:

```
Frontend Pod
     ↓
order-service (ClusterIP)
     ↓
Order Pods
```

External users **cannot access it**.

Example use case:

```
API service → database service
microservice → microservice communication
```

Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  type: ClusterIP
  selector:
    app: order
  ports:
  - port: 80
    targetPort: 8080
```

Flow:

```
ClusterIP → Pod
```

---

# 3. NodePort (External Access via Node)

NodePort exposes the service **on every node's IP**.

Example:

```
Node IP: 192.168.1.10
NodePort: 30007
```

Access URL:

```
http://192.168.1.10:30007
```

Flow:

```
Client
   ↓
Node IP:30007
   ↓
Service
   ↓
Pods
```

Kubernetes reserves port range:

```
30000 – 32767
```

Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  type: NodePort
  selector:
    app: order
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30007
```

Problem with NodePort:

```
Ugly URLs
Security concerns
Hard to manage
```

So production usually uses **LoadBalancer or Ingress**.

---

# 4. LoadBalancer

LoadBalancer is used in **cloud environments**.

Example:

```
AWS
Azure
GCP
```

Kubernetes automatically creates a **cloud load balancer**.

Example:

```
Client
   ↓
Cloud Load Balancer
   ↓
Service
   ↓
Pods
```

Example public URL:

```
http://a34f12.elb.amazonaws.com
```

Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  type: LoadBalancer
  selector:
    app: order
  ports:
  - port: 80
    targetPort: 8080
```

Kubernetes automatically provisions:

```
External Load Balancer
```

Used for:

```
Public APIs
Web applications
```

---

# 5. Ingress (HTTP Routing Layer)

Ingress is **smarter routing for HTTP/HTTPS traffic**.

Instead of creating many load balancers, we use **one ingress controller**.

Example problem:

You have 3 services:

```
user-service
order-service
payment-service
```

Without ingress:

```
3 LoadBalancers
```

Expensive.

---

### With Ingress

One entry point:

```
Internet
   ↓
Ingress Controller
   ↓
Routing Rules
```

Example routing:

```
example.com/users → user-service
example.com/orders → order-service
example.com/payments → payment-service
```

---

# Ingress Architecture

```
Client
   ↓
Ingress Controller (Nginx / Traefik)
   ↓
Service
   ↓
Pods
```

Ingress handles:

```
HTTP routing
HTTPS TLS
Domain-based routing
Path-based routing
```

---

# Example Ingress YAML

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: order-service
            port:
              number: 80
```

Flow:

```
myapp.com/orders
      ↓
order-service
      ↓
Pods
```

---

# Ingress Controller

Ingress needs a **controller**.

Popular ones:

```
NGINX Ingress Controller
Traefik
HAProxy
AWS ALB Ingress
```

Without controller:

```
Ingress rules do nothing
```

---

# Full Traffic Flow Example

User opens:

```
https://myapp.com/orders
```

Flow:

```
Internet
   ↓
Load Balancer
   ↓
Ingress Controller
   ↓
order-service
   ↓
Pods
```

---

# Quick Comparison

| Feature  | ClusterIP          | NodePort    | LoadBalancer   | Ingress      |
| -------- | ------------------ | ----------- | -------------- | ------------ |
| Access   | Internal           | External    | External       | External     |
| Use case | Service-to-service | Testing     | Public service | HTTP routing |
| URL      | Cluster IP         | NodeIP:Port | Public IP      | Domain-based |

---

# Real Production Example

Example Kubernetes system:

```
Kubernetes Cluster
```

Services:

```
ClusterIP
   ├ user-service
   ├ order-service
   └ payment-service

Ingress
   ↓
api.myapp.com
```

Routing:

```
api.myapp.com/users → user-service
api.myapp.com/orders → order-service
api.myapp.com/payments → payment-service
```

---

# Interview One-Line Summary

**ClusterIP**

```
Internal service communication
```

**NodePort**

```
Exposes service via Node IP and port
```

**LoadBalancer**

```
Creates external cloud load balancer
```

**Ingress**

```
HTTP/HTTPS routing to multiple services using domains or paths
```

---
These concepts are about **externalizing configuration in Kubernetes**, which is very important for **Spring Boot applications** (like `application.yml`).

Instead of hardcoding configs inside the Docker image, Kubernetes provides:

```
ConfigMaps
Secrets
Environment Variables
```

---

# 1. Why ConfigMaps and Secrets Exist

Problem:

If configuration is inside the image:

```
application.yml inside jar
```

Example:

```yaml
db.host=localhost
db.port=3306
```

If environment changes:

```
dev → test → prod
```

You must **rebuild the image every time** ❌.

Solution:

```
External configuration
```

Kubernetes provides:

```
ConfigMap → non-sensitive configs
Secrets → sensitive configs
```

---

# 2. ConfigMaps

ConfigMaps store **non-sensitive configuration data**.

Example configs:

```
application.yml
feature flags
service URLs
log levels
```

Example:

```
DB_HOST=mysql-service
LOG_LEVEL=INFO
FEATURE_FLAG=true
```

---

## Example ConfigMap YAML

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: mysql-service
  DB_PORT: "3306"
  LOG_LEVEL: INFO
```

Kubernetes stores these key-value pairs.

---

# 3. Using ConfigMap in a Pod (Environment Variables)

You can inject config as **environment variables**.

Example Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: order-service
        image: order-service:1.0
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_HOST
```

Inside container:

```
DB_HOST=mysql-service
```

Spring Boot can read it.

Example:

```yaml
spring.datasource.url=${DB_HOST}
```

---

# 4. Mounting ConfigMap as Files

Another common method is **mounting ConfigMap as a file**.

Example:

```
ConfigMap → application.yml
```

ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: spring-config
data:
  application.yml: |
    server:
      port: 8080
    logging:
      level:
        root: INFO
```

Mount it:

```yaml
volumeMounts:
- name: config-volume
  mountPath: /config

volumes:
- name: config-volume
  configMap:
    name: spring-config
```

Inside container:

```
/config/application.yml
```

Spring Boot loads it.

---

# 5. Secrets

Secrets store **sensitive data**.

Examples:

```
Database passwords
API keys
JWT secrets
TLS certificates
```

Example secret data:

```
DB_PASSWORD
API_KEY
```

---

## Example Secret YAML

Secrets must be **Base64 encoded**.

Example:

```
password = mypassword
```

Base64:

```
bXlwYXNzd29yZA==
```

Secret YAML:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_PASSWORD: bXlwYXNzd29yZA==
```

---

# 6. Injecting Secrets as Environment Variables

Deployment example:

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: DB_PASSWORD
```

Inside container:

```
DB_PASSWORD=mypassword
```

Spring Boot can read:

```yaml
spring.datasource.password=${DB_PASSWORD}
```

---

# 7. Secrets as Files

Secrets can also be mounted as files.

Example:

```
/etc/secrets/db-password
```

Deployment:

```yaml
volumeMounts:
- name: secret-volume
  mountPath: /etc/secrets

volumes:
- name: secret-volume
  secret:
    secretName: db-secret
```

---

# 8. Real Production Example (Spring Boot)

Example microservice:

```
order-service
```

Deployment uses:

```
ConfigMap
Secrets
```

Example config:

```
ConfigMap
   DB_HOST=mysql-service
   LOG_LEVEL=INFO

Secret
   DB_PASSWORD=******
```

Spring Boot reads:

```
spring.datasource.url=jdbc:mysql://${DB_HOST}:3306/orders
spring.datasource.password=${DB_PASSWORD}
```

No rebuild required.

---

# 9. Difference Between ConfigMap and Secret

| Feature   | ConfigMap     | Secret          |
| --------- | ------------- | --------------- |
| Data type | Non-sensitive | Sensitive       |
| Encoding  | Plain text    | Base64          |
| Examples  | URLs, configs | passwords, keys |
| Security  | Normal        | Protected       |

---

# 10. Best Practice (Production)

Typical microservice config:

```
ConfigMap
   application.yml
   service URLs
   feature flags

Secrets
   DB passwords
   API keys
   TLS certificates
```

Deployment injects both.

---

# Quick Interview Summary

**ConfigMap**

```
Stores non-sensitive configuration
Example: application.yml, service URLs
```

**Secret**

```
Stores sensitive data
Example: DB passwords, API keys
```

**Injection Methods**

```
Environment variables
Volume mounts (files)
```

Example:

```
env:
valueFrom:
configMapKeyRef
secretKeyRef
```

---
These concepts are about **running applications reliably in Kubernetes production environments**. They ensure **pods stay healthy, resources are controlled, and apps scale automatically**.

I'll explain them in a **Spring Boot microservice context** since that's closest to real-world usage.

---

# 1. Liveness Probe

A **liveness probe checks if the application is alive**.

If the liveness probe fails:

```
Kubernetes restarts the container
```

Example situation:

```
Spring Boot service running
Thread deadlock happens
App stops responding
```

Kubernetes detects this and **restarts the pod**.

---

## Example Liveness Probe

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
```

Flow:

```
Kubernetes
   ↓
Call /actuator/health
   ↓
200 OK → healthy
Failure → restart container
```

Typical use:

```
Detect deadlocks
Detect stuck applications
Restart unhealthy containers
```

---

# 2. Readiness Probe

A **readiness probe checks if the pod is ready to serve traffic**.

If readiness probe fails:

```
Pod stays running
BUT removed from Service load balancing
```

Example situation:

```
Spring Boot app starting
Database connection not ready
```

You **don't want traffic yet**.

So readiness probe fails.

Flow:

```
Pod running
↓
Readiness fails
↓
Service does NOT send traffic
```

Once ready:

```
Readiness passes
↓
Traffic starts flowing
```

---

## Example Readiness Probe

```yaml
readinessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

Example Spring Boot readiness check:

```
DB connection ready
Kafka connection ready
Redis ready
```

---

# Liveness vs Readiness

| Feature        | Liveness              | Readiness                  |
| -------------- | --------------------- | -------------------------- |
| Purpose        | Check if app is alive | Check if app is ready      |
| Failure result | Container restart     | Remove from load balancing |
| Example issue  | Deadlock              | DB not ready               |

---

# 3. Startup Probe

Startup probes solve a **slow startup problem**.

Example:

```
Spring Boot app
Startup time = 90 seconds
```

Without startup probe:

```
Liveness probe runs too early
App not ready yet
Container restarted repeatedly
```

This causes **CrashLoopBackOff**.

Startup probe tells Kubernetes:

```
Wait until application starts
```

---

## Example Startup Probe

```yaml
startupProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

Meaning:

```
Try for 300 seconds (5 minutes)
```

After startup succeeds:

```
Liveness + Readiness probes begin
```

---

# 4. CPU & Memory Requests

Requests define **minimum resources guaranteed for a pod**.

Example:

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
```

Meaning:

```
CPU = 0.2 core guaranteed
Memory = 256 MB guaranteed
```

Scheduler uses this to decide:

```
Which node has enough resources
```

Example:

```
Node has 4GB memory
Pod requests 512MB
Scheduler can place it
```

---

# 5. CPU & Memory Limits

Limits define **maximum resources a pod can use**.

Example:

```yaml
resources:
  limits:
    cpu: "1"
    memory: "512Mi"
```

Meaning:

```
Max CPU = 1 core
Max Memory = 512MB
```

Behavior:

```
CPU limit exceeded → throttled
Memory limit exceeded → container killed
```

---

# 6. OOMKilled (Out Of Memory)

This happens when:

```
Container exceeds memory limit
```

Example:

```
Memory limit = 512MB
Application uses = 600MB
```

Result:

```
Linux OOM Killer kills container
```

Pod status becomes:

```
OOMKilled
```

Example command:

```
kubectl describe pod
```

Output:

```
Last State: Terminated
Reason: OOMKilled
```

Common causes:

```
Memory leaks
Large queries
Huge file processing
Improper limits
```

Fix:

```
Increase memory limit
Optimize memory usage
```

---

# 7. Horizontal Pod Autoscaler (HPA)

HPA automatically **scales pods based on load**.

Example:

```
order-service
replicas = 3
```

If traffic increases:

```
CPU usage = 80%
```

HPA scales:

```
replicas = 6
```

Traffic decreases:

```
CPU usage = 20%
```

HPA scales down:

```
replicas = 2
```

---

## HPA Example YAML

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: order-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: order-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

Meaning:

```
Maintain CPU usage around 70%
Scale pods between 2 and 10
```

---

# Real Production Flow

Example microservice:

```
order-service
```

Deployment config:

```
Readiness probe → check DB + Kafka
Liveness probe → detect deadlock
Startup probe → allow slow boot
Requests → 256MB memory
Limits → 512MB memory
HPA → scale 2–10 pods based on CPU
```

---

# Quick Interview Summary

**Liveness Probe**

```
Checks if container is alive
Failure → restart container
```

**Readiness Probe**

```
Checks if pod can receive traffic
Failure → removed from service
```

**Startup Probe**

```
Used for slow-start applications
Prevents premature restarts
```

**Requests**

```
Minimum resources guaranteed
Used by scheduler
```

**Limits**

```
Maximum resources allowed
Exceed memory → OOMKilled
```

**HPA**

```
Automatically scales pods based on CPU or metrics
```

---
