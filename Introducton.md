❌ Drawbacks of Docker (Standalone)
and how Kubernetes helps

| **Docker Limitation**                             | **How Kubernetes Solves It**                                        |
| ------------------------------------------------- | ------------------------------------------------------------------- |
| 🚫 **No built-in orchestration**                  | ☸️ Kubernetes orchestrates thousands of containers across clusters. |
| 🧍 **Single-host deployment**                     | ☸️ Kubernetes manages multi-host clusters.                          |
| ❌ **Manual scaling**                              | ☸️ Automatically scales up/down pods based on resource usage.       |
| 💥 **No self-healing**                            | ☸️ Detects failed containers and restarts or replaces them.         |
| 🔄 **No rolling updates or rollback**             | ☸️ Supports seamless updates and easy rollbacks for deployments.    |
| 🔌 **Manual load balancing**                      | ☸️ Built-in service discovery and load balancing.                   |
| 📦 **Persistent storage setup is manual & basic** | ☸️ Abstracts persistent volumes; supports dynamic provisioning.     |
| 🧪 **Monitoring and logging not integrated**      | ☸️ Integrates with Prometheus, Grafan                               |

✅ In Short:
|                     | **Docker**             | **Kubernetes**                        |
| ------------------- | ---------------------- | ------------------------------------- |
| Scope               | Manages **containers** | Manages **containers at scale**       |
| Manages             | One machine            | Cluster of machines                   |
| Scaling             | Manual                 | Automatic                             |
| Healing             | Manual restart         | Auto restart, self-healing            |
| Deployment Strategy | Manual                 | Declarative with rollouts & rollbacks |

☸️ Kubernetes Master vs Worker Nodes

| Role                     | **Master Node (Control Plane)**                                                                 | **Worker Node (Data Plane)**                                                       |
| ------------------------ | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| 🧠 **Purpose**           | Manages the cluster                                                                             | Runs your application workloads (containers/pods)                                  |
| ⚙️ **Components**        | API server, scheduler, controller manager, etcd                                                 | kubelet, kube-proxy, container runtime (Docker/containerd)                         |
| 📦 **Runs Pods?**        | No (except system pods)                                                                         | Yes, user application pods                                                         |
| 🗺️ **Responsibilities** | - Scheduling pods<br>- Maintaining cluster state<br>- Managing nodes<br>- Handling API requests | - Running containers<br>- Reporting status<br>- Following instructions from master |
| 📡 **Communication**     | Exposes Kubernetes API (kubectl talks to this)                                                  | Listens to instructions from master (via kubelet)                                  |

🧩 Components Explained:
✅ Master Node Components:
API Server – Entry point for all requests (kubectl communicates with it).

Scheduler – Decides which node a pod should run on.

Controller Manager – Monitors cluster state and makes changes as needed.

etcd – Key-value store for cluster data (like a database for cluster state).

✅ Worker Node Components:
Kubelet – Agent on each worker that talks to the master and runs containers.

Kube-proxy – Manages networking and load balancing for services.

Container Runtime – Runs the actual containers (Docker, containerd, CRI-O, etc.).

🔄 Interaction Diagram (Text-based)
User → kubectl → API Server (Master)
                      ↓
        Scheduler ← Controller Manager ← etcd
                      ↓
          Sends instructions to → Worker Nodes
                      ↓
              Worker runs Pods via Kubelet

## IN kubernetes container is called pod
