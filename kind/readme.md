# 🚀 Bootstrapping a Local Kubernetes Cluster With Kind and Argo CD

This guide explains how to **create a local Kubernetes cluster using [Kind](https://kind.sigs.k8s.io/)**, install essential components like **NGINX Ingress Controller**, and **deploy Argo CD** for GitOps-based application delivery.

---

## 📘 Overview

[Kind](https://kind.sigs.k8s.io/) (Kubernetes IN Docker) allows you to run a Kubernetes cluster locally using Docker containers as nodes.
It’s perfect for **development, testing, and CI/CD pipelines**, without the overhead of a full cloud cluster.

In this setup, you’ll:

1. Create a local Kubernetes cluster using Kind.
2. Install **Helm**, **kubectl**, and **Argo CD**.
3. Deploy the **Ingress NGINX controller** to handle HTTP routing.
4. Access the Argo CD UI locally to manage GitOps deployments.

---

## 🧰 Prerequisites

Ensure you have the following tools installed on your system:

* [Homebrew](https://brew.sh/) (for macOS users)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [Kind](https://kind.sigs.k8s.io/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Helm](https://helm.sh/)
* [Argo CD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

You can install the core dependencies with:

```bash
brew install kind helm kubectl argocd
```

---

## ⚙️ Step 1: Create the Kubernetes Cluster

Create your local cluster using the configuration file `bootstrap.yaml` (this file typically defines the number of nodes, networking, and port mappings):

```bash
kind create cluster --config bootstrap.yaml
```

> 📝 **Tip:** You can verify the cluster is running with:
>
> ```bash
> kubectl get nodes
> ```

---

## 📦 Step 2: Add Helm Repositories

Add the official Helm repositories for NGINX Ingress and Argo CD:

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

---

## 🌐 Step 3: Deploy the NGINX Ingress Controller

Install the ingress controller that will handle external access to services in your cluster.

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
  -n ingress-nginx \
  --create-namespace \
  -f values-ingress-nginx.yaml
```

> The `values-ingress-nginx.yaml` file allows you to customize your ingress configuration — for example, enabling hostPort bindings for local routing or defining custom annotations.

---

## 🚀 Step 4: Deploy Argo CD

Now, install **Argo CD** — a declarative, GitOps continuous delivery tool for Kubernetes.

```bash
helm upgrade -i argo-cd argo/argo-cd \
  -n argocd \
  --create-namespace \
  -f values-argocd-ingress.yaml
```

> The `values-argocd-ingress.yaml` file should define:
>
> * Ingress host and path rules
> * Service type (ClusterIP, NodePort, etc.)
> * Optional authentication and TLS settings

Once deployed, check that all pods are running:

```bash
kubectl get pods -n argocd
```

---

## 🔐 Step 5: Access the Argo CD Dashboard

Retrieve the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 --decode && echo
```

Then, forward the Argo CD API server port to your local machine:

```bash
kubectl port-forward svc/argo-cd-argocd-server -n argocd 8080:443
```

Access the dashboard at:
👉 [https://localhost:8080](https://localhost:8080)

Login with:

* **Username:** `admin`
* **Password:** *(the one retrieved above)*

---

## 🧹 Step 6: Delete the Cluster (Optional)

When you’re done experimenting, you can delete the cluster to free resources:

```bash
kind delete cluster
```

---

## 🧩 Folder Structure Example

```
.
├── bootstrap.yaml              # Kind cluster configuration
├── values-ingress-nginx.yaml   # Ingress controller Helm values
├── values-argocd-ingress.yaml  # Argo CD Helm values
└── README.md                   # Documentation (this file)
```

---

## ✅ Summary

By following this setup, you’ve created:

* A **local Kubernetes cluster** running with Kind
* An **Ingress controller** for routing traffic
* A **fully functional Argo CD instance** for managing GitOps workflows

This environment is ideal for **testing CI/CD pipelines**, **developing Helm charts**, or **simulating cloud-like Kubernetes environments locally**.

---

## 🧠 Next Steps

* Configure a Git repository in Argo CD and sync your first app.
* Add TLS support using local certificates (e.g., via [mkcert](https://github.com/FiloSottile/mkcert)).
* Experiment with multi-cluster setups using Kind.
