# ☸️ Kubernetes Administration Labs - ITI Journey

## 📌 Overview
This repository documents my practical journey through Kubernetes administration. It contains step-by-step solutions, architectural implementations, and documentation for four comprehensive labs completed during my ITI scholarship. The labs cover everything from cluster bootstrapping to advanced networking, storage, and configuration management.

---

## 🚀 What I Learned (Lab by Lab Breakdown)

### Lab 1: Cluster Provisioning & Bootstrapping
Focused on the foundational steps of setting up a Kubernetes cluster from scratch.
* **Kubeadm & CNI:** Successfully provisioned a multi-node cluster using `kubeadm` and configured pod-to-pod communication using the Flannel CNI.
* **Lightweight Kubernetes (K3s):** Explored and implemented a K3s cluster, understanding the differences in bootstrapping and resource footprint compared to traditional Kubernetes.
* **Core Workloads:** Deployed initial Nginx workloads to verify cluster health and networking.

### Lab 2: Cluster Management & Customization
Transitioned into interacting with the cluster, managing contexts, and writing declarative manifests.
* **Kubeconfig Mastery:** Created custom namespaces and managed multiple contexts within the `kubeconfig` file to switch seamlessly between administrative boundaries.
* **Kubectl Plugins:** Extended the functionality of the Kubernetes CLI by creating a custom Bash plugin (`kubectl hostnames`) to interact with cluster nodes.
* **Declarative Deployments:** Authored YAML manifests for multi-replica deployments and injected environment variables directly into pod specifications.

### Lab 3: Networking, DNS & Service Discovery
A deep dive into how traffic flows inside the cluster and how external users access applications.
* **Internal Service Discovery:** Configured NodePort services and utilized CoreDNS to resolve internal Fully Qualified Domain Names (FQDNs) and SRV records across different namespaces.
* **Iptables & Load Balancing:** Inspected OS-level `iptables` NAT rules to understand how `kube-proxy` distributes traffic probability among pod replicas.
* **Ingress Controllers:** Implemented Path-Based Routing (e.g., `/apple` vs `/banana`) using an Ingress Resource to direct traffic to the correct backend services.

### Lab 4: Storage, Configuration & Downward API
Tackled stateful data, dynamic metadata injection, and decoupling configuration from application code.
* **Persistent Storage (PV & PVC):** Configured `hostPath` Persistent Volumes and bound them using Persistent Volume Claims. Enforced `podAffinity` to ensure workload scheduling constraints matched the underlying storage location.
* **Downward API:** Dynamically injected pod metadata (Pod Name and IP) into the application environment at runtime without hardcoding.
* **ConfigMaps:** Created ConfigMaps via literal commands and YAML files, then mounted them simultaneously as Environment Variables and Volume files inside the pods.

---

## 🛠️ Tech Stack & Tools
* **Container Orchestration:** Kubernetes (v1.31), K3s, kubeadm
* **Networking:** Flannel CNI, Traefik/Nginx Ingress, CoreDNS, iptables
* **Storage:** Persistent Volumes (hostPath), PVCs
* **OS & CLI:** Linux Administration, Bash Scripting, `kubectl`

---
*Feel free to explore the individual lab directories for detailed YAML manifests and execution screenshots!*
