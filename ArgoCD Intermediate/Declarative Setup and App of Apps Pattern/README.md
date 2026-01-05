# Declarative Setup and App of Apps Pattern (Argo CD)
---
## 1. What is Declarative Setup?

**Declarative setup means defining *what you want*, not *how to do it*.**

You describe the **desired state** in configuration files, and the system makes reality match that state.

### Simple Example

Instead of saying:

* Create pod
* Scale pod
* Restart pod

You say:

> "I want 3 replicas of this application"

Kubernetes and Argo CD handle *how* to achieve it.

---

## 2. Declarative Setup in GitOps

In GitOps:

* Desired state is written as YAML
* YAML is stored in Git
* Git is the **single source of truth**
* Tools continuously enforce that state

If Git says:

```yaml
replicas: 3
```

Then the cluster **must always have 3 replicas**.

---

## 3. Declarative Setup in Argo CD

Argo CD itself is also managed **declaratively**.

That means:

* Applications are defined as YAML
* Projects are defined as YAML
* Sync policies are defined as YAML

Argo CD reads these definitions and applies them automatically.

---

## 4. Benefits of Declarative Setup

* Version control (Git history)
* Easy rollback
* Reproducible environments
* No manual configuration drift
* Perfect fit for GitOps

---

## 5. What is App of Apps Pattern?

The **App of Apps pattern** is a GitOps design where:

> One **parent Argo CD Application** manages multiple **child Applications**.

Instead of Argo CD managing workloads directly, it manages **applications that manage workloads**.

---

## 6. Why App of Apps is Used

Without App of Apps:

* Each application is created manually
* Hard to manage at scale

With App of Apps:

* Entire environment is bootstrapped from Git
* Easy multi-app and multi-env management
* Fully declarative Argo CD

---

## 7. How App of Apps Works (Step-by-Step)

1. Create a **parent application** in Argo CD
2. Parent application points to a Git folder
3. That folder contains multiple **Application YAMLs**
4. Argo CD creates child applications automatically
5. Each child application deploys its own resources

---

## 8. Simple Folder Structure Example

```text
git-repo/
├── bootstrap/
│   └── root-app.yaml        # Parent app
├── apps/
│   ├── frontend-app.yaml
│   ├── backend-app.yaml
│   └── monitoring-app.yaml
```

* `root-app.yaml` → Parent
* Other YAMLs → Child apps

---

## 9. Where App of Apps is Used in Production

Common production use cases:

* Environment bootstrap (dev / stage / prod)
* Platform components (monitoring, logging, ingress)
* Multi-team application management

---

## 10. Declarative Setup + App of Apps (Together)

When combined:

* Argo CD is installed once
* Parent app is applied once
* Everything else comes from Git

This is **pure GitOps**.

---

## 11. Advantages of App of Apps Pattern

* Fully declarative Argo CD
* Easy onboarding of new apps
* Consistent environments
* Scales very well
* Minimal manual work

---

