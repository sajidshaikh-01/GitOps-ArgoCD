# Argo CD – Overview, How It Works, and Architecture
---

## 1. What is Argo CD?

**Argo CD is a Kubernetes-native GitOps continuous delivery (CD) tool.**

It continuously monitors Git repositories and ensures that the **desired state defined in Git** matches the **actual state running in Kubernetes**.

> If it’s not defined in Git, Argo CD considers it invalid.

---

## 2. Why Argo CD is Used

Argo CD is used to:

* Implement GitOps in Kubernetes
* Eliminate manual `kubectl apply`
* Detect and fix configuration drift
* Provide secure, auditable deployments

---

## 3. How Argo CD Works (High-Level Flow)

1. Developer pushes Kubernetes manifests (YAML / Helm / Kustomize) to Git
2. Git becomes the **single source of truth**
3. Argo CD continuously watches the Git repository
4. Argo CD compares:

   * Desired state (Git)
   * Live state (Kubernetes cluster)
5. If a difference is detected:

   * Argo CD reports drift
   * Syncs the cluster to match Git (manual or automatic)

---

## 4. Argo CD Deployment Model

* Argo CD runs **inside the Kubernetes cluster**
* Installed in a dedicated namespace (commonly `argocd`)
* Uses Kubernetes API to manage applications
* Does NOT deploy applications to specific nodes

Applications run:

* In their own namespaces
* Across any worker nodes selected by Kubernetes scheduler

---

## 5. Argo CD Architecture (Core Components)
<img width="1418" height="786" alt="image" src="https://github.com/user-attachments/assets/b2355b1f-1e21-4bfd-8dca-9d0ea0f7cb3b" />

### 1. API Server

* Exposes UI and CLI
* Handles authentication and RBAC
* Manages application definitions

### 2. Application Controller

* Core reconciliation engine
* Compares Git state vs live cluster state
* Performs sync and self-healing

### 3. Repository Server

* Pulls manifests from Git repositories
* Renders Helm charts and Kustomize overlays
* Sends generated manifests to controller

### 4. Redis

* Caches application state
* Improves performance and scalability

### 5. UI (Web Dashboard)

* Visualizes applications
* Shows sync status, health, and history
* Allows manual sync and rollback
---
<img width="1415" height="789" alt="image" src="https://github.com/user-attachments/assets/a6c68157-05b6-4795-af3f-fe9cb39f4174" />
