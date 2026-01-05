# Deploy Applications Using Helm Charts
---

## 1. What is Helm?

**Helm is a package manager for Kubernetes.**

It helps you:

* Package Kubernetes manifests
* Reuse configurations
* Manage versions and releases

> Helm is to Kubernetes what apt/yum is to Linux.

---

## 2. What is a Helm Chart?

A **Helm chart** is a collection of files that describe:

* Kubernetes resources
* Application configuration
* Deployment logic

Helm charts make applications **installable, upgradeable, and reusable**.

---

## 3. Basic Helm Chart Structure

```text
my-app/
├── Chart.yaml        # Chart metadata
├── values.yaml       # Default configuration values
├── templates/        # Kubernetes manifest templates
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── charts/           # Dependencies (optional)
```

---

## 4. How Helm Deployment Works

1. You run a Helm command
2. Helm reads `values.yaml`
3. Helm renders templates
4. Kubernetes manifests are generated
5. Manifests are applied to the cluster

Helm keeps track of the **release state**.

---

## 5. Deploy Application Using Helm (Step-by-Step)

### Step 1: Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

### Step 2: Create a Helm Chart

```bash
helm create my-app
```

---

### Step 3: Configure Values

Edit `values.yaml`:

```yaml
replicaCount: 2
image:
  repository: nginx
  tag: latest
service:
  type: ClusterIP
  port: 80
```

---

### Step 4: Deploy the Chart

```bash
helm install my-app ./my-app -n my-namespace --create-namespace
```

---

### Step 5: Verify Deployment

```bash
helm list -n my-namespace
kubectl get pods -n my-namespace
```

---

## 6. Upgrade Application Using Helm

To apply changes:

```bash
helm upgrade my-app ./my-app -n my-namespace
```

Helm performs a **rolling update**.

---

## 7. Rollback Application Using Helm

List revisions:

```bash
helm history my-app -n my-namespace
```

Rollback:

```bash
helm rollback my-app 1 -n my-namespace
```

---

## 8. Uninstall Application

```bash
helm uninstall my-app -n my-namespace
```

---

## 9. Using Helm Repositories

Add a repo:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Install from repo:

```bash
helm install nginx bitnami/nginx
```

---

## 10. Helm in Production (Best Practices)

* One chart per application
* Keep environment values separate
* Use `values-dev.yaml`, `values-prod.yaml`
* Version charts properly
* Use Helm with GitOps tools

---

## 11. Helm with GitOps (Argo CD)

In production:

* Helm charts are stored in Git
* Argo CD renders and deploys them
* Git controls the desired state

