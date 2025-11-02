# 🧠 Kubernetes Homelab – Learning, Observability & Experiments

Welcome to my *Kubernetes Homelab*, a private *5-node cluster* designed for *learning*, *observability*, and **hands-on experimentation** with modern cloud-native technologies.

This repository centralizes configuration, manifests, and automation scripts for different **Kubernetes setup methods** and *cluster management workflows*.

---

## 🚀 Objectives

This homelab is built to explore and master:

* Kubernetes cluster creation and administration
* GitOps workflows (Argo CD, FluxCD)
* Observability stack (Prometheus, Grafana, Loki, Tempo)
* Service mesh experimentation (Istio, Linkerd, Cilium)
* Application deployment, scaling & resilience testing
* CI/CD pipelines and automation
* Cluster security, policies & RBAC
* Platform Engineering through Internal Developer Platforms and Portals development (Backstage, Kratix, [Humanitec])

---

## 🗂️ Repository Structure

| Folder                                        | Description                                                                                                      | Link                                    |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| **[`kind/`](./kind/README.md)**               | Local Kubernetes clusters using [Kind](https://kind.sigs.k8s.io/) (Kubernetes IN Docker)                         | [📘 Read more](./kind/README.md)        |
| **[`kubeadm/`](./kubeadm/README.md)**         | Multi-node cluster setup using [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/) | [📘 Read more](./kubeadm/README.md)     |
| **[`k0s/`](./k0s/README.md)**                 | Lightweight Kubernetes setup using [k0s](https://k0sproject.io/)                                                 | [📘 Read more](./k0s/README.md)         |
| **[`manifests/`](./manifests/README.md)**     | Common reusable Kubernetes manifests (monitoring, networking, storage, security)                                 | [📘 Read more](./manifests/README.md)   |
| **[`helm-values/`](./helm-values/README.md)** | Centralized Helm values for custom deployments (Argo CD, Grafana, etc.)                                          | [📘 Read more](./helm-values/README.md) |

---

## ⚙️ Tools & Environments

| Category                 | Tools / Components                      |
| ------------------------ | --------------------------------------- |
| **Cluster Provisioning** | Kind · Kubeadm · k0s                    |
| **Package Management**   | Helm · Kustomize                        |
| **GitOps**               | Argo CD · FluxCD                        |
| **Networking**           | Cilium · NGINX Ingress Controller       |
| **Observability**        | Prometheus · Grafana · Loki · Tempo · OpenTelemetry  |
| **Automation & CI/CD**   | GitHub Actions · Argo Workflows · Jenkins |
| **Security**             | Kyverno · OPA Gatekeeper · Cert-Manager |
| **Platform Engineering** | Backstage, Kratix, Humanitec  |

---

## 🧩 Cluster Layout (5 Nodes)

| Role          | Hostname   | Purpose                                   |
| ------------- | ---------- | ----------------------------------------- |
| Control Plane | `master-1` | API server, scheduler, controller-manager |
| Worker        | `worker-1` | General-purpose workloads                 |
| Worker        | `worker-2` | Application testing, monitoring stack     |
| Worker        | `worker-3` | GitOps / Argo CD / observability          |
| Worker        | `worker-4` | Service mesh and network experiments      |

> 🧠 Nodes may run as containers (Kind) or as VMs (Kubeadm, k0s).

---

## 🧭 Quick Start (Kind Example)

### 1. Create a local cluster

```bash
cd kind/
kind create cluster --config bootstrap.yaml
```

### 2. Install Ingress and Argo CD

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace -f values-ingress-nginx.yaml
helm install argo-cd argo/argo-cd -n argocd --create-namespace -f values-argocd-ingress.yaml
```

> 📘 Full guide: [Setup Kind Cluster →](./kind/README.md)

---

## 📊 Observability Stack (Planned)

This homelab aims to include a full observability pipeline:

* **Metrics:** Prometheus
* **Visualization:** Grafana
* **Logs:** Loki
* **Traces:** Tempo
* **Alerting:** Alertmanager
* **Dashboards:** kube-prometheus-stack

---

## 🧪 Learning Focus & Experiments

* GitOps workflows with Argo CD
* Multi-cluster synchronization
* Network policies & service mesh
* Monitoring and alerting automation
* Secure ingress with cert-manager
* Local storage and dynamic provisioning
* Developing custom controllers/operators
* IDP development 

---

## 🧹 Cleanup

To delete a local cluster (Kind):

```bash
kind delete cluster
```

For Kubeadm or k0s setups, refer to:

* [Kubeadm teardown guide →](./kubeadm/README.md)
* [k0s teardown guide →](./k0s/README.md)

---

## 🔮 Future Enhancements

* [ ] Integrate full GitOps pipelines (ArgoCD + Helm + GitHub Actions + Jenkins)
* [ ] Add Terraform automation for VM provisioning
* [ ] Include persistent storage (Longhorn / Ceph / )
* [ ] Explore cluster federation and cross-cluster observability
* [ ] Add Kustomize overlays for environment-specific customization

---

## 🧑‍💻 Author

**Andreas MOUZONG**
🌐 [CodeGrill](https://www.youtube.com/@CodeGrill) • 💼 DevOps & Cloud Platform Engineer • 🚀 Passionate about automation, observability & Kubernetes

---

## 🧭 License

This project is open for **educational and experimental use**.
Feel free to fork, adapt, and improve it — with proper attribution.

---

> 💬 *"Build. Observe. Automate. That’s how you master Kubernetes Engineering."*
