# Argo CD Interview Questions & Answers + Mock Interview
---

## PART 1: Argo CD Interview Questions & Answers

### 1. What is Argo CD?

**Answer:**
Argo CD is a Kubernetes-native GitOps continuous delivery tool that continuously reconciles the desired state defined in Git with the live state running in a Kubernetes cluster.

---

### 2. What problem does Argo CD solve?

**Answer:**
It eliminates manual deployments, provides drift detection, enables Git as the single source of truth, and ensures secure, auditable, and consistent Kubernetes deployments.

---

### 3. Is Argo CD push-based or pull-based?

**Answer:**
Argo CD is **pull-based**. It runs inside the cluster and pulls the desired state from Git instead of CI pushing changes to the cluster.

---

### 4. Where does Argo CD run?

**Answer:**
Argo CD runs as Kubernetes pods inside the cluster, usually in the `argocd` namespace, and can be scheduled on any worker node by the Kubernetes scheduler.

---

### 5. What is GitOps?

**Answer:**
GitOps is an operational model where Git is the single source of truth and a controller continuously reconciles the desired state from Git with the live system state.

---

### 6. What are the main components of Argo CD?

**Answer:**

* API Server
* Application Controller
* Repository Server
* Redis
* Dex (optional)

---

### 7. What is an Argo CD Application?

**Answer:**
An Application is a CRD that defines the Git source, path, destination cluster, namespace, and sync policy for deploying workloads.

---

### 8. Difference between Application and Project?

**Answer:**
An Application deploys workloads, while a Project defines security boundaries such as allowed repos, clusters, namespaces, and RBAC rules.

---

### 9. What is sync in Argo CD?

**Answer:**
Sync is the process of applying the desired state from Git to the Kubernetes cluster so that both states match.

---

### 10. What are sync types in Argo CD?

**Answer:**

* Manual Sync
* Automatic Sync (with options like self-heal and prune)

---

### 11. What is self-heal?

**Answer:**
Self-heal automatically reverts manual changes made directly in the cluster back to the state defined in Git.

---

### 12. What is prune?

**Answer:**
Prune deletes Kubernetes resources that were removed from Git, ensuring the cluster exactly matches the Git state.

---

### 13. What is reconciliation loop?

**Answer:**
It is a continuous process where Argo CD compares Git state and live cluster state and takes action if they differ.

---

### 14. Does Argo CD deploy applications node by node?

**Answer:**
No. Argo CD interacts with the Kubernetes API server. Kubernetes decides pod placement using its scheduler.

---

### 15. How does Argo CD support multi-cluster deployments?

**Answer:**
Argo CD can register and manage multiple clusters and deploy applications to them from a single control plane.

---

### 16. How are secrets handled in Argo CD?

**Answer:**
Argo CD does not manage secrets directly. It integrates with solutions like External Secrets Operator, Sealed Secrets, or Vault.

---

### 17. How does Argo CD integrate with SSO?

**Answer:**
Argo CD uses OIDC, either via Dex or direct integration with enterprise identity providers like Azure AD or Okta.

---

### 18. Is Argo CD continuous delivery or continuous deployment?

**Answer:**
By default it enables continuous delivery. With auto-sync enabled, it supports continuous deployment.

---

### 19. Can we use Helm with Argo CD?

**Answer:**
Yes. Argo CD natively supports Helm charts and renders them during deployment.

---

### 20. What is App of Apps pattern?

**Answer:**
It is a GitOps pattern where a parent Argo CD application manages multiple child applications, enabling scalable and declarative deployments.

---

## PART 2: Mock Interview (Real Interviewer Style)

### Interviewer:

Explain your CI/CD flow using Argo CD in production.

### Candidate:

We use a GitOps-based CI/CD model where CI builds and tests the application, pushes the image to a registry, and updates the image tag in a Git repository. Argo CD continuously watches that Git repository and reconciles the desired state with the Kubernetes cluster using a pull-based model.

---

### Interviewer:

Why don’t you deploy directly from Jenkins?

### Candidate:

Direct deployment requires cluster credentials in CI, which is a security risk. With Argo CD, the cluster pulls changes from Git, making deployments more secure and auditable.

---

### Interviewer:

What happens if someone manually changes a deployment in the cluster?

### Candidate:

Argo CD’s reconciliation loop detects the drift and, if self-heal is enabled, automatically reverts the change to match Git.

---

### Interviewer:

Where does Argo CD run, and how does it deploy applications?

### Candidate:

Argo CD runs as pods inside the cluster. It does not deploy to specific nodes; instead, it interacts with the Kubernetes API server, and Kubernetes schedules workloads.

---

### Interviewer:

How do you handle production deployments safely?

### Candidate:

We use manual sync for production, protected branches in Git, Argo CD projects for isolation, RBAC, and external secret management. Monitoring and alerts are enabled for sync failures.

---

### Interviewer:

What are common mistakes people make with Argo CD?

### Candidate:

Using kubectl in CI pipelines, storing plaintext secrets in Git, not using projects, and enabling auto-sync in production without proper controls.

---


