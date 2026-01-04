# Creating an Argo CD Application (UI vs CLI)
---

## 1. What is an Argo CD Application?

An **Application** tells Argo CD:

* Where the code/config is (Git repo)
* Which manifests to use (path / Helm / Kustomize)
* Which cluster to deploy to
* Which namespace to deploy into

Without an Application, Argo CD does nothing.

---

## 2. Creating Application Using Argo CD UI

### Step-by-Step (Simple)

1. Open Argo CD UI in browser

2. Click **"NEW APP"**

3. Fill basic details:

   * **Application Name**: my-app
   * **Project**: default
   * **Sync Policy**: Manual or Automatic

4. Source section:

   * **Repository URL**: Git repo URL
   * **Revision**: main / master
   * **Path**: folder where Kubernetes manifests exist

5. Destination section:

   * **Cluster**: [https://kubernetes.default.svc](https://kubernetes.default.svc)
   * **Namespace**: app-namespace

6. Click **CREATE**

7. Click **SYNC** (if manual sync)

---

### When UI Method is Used

* Learning and practice
* Manual approvals
* Demo and troubleshooting
* Small teams

---

## 3. Creating Application Using Argo CD CLI

### Step-by-Step (Simple)

1. Login to Argo CD CLI

```bash
argocd login <ARGOCD_SERVER>
```

2. Create application

```bash
argocd app create my-app \
  --repo https://github.com/org/app-config \
  --path k8s \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace app-namespace
```

3. Sync the application

```bash
argocd app sync my-app
```

4. Check status

```bash
argocd app get my-app
```

---

### When CLI Method is Used

* Automation
* Scripting
* CI pipelines
* Repeatable environments

---

## 4. UI vs CLI Comparison

| Feature          | UI        | CLI          |
| ---------------- | --------- | ------------ |
| Ease of use      | Very easy | Moderate     |
| Automation       | ❌ No      | ✅ Yes        |
| Production usage | Limited   | High         |
| Repeatable       | ❌ Manual  | ✅ Scriptable |

---

## 5. Production Best Practice

* UI → Used for visibility and manual sync
* CLI → Used for automation and scripting
* YAML (Application manifest) → Best for GitOps

> In production, applications are usually created using **YAML or CLI**, not manually from UI.

