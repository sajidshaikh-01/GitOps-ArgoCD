# Argo CD Metrics & Monitoring
---

## 1. Why Monitoring Argo CD Is Important

In production, Argo CD is a **critical control plane component**.

Monitoring helps to:

* Ensure Argo CD is healthy
* Detect sync failures early
* Track application health and drift
* Avoid deployment bottlenecks

If Argo CD is down, **deployments stop**.

---

## 2. How Argo CD Exposes Metrics

Argo CD exposes **Prometheus-compatible metrics** via HTTP endpoints.

Key points:

* Metrics are in **/metrics** endpoint
* Metrics follow Prometheus format
* No custom agent required

---

## 3. Argo CD Components That Expose Metrics

The following Argo CD components expose metrics:

* argocd-server
* argocd-application-controller
* argocd-repo-server
* argocd-dex-server (if enabled)

Each component provides **health and performance metrics**.

---

## 4. Commonly Used Argo CD Metrics (Production)

### Application Metrics

* Application sync status
* Application health status
* Number of managed applications

Examples:

* Applications in Synced vs OutOfSync state
* Healthy vs Degraded applications

---

### Sync & Reconciliation Metrics

* Sync attempts
* Sync failures
* Reconciliation duration

These metrics help identify:

* Deployment issues
* Slow reconciliations

---

### Controller Metrics

* Workqueue depth
* Processing latency
* Error rates

Used to monitor **controller performance**.

---

### API Server Metrics

* API request count
* API response latency
* Authentication failures

Used to monitor **UI and CLI usage**.

---

## 5. Monitoring Stack Used with Argo CD

In production, Argo CD is usually monitored using:

* Prometheus → Metrics collection
* Grafana → Visualization
* Alertmanager → Alerts

This is the **standard Kubernetes monitoring stack**.

---

## 6. How Prometheus Scrapes Argo CD Metrics

High-level flow:

1. Argo CD exposes metrics endpoint
2. Prometheus scrapes metrics periodically
3. Metrics are stored in Prometheus TSDB
4. Grafana visualizes metrics
5. Alertmanager triggers alerts

---

## 7. ServiceMonitor / PodMonitor (Kubernetes)

In Kubernetes:

* ServiceMonitor or PodMonitor is used
* Defined using Prometheus Operator

This allows automatic discovery of Argo CD metrics.

---

## 8. Alerts Commonly Configured in Production

Typical alerts include:

* Argo CD application is OutOfSync for long time
* Application health is Degraded
* Sync failures are increasing
* Argo CD controller is not running
* Repo server unreachable

These alerts prevent silent deployment failures.

---

## 9. Logs vs Metrics vs Events (Clear Difference)

* Metrics → Numbers & trends (Prometheus)
* Logs → Detailed debugging (Loki / ELK)
* Events → Kubernetes state changes

All three are used together in production.

---

## 10. Argo CD Health Checks

Argo CD also provides:

* Kubernetes readiness probes
* Liveness probes

These help Kubernetes restart unhealthy components automatically.

---

## 11. Production Best Practices

* Always monitor Argo CD controller
* Alert on OutOfSync apps
* Monitor sync failures
* Use dashboards per environment
* Do not ignore repo-server metrics

---


