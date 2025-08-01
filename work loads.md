# Stateless vs Stateful Applications

In the context of **Kubernetes**, and more broadly in distributed systems and cloud-native architecture, understanding the difference between **stateless** and **stateful** applications is crucial.

---

## 🟢 Stateless Applications

A **stateless** application does **not retain any data or session state** between requests. Each request is treated as an independent transaction.

### ✅ Characteristics:

- No knowledge of past client requests.
- Easy to scale horizontally (add more replicas).
- Failure recovery is simple — any replica can serve any request.
- Best suited for **web servers**, **REST APIs**, **frontend applications**.

### 📦 Examples:
- NGINX or Apache web servers
- Node.js/Express APIs
- Kubernetes Dashboard
- Microservices that use external databases to store data

### 🔁 Analogy:
Think of a **vending machine** – you press buttons, it gives you items. It doesn’t remember what you bought before.

---

## 🔵 Stateful Applications

A **stateful** application **remembers** information between sessions or requests. This state might include:

- User sessions
- Database entries
- Caches
- Queued messages

### ✅ Characteristics:

- Requires **persistent storage** (disk/database).
- Each replica might have its **own identity** (e.g., node ID, hostname).
- Scaling and recovery are more complex.
- Needs **ordered deployment and termination** in some cases.

### 📦 Examples:
- MySQL/PostgreSQL databases
- Redis/Memcached with persistence
- Kafka queues
- Elasticsearch
- Stateful web apps that manage sessions in memory

### 🔁 Analogy:
Think of a **bank teller** – they keep track of your account history and state as you interact with them.

---

## 📊 Comparison Table

| Feature                 | Stateless                         | Stateful                              |
|------------------------|-----------------------------------|---------------------------------------|
| **Stores past data?**  | ❌ No                             | ✅ Yes                                |
| **Easy to scale?**     | ✅ Yes                            | ❌ More complex                       |
| **Pod identity needed?**| ❌ No                            | ✅ Often yes                          |
| **Persistent storage?**| ❌ Not required                   | ✅ Required                           |
| **Failure recovery**   | ✅ Any instance can restart       | ❌ Needs data restoration             |
| **Examples**           | Web servers, APIs                 | Databases, queues, session apps       |

---

## 🧠 Kubernetes Perspective

| Component     | Works well for stateless | Works well for stateful |
|---------------|---------------------------|---------------------------|
| Deployment    | ✅ Yes                    | ❌ Not ideal              |
| StatefulSet   | ❌ Not needed             | ✅ Designed for this      |
| DaemonSet     | ✅ Sometimes              | ✅ Sometimes              |

---

> 🔑 **Key takeaway:**  
Use **stateless apps** for services that don’t require saved data between requests.  
Use **stateful apps** when you need to **retain data**, sessions, or identity across restarts and between users.

# 📄 What is a Manifest in Kubernetes?

A **Kubernetes Manifest** is a YAML or JSON configuration file that **describes the desired state** of a Kubernetes object — such as a Pod, Deployment, Service, ConfigMap, etc.

It is the **blueprint** you give to Kubernetes to tell it *what to create and how it should behave*.

# 📦 Kubernetes Workload Components Overview

## 1. **Pod**
- The smallest and simplest unit in the Kubernetes object model.
- Represents one or more containers that share storage, network, and a specification for how to run the containers.

## 2. **Deployment**
- Manages ReplicaSets and Pods to ensure a specified number of Pods are running.
- Supports rolling updates, rollbacks, and declarative updates.

A **Deployment** is a higher‑level controller in Kubernetes that manages a group of identical Pods through an underlying **ReplicaSet**. Think of it as a “blueprint + traffic manager” for stateless applications that need easy updates, rollbacks, and automatic healing.

---
## Why use a Deployment?

- **Declarative updates** – You tell Kubernetes the *desired* state (image version, replica count, labels, etc.). Kubernetes does the work to reach and maintain that state.
- **Rolling upgrades & rollbacks** – Zero‑downtime image or configuration changes with the ability to revert instantly if something goes wrong.
- **Self‑healing** – If a Pod crashes or the node disappears, the ReplicaSet recreates it automatically.
- **Horizontal scaling** – Change one field (`spec.replicas`) or run `kubectl scale` to add/remove replicas on the fly.
- **Version history** – Each change creates a new ReplicaSet; older ones are retained for rollback.

---

## How it works (simplified flow)

1. **You apply a Deployment manifest.**
2. The Deployment creates (or updates) a **ReplicaSet**.
3. The ReplicaSet maintains the requested number of identical **Pods**.
4. When you update the Deployment (e.g., new container image):
   - A *new* ReplicaSet is spun up with the new spec.
   - Pods are shifted gradually (rolling strategy) from the old ReplicaSet to the new one.
   - If health checks start failing, Kubernetes can pause or roll back.
_In short_: a Deployment lets you declare what you want running, and Kubernetes continually makes sure it happens—while giving you safe, controllable updates and rollbacks along the way.

## 3. **ReplicaSet**
- Maintains a stable set of replica Pods running at any given time.
- Automatically replaces failed Pods to ensure availability.

A **ReplicaSet** is a Kubernetes controller that ensures a **specified number of identical Pods are running at all times**.

It is mostly used **indirectly through Deployments**, but can also be created and managed directly (not common in practice).

---

## 🎯 Purpose of a ReplicaSet

- **Maintain availability**: If a Pod fails or is deleted, the ReplicaSet automatically replaces it.
- **Ensure replica count**: If there are fewer Pods than desired, it creates new ones; if there are too many, it deletes extras.
- **Load balancing**: Multiple Pods created by a ReplicaSet can be load-balanced behind a Service.

