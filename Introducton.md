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

## IN kubernetes container is called pod
