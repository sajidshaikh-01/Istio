# Kiali & Envoy Proxies – What They Are and What They Do
---
## 1. What is Envoy Proxy?

**Envoy** is a **high-performance, open-source L7 (Layer 7) proxy** that acts as the **data plane** in a service mesh like Istio.

In Istio, Envoy is deployed as a **sidecar proxy** alongside every application pod.

---

## 2. Why Envoy is Used in Istio

Microservices communicate over the network constantly. Envoy sits between services and:

* Intercepts all inbound and outbound traffic
* Applies networking rules consistently
* Removes networking logic from application code

> Applications never talk to each other directly — Envoy handles communication.

---

## 3. What Envoy Does (Core Responsibilities)

### 3.1 Traffic Management

Envoy handles:

* Request routing
* Canary deployments
* Blue-green deployments
* A/B testing
* Traffic mirroring

---

### 3.2 Security

Envoy provides:

* Mutual TLS (mTLS) between services
* Service identity verification
* Encryption of service-to-service traffic

Certificates are **automatically issued and rotated** by Istio.

---

### 3.3 Reliability & Resilience

Envoy enforces:

* Retries
* Timeouts
* Circuit breaking
* Rate limiting (with extensions)

This prevents cascading failures in microservices.

---

### 3.4 Observability

Envoy automatically collects:

* Metrics (latency, error rates, request count)
* Distributed traces
* Access logs

No application code changes are required.

---

## 4. Envoy Sidecar Pattern

Each Kubernetes pod contains:

```
Pod
 ├── Application Container
 └── Envoy Proxy (sidecar)
```

Traffic flow:

```
Service A → Envoy → Envoy → Service B
```

This gives Istio full control over traffic behavior.

---

## 5. What is Kiali?

**Kiali** is an **observability and visualization tool** for Istio service mesh.

Kiali provides a **real-time UI** to understand:

* Service topology
* Traffic flow
* Health of services
* Istio configuration issues

> Kiali is mainly used by **DevOps, SRE, and platform teams**.

---

## 6. What Kiali Does (Core Responsibilities)

### 6.1 Service Mesh Visualization

Kiali shows:

* How services are connected
* Which services call each other
* Real-time traffic flow

This helps understand complex microservice architectures.

---

### 6.2 Traffic Monitoring

Kiali displays:

* Request rates
* Error percentages
* Latency metrics

These metrics are pulled from **Prometheus**.

---

### 6.3 Configuration Validation

Kiali validates Istio resources like:

* VirtualService
* DestinationRule
* Gateway

It highlights:

* Misconfigurations
* Conflicting routing rules
* Unused or broken policies

---

### 6.4 Troubleshooting & Debugging

With Kiali, teams can:

* Quickly find failing services
* Detect traffic anomalies
* Verify canary deployments visually

---

## 7. Kiali Architecture (Simple)

Kiali itself:

* Does NOT handle traffic
* Does NOT replace monitoring tools
* Reads data from:

  * Prometheus (metrics)
  * Istio APIs (config)

Kiali is a **read-only visibility layer**.

---

