# Istio Interview Questions & Answers – Complete Guide (Beginner → Advanced)
---
## 1. Basics & Architecture

### Q1. What is Istio?

**Answer:**
Istio is a **service mesh** that provides traffic management, security (mTLS), and observability for microservices without requiring application code changes.

---

### Q2. What problem does Istio solve?

**Answer:**
It solves problems like insecure service‑to‑service communication, lack of traffic control, no observability, and complex retry/circuit‑breaker logic in microservices.

---

### Q3. What is a service mesh?

**Answer:**
A service mesh is an infrastructure layer that manages **service‑to‑service communication**, handling security, routing, retries, and observability transparently.

---

### Q4. Explain Istio architecture.

**Answer:**
Istio has:

* **Control plane (istiod)** – pushes config, issues certificates
* **Data plane (Envoy proxies)** – handles actual traffic

---

### Q5. What is Envoy and why is it used?

**Answer:**
Envoy is a high‑performance proxy that handles traffic routing, mTLS, retries, circuit breaking, and telemetry in Istio.

---

## 2. Traffic Management

### Q6. What is Istio Gateway?

**Answer:**
Gateway defines **how traffic enters or exits the mesh** (ports, TLS, protocols). It does not perform routing.

---

### Q7. Difference between Ingress and Istio Gateway?

**Answer:**
Ingress is basic L7 routing, while Istio Gateway provides advanced traffic control, security, and integrates with VirtualService.

---

### Q8. What is a VirtualService?

**Answer:**
VirtualService defines **routing rules** such as path‑based routing, header‑based routing, traffic splitting, retries, and fault injection.

---

### Q9. What is a DestinationRule?

**Answer:**
DestinationRule defines **how traffic behaves after routing**, such as load balancing, mTLS mode, circuit breaking, and subsets.

---

### Q10. What are subsets in Istio?

**Answer:**
Subsets represent **versions of a service** (v1, v2) defined in DestinationRule and used for canary or A/B deployments.

---

### Q11. How does traffic splitting work in Istio?

**Answer:**
VirtualService splits traffic using weights between subsets defined in DestinationRule.

---

### Q12. What is fault injection?

**Answer:**
Fault injection deliberately introduces delays or errors to test application resilience.

---

### Q13. What is circuit breaking in Istio?

**Answer:**
Circuit breaking stops sending traffic to unhealthy services to prevent cascading failures.

---

## 3. Security (Authentication & Authorization)

### Q14. How does Istio provide security?

**Answer:**
Using **mTLS for authentication**, **AuthorizationPolicy for access control**, and **JWT for user authentication**.

---

### Q15. What is mTLS?

**Answer:**
Mutual TLS authenticates both client and server using certificates and encrypts traffic.

---

### Q16. How are certificates managed in Istio?

**Answer:**
istiod automatically issues, rotates, and renews short‑lived certificates for workloads.

---

### Q17. What is PeerAuthentication?

**Answer:**
PeerAuthentication enables or enforces mTLS at mesh, namespace, or workload level.

---

### Q18. What is RequestAuthentication?

**Answer:**
RequestAuthentication validates JWT tokens but does not allow or deny traffic.

---

### Q19. What is AuthorizationPolicy?

**Answer:**
AuthorizationPolicy controls **who can access what**, based on service identity, namespace, paths, or JWT claims.

---

### Q20. Difference between Authentication and Authorization?

**Answer:**
Authentication verifies identity; authorization decides permissions.

---

## 4. Observability

### Q21. How does Istio provide observability?

**Answer:**
Envoy sidecars automatically generate metrics, logs, and traces without app code changes.

---

### Q22. How are metrics collected in Istio?

**Answer:**
Prometheus scrapes metrics exposed by Envoy proxies at `/metrics`.

---

### Q23. What is the role of Grafana?

**Answer:**
Grafana visualizes metrics stored in Prometheus.

---

### Q24. How does distributed tracing work in Istio?

**Answer:**
Envoy propagates trace headers and sends spans to tracing backends like Jaeger or Tempo.

---

### Q25. What is Kiali?

**Answer:**
Kiali provides a **visual service graph**, health checks, and Istio configuration validation.

---

## 5. Production & Scenario Questions

### Q26. How do you secure service‑to‑service communication in production?

**Answer:**
Enable STRICT mTLS with PeerAuthentication and restrict access using AuthorizationPolicy.

---

### Q27. How do you implement canary deployment in Istio?

**Answer:**
Using VirtualService for traffic splitting and DestinationRule subsets.

---

### Q28. How do you debug latency issues in Istio?

**Answer:**
Use Grafana for latency metrics, Jaeger for tracing, and Envoy access logs.

---

### Q29. Does Istio require application code changes?

**Answer:**
No. All features are handled by Envoy proxies.

---

### Q30. When should you not use Istio?

**Answer:**
For very small systems where added complexity and overhead are unnecessary.

---

