# Argo CD SSO Integration – 
---

## 1. What is SSO?

**Single Sign-On (SSO)** allows users to log in **once** and access multiple systems without re-authenticating.

In Argo CD, SSO is used to:

* Avoid local users/passwords
* Integrate with company identity providers
* Enforce centralized access control

---

## 2. Why SSO is Important in Production

Production environments require:

* Centralized identity management
* Strong authentication
* Easy user onboarding/offboarding
* Compliance and auditing

SSO solves all of these.

---

## 3. How Argo CD Supports SSO (High Level)

Argo CD **does NOT implement authentication itself**.

Instead:

* Argo CD delegates authentication to **Dex** or an **external OIDC provider**
* Argo CD handles **authorization using RBAC**

---

## 4. SSO Architecture in Argo CD

```text
User → Identity Provider → Argo CD → Kubernetes
```

Flow:

1. User accesses Argo CD UI/CLI
2. Redirected to Identity Provider (IdP)
3. IdP authenticates user
4. Token is sent back to Argo CD
5. Argo CD maps user/groups to RBAC roles

---

## 5. SSO Integration Options in Argo CD

Argo CD supports SSO mainly via **OIDC**.

---

## 6. Option 1: Dex (Built-in Identity Broker)

### What is Dex?

Dex is an **OIDC identity broker** bundled with Argo CD.

It connects Argo CD to external identity providers.

### Supported Providers via Dex

* LDAP / Active Directory
* GitHub
* GitLab
* Google
* Microsoft Azure AD
* SAML providers (via connectors)

### When Dex is Used

* Small to medium setups
* When IdP does not support native OIDC

### Pros

* Easy to configure
* Supports many providers

### Cons

* Extra component to manage
* Not preferred in very large enterprises

---

## 7. Option 2: Direct OIDC Integration (Without Dex)

### What is Direct OIDC?

Argo CD integrates **directly with an OIDC-compliant IdP**, without Dex.

### Common Identity Providers

* Azure AD (Entra ID)
* Okta
* Google Workspace
* Keycloak
* Auth0

### When Used

* Enterprise environments
* Cloud-native identity systems

### Pros

* Fewer components
* Better scalability
* Enterprise-grade security

### Cons

* Requires IdP with OIDC support

---

## 8. Option 3: LDAP / Active Directory (via Dex)

### How It Works

* LDAP/AD authenticates users
* Dex acts as OIDC bridge
* Argo CD consumes OIDC tokens

### Common Use Case

* Traditional enterprises
* On-prem or hybrid setups

---

## 9. Option 4: SAML-Based SSO (Indirect)

Argo CD does **not support SAML directly**.

SAML is supported:

* Through Dex
* Or through IdP → OIDC translation

Used when organization is fully SAML-based.

---

## 10. CLI Authentication with SSO

Argo CD CLI supports SSO login:

```bash
argocd login <ARGOCD_SERVER> --sso
```

* Browser-based authentication
* Token stored locally

---

## 11. Authorization: SSO + RBAC

SSO handles **who you are**.

RBAC handles **what you can do**.

RBAC is configured using:

* Usernames
* Groups from IdP

Example:

* Dev group → Read-only
* Ops group → Admin access

---

## 12. Production Best Practices

* Prefer Direct OIDC over Dex
* Integrate with company IdP
* Disable local admin login
* Use group-based RBAC
* Audit access regularly

---

## 13. SSO Options Summary Table

| Option      | Used In Production | Notes                 |
| ----------- | ------------------ | --------------------- |
| Dex         | Yes                | Small/medium setups   |
| Direct OIDC | Yes (Most Common)  | Enterprise standard   |
| LDAP/AD     | Yes                | Via Dex               |
| SAML        | Limited            | Via Dex or IdP bridge |

---