📦 Use in Practice
- ReplicaSets are rarely used directly. Instead, they are:
- Automatically managed by Deployments.
- Used behind the scenes to support rolling updates and rollbacks.

🔄 How It Works
- You create a ReplicaSet with replicas: 3.
- It launches 3 Pods with the specified template.
- If a Pod crashes or is deleted, ReplicaSet creates a new one automatically.
- If you scale to 5, it adds 2 Pods; if you scale to 2, it removes 1.

🧠 Analogy
Imagine a photocopier that ensures there are always 3 copies of a document. If someone takes one away, the copier prints a new one. That’s your ReplicaSet.

## 4. **Service**
- An abstraction that defines a logical set of Pods and a policy by which to access them.
- Types include ClusterIP, NodePort, LoadBalancer, and ExternalName.

A **Service** in Kubernetes is a **stable networking abstraction** that exposes a set of Pods as a **network service**. It acts as a **bridge** between the outside world or other internal components and your running Pods.

Since Pods are **ephemeral** (can die and be replaced anytime), a Service ensures that communication can **reliably reach the correct Pods**, regardless of their IP changes.

---

## 🚀 Why Do We Need a Service?

- Pods have **dynamic IPs** — they change after restarts.
- Services provide a **static IP or DNS name**.
- Services offer **load balancing** across multiple Pods.
- Enables **discovery and communication** between components in a microservices architecture.

---

## 🧱 Basic Service Types

| Type            | Purpose                                              | Accessible From          |
|-----------------|------------------------------------------------------|--------------------------|
| `ClusterIP`     | Default. Exposes service on internal cluster IP.     | Only within the cluster  |
| `NodePort`      | Exposes service on a static port on each Node IP.    | External and internal    |
| `LoadBalancer`  | Provisions an external load balancer (cloud only).   | Internet-accessible      |
| `ExternalName`  | Maps service to an external DNS (e.g., `example.com`) | Internal (via DNS)       |

---
🎯 How a Service Works
- You create a Service and give it a selector (e.g., app: my-app).
- Kubernetes finds all matching Pods.
- The Service gets a stable DNS name and IP.
- Any traffic to the Service is load-balanced to the backend Pods.

🔧 Example Use Cases
- _Frontend calls backend:_ A frontend Deployment talks to a backend Service.
- _Expose app to browser:_ Use a NodePort or LoadBalancer Service.
- _Microservice discovery:_ Services act as the internal DNS system in Kubernetes.

## 5. **Ingress**
- Manages external access to services, typically HTTP.
- Provides routing rules to expose multiple services under the same IP or domain.
# Ingress in Kubernetes

## Overview

In Kubernetes, **Ingress** is an API object that manages **external access to services**, typically over **HTTP and HTTPS**. It provides a way to **expose multiple services** under a single IP address or domain name with **routing rules**, **SSL termination**, and **virtual hosting**.

---

## Why Use Ingress?

- Consolidates access to multiple services using a **single external IP**
- Supports **name-based** and **path-based routing**
- Allows **SSL/TLS termination** at the Ingress level
- Offers better **control** and **security** than exposing services directly with NodePort or LoadBalancer

---
_*How Ingress Works*_
A user accesses http://myapp.example.com/service1.

DNS resolves to the Ingress Controller's external IP.

The controller reads the Ingress resource and routes traffic to service1.

Similarly, requests to /service2 are routed to service2.

## Ingress vs Service

| Feature              | Service (NodePort/LoadBalancer) | Ingress                      |
|----------------------|----------------------------------|------------------------------|
| Type of routing      | Basic (1:1)                     | Advanced (paths, domains)    |
| Protocols            | TCP/UDP only                    | HTTP/HTTPS only              |
| SSL termination      | Not supported (needs workaround) | Supported natively           |
| Cost (Cloud)         | Expensive (each service exposed) | Cost-effective (single IP)   |

---

## Ingress Components

- **Ingress Resource**: YAML definition that contains routing rules
- **Ingress Controller**: Actual implementation (e.g., NGINX, Traefik) that watches Ingress resources and routes traffic accordingly

---
## *How Ingress Works*_
- A user accesses http://myapp.example.com/service1.
- DNS resolves to the Ingress Controller's external IP.
- The controller reads the Ingress resource and routes traffic to service1.
- Similarly, requests to /service2 are routed to service2.




## 6. **ConfigMap**
- Used to store configuration data as key-value pairs.
- Allows you to decouple configuration artifacts from application code.

## 7. **Secret**
- Stores sensitive data such as passwords, OAuth tokens, and SSH keys.
- Data is base64 encoded and more secure than ConfigMaps.

## 8. **Namespace**
- Provides a mechanism for isolating groups of resources within a single cluster.
- Useful for separating environments (dev, staging, prod) or teams.

## 9. **PersistentVolume (PV)**
- A piece of storage in the cluster provisioned by an administrator or dynamically provisioned using StorageClasses.
- Abstracts away details of how storage is provided.

## 10. **PersistentVolumeClaim (PVC)**
- A request for storage by a user.
- Binds to a PersistentVolume and allows Pods to use it as durable storage.

## 11. **StatefulSet**
- Manages stateful applications.
- Maintains sticky identities for each Pod and provides stable network and persistent storage.

## 12. **DaemonSet**
- Ensures a copy of a Pod runs on all or selected nodes.
- Commonly used for cluster-wide services like log collection and node monitoring.

## 13. **Job**
- Creates one or more Pods and ensures they complete successfully.
- Used for batch processes and finite tasks.

## 14. **CronJob**
- A scheduled Job, runs on a recurring basis based on a cron expression.
- Used for tasks like backups, reports, or automated maintenance.

