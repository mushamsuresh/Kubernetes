# 📦 Kubernetes Workload Components Overview

## 1. **Pod**
- The smallest and simplest unit in the Kubernetes object model.
- Represents one or more containers that share storage, network, and a specification for how to run the containers.

## 2. **Deployment**
- Manages ReplicaSets and Pods to ensure a specified number of Pods are running.
- Supports rolling updates, rollbacks, and declarative updates.

## 3. **ReplicaSet**
- Maintains a stable set of replica Pods running at any given time.
- Automatically replaces failed Pods to ensure availability.

## 4. **Service**
- An abstraction that defines a logical set of Pods and a policy by which to access them.
- Types include ClusterIP, NodePort, LoadBalancer, and ExternalName.

## 5. **Ingress**
- Manages external access to services, typically HTTP.
- Provides routing rules to expose multiple services under the same IP or domain.

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

