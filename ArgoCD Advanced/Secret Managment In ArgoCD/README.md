# Secret Management in Argo CD – 

---

## 1. What is Secret Management?

**Secret management** is the process of securely storing, accessing, and using sensitive data such as:

* Passwords
* API keys
* Tokens
* Certificates

In Kubernetes + GitOps, **secrets must never be stored in plain text in Git**.

---

## 2. Why Secret Management is Critical in GitOps

GitOps principles say:

* Git is the source of truth

But:

* Git is not secure for plaintext secrets

So we need **secure ways to integrate secrets with Argo CD** without exposing them in Git.

---

## 3. How Argo CD Handles Secrets (Important)

Argo CD:

* Does NOT manage secrets directly
* Does NOT encrypt secrets by itself

Instead, Argo CD:

* Deploys applications
* Integrates with **external secret management solutions**

---

## 4. Secret Management Options with Argo CD

There are **multiple supported approaches**.

---

## 5. Option 1: Kubernetes Secrets (Basic)

### How It Works

* Secrets are created directly in Kubernetes
* Applications reference them
* Argo CD deploys apps that use those secrets

### Example

```yaml
envFrom:
- secretRef:
    name: app-secret
```

### Pros

* Simple
* Kubernetes-native

### Cons

* Secrets stored base64 (not encrypted)
* Not Git-friendly
* Manual secret handling

### Usage

* Dev environments
* Small setups

---

## 6. Option 2: Sealed Secrets

### What is Sealed Secrets?

* Encrypts Kubernetes secrets
* Encrypted secrets can be safely stored in Git

### How It Works

1. Secret encrypted using cluster public key
2. Encrypted secret committed to Git
3. Controller decrypts inside cluster

### Pros

* Git-safe
* Kubernetes-native

### Cons

* Cluster-specific
* Harder in multi-cluster setups

### Usage

* Small to medium production setups

---

## 7. Option 3: External Secrets Operator (Most Used)

### What is External Secrets Operator (ESO)?

* Syncs secrets from external secret managers into Kubernetes

### Supported Secret Stores

* AWS Secrets Manager
* AWS Parameter Store
* Azure Key Vault
* Google Secret Manager
* HashiCorp Vault

### How It Works

1. Secret stored in cloud secret manager
2. ESO reads secret
3. ESO creates Kubernetes Secret
4. App consumes secret

### Pros

* Secrets never stored in Git
* Enterprise-grade security
* Works well with multi-cluster

### Cons

* Extra component

### Usage

* **Most common in production**

---

## 8. Option 4: HashiCorp Vault (Enterprise)

### How It Works

* Vault stores secrets securely
* Apps retrieve secrets dynamically

### Integration Models

* Vault Agent Injector
* External Secrets Operator with Vault

### Pros

* Dynamic secrets
* Short-lived credentials
* Strong security

### Cons

* Operational complexity

### Usage

* Large enterprises
* High-security environments

---

## 9. Option 5: Argo CD Vault Plugin (AVP)

### What is AVP?

* Plugin that injects secrets during manifest rendering

### How It Works

* Secrets fetched at deploy time
* Injected into manifests

### Pros

* No secrets in Git

### Cons

* Plugin management
* Operational overhead

### Usage

* Advanced GitOps setups

---

## 10. What NOT to Do (Important)

❌ Do NOT store plaintext secrets in Git
❌ Do NOT hardcode secrets in Helm values
❌ Do NOT expose secrets in CI logs

---

## 11. Production Recommendation (Very Important)

| Environment   | Recommended Option                  |
| ------------- | ----------------------------------- |
| Dev           | Kubernetes Secrets / Sealed Secrets |
| Production    | External Secrets Operator           |
| High Security | Vault + ESO                         |

---

## 12. Argo CD Role in Secret Management

Argo CD:

* Deploys secret references
* Does NOT read secret values
* Stays GitOps-compliant

This keeps Argo CD secure.

---
