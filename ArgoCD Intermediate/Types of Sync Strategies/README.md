# Argo CD Sync – Meaning and Types of Sync Strategies
---

## 1. What is Sync in Argo CD?

**Sync is the process of making the live Kubernetes state match the desired state defined in Git.**

In simple words:

* Git = What *should* be running
* Cluster = What *is* running
* **Sync = Argo CD fixing the difference**

If Git and cluster are the same → Application is **Synced**
If they are different → Application is **OutOfSync**

---

## 2. Why Sync is Important

Sync ensures:

* Git is the single source of truth
* No manual `kubectl apply`
* Drift is corrected automatically
* Deployments are consistent and repeatable

---

## 3. Types of Sync in Argo CD

Argo CD mainly supports **two sync strategies**:

---

## 4. Manual Sync

### What is Manual Sync?

* Argo CD detects changes
* Human manually clicks **Sync** (UI / CLI)
* Changes are applied only after approval

### How it Works

1. Git change happens
2. Argo CD marks app as **OutOfSync**
3. Operator clicks **Sync**
4. Resources are applied to cluster

### When Used in Production

* Critical production environments
* Approval‑based deployments
* Regulated industries

---

## 5. Automatic Sync

### What is Automatic Sync?

* Argo CD syncs automatically when Git changes
* No human intervention
* Enables **Continuous Deployment**

### How it Works

1. Git change happens
2. Argo CD detects change
3. Argo CD automatically applies changes
4. Cluster reaches desired state

### Common Auto‑Sync Options

#### a) Self‑Heal

* Reverts manual cluster changes
* Fixes drift automatically

#### b) Prune

* Deletes resources removed from Git

---

## 6. Sync Strategy Modes (Important)

### 1. Apply‑Only Sync

* Only creates or updates resources
* Does NOT delete removed resources

### 2. Sync with Prune

* Creates, updates, and deletes resources
* Ensures cluster exactly matches Git

---

## 7. Sync Order (Hooks & Waves)

Argo CD supports **sync ordering** using:

* Sync Waves
* PreSync / Sync / PostSync hooks

Example:

* Database first
* Application next
* Cleanup last

This helps manage complex deployments.

---

## 8. Sync Status Explained

| Status    | Meaning               |
| --------- | --------------------- |
| Synced    | Git and cluster match |
| OutOfSync | Drift detected        |
| Syncing   | Sync in progress      |
| Failed    | Sync error            |

---

## 9. Production Best Practices

* Dev → Auto Sync enabled
* Prod → Manual Sync with approval
* Enable self‑heal cautiously in prod
* Always use prune carefully

---

