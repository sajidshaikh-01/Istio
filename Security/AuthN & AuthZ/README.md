# Istio Security – Authentication & Authorization (Production‑Grade Guide)
---
## 1. Why Istio Security Exists

Traditional Kubernetes security problems:

* No service‑to‑service identity
* No encryption inside the cluster
* Hard to enforce fine‑grained access control
* App teams must implement security themselves

**Istio solves this at the platform layer** using Envoy proxies and control‑plane policies.

---

## 2. Two Pillars of Istio Security

| Pillar         | Question it answers    |
| -------------- | ---------------------- |
| Authentication | Who is the caller?     |
| Authorization  | Is the caller allowed? |

---

## 3. Istio Authentication (AuthN)

### What is Authentication in Istio?

Authentication verifies the **identity of a workload**.
Istio uses **strong workload identity**, not IP‑based trust.

---

## 3.1 Types of Authentication

### A. Peer Authentication (Service‑to‑Service)

* Used for **internal mesh traffic (east‑west)**
* Based on **mTLS (mutual TLS)**
* Verifies **both client and server identities**

### B. Request Authentication (End‑User / JWT)

* Used for **user‑level authentication**
* Validates JWT tokens (OIDC, OAuth2)
* Common with API Gateway, Keycloak, Auth0, Cognito

---

## 4. mTLS (Mutual TLS) – Core of Istio Security

### What mTLS Provides

* Encryption (in‑transit)
* Strong workload identity
* Protection from impersonation
* Zero‑trust networking

---

## 4.1 How mTLS Works in Istio

1. Each workload gets an identity (SPIFFE ID)
2. Istio issues certificates automatically
3. Envoy proxies perform TLS handshake
4. Traffic is encrypted end‑to‑end

**Applications are completely unaware of TLS**.

---

## 4.2 PeerAuthentication Resource

Controls **whether mTLS is enforced**.

mTLS Modes:

* `STRICT` → only mTLS allowed (recommended)
* `PERMISSIVE` → mTLS + plaintext (migration)
* `DISABLE` → no mTLS

Scopes:

* Mesh‑wide
* Namespace‑level
* Workload‑level

---

## 4.3 DestinationRule and mTLS

* PeerAuthentication enables mTLS
* DestinationRule defines **how clients connect**

Common mode:

* `ISTIO_MUTUAL` (automatic certs)

---

## 5. Request Authentication (JWT)

### What is RequestAuthentication?

* Validates JWT tokens
* Does NOT allow or deny traffic
* Only verifies token authenticity

Used for:

* API authentication
* Microservices behind gateways

---

### JWT Flow

1. Client sends request with JWT
2. Envoy validates token signature
3. Claims added to request context
4. AuthorizationPolicy decides access

---

## 6. Istio Authorization (AuthZ)

### What is Authorization in Istio?

Authorization decides **whether a request is allowed** after authentication.

---

## 6.1 AuthorizationPolicy Resource

This is the **only authorization object in Istio**.

### What it Controls

* Which service can call which service
* Which HTTP paths are allowed
* Which methods are allowed
* Which JWT claims are required
* Which namespaces / principals are allowed

---

## 6.2 AuthorizationPolicy Evaluation Order

1. DENY rules (highest priority)
2. ALLOW rules
3. If no rule matches → request denied (zero trust)

---

## 6.3 Common Authorization Attributes

| Attribute           | Example                |
| ------------------- | ---------------------- |
| source.principal    | Service identity       |
| source.namespace    | Namespace based access |
| request.headers     | Header based rules     |
| request.auth.claims | JWT claims             |
| operation.paths     | URL paths              |
| operation.methods   | GET, POST              |

---

## 7. Complete Security Flow (End‑to‑End)

```
Client
  |
  | (JWT)
  v
Ingress Gateway (Envoy)
  |
  | RequestAuthentication (JWT validation)
  | AuthorizationPolicy (user access)
  v
Service A Sidecar Envoy
  |
  | mTLS handshake
  | AuthorizationPolicy (service‑to‑service)
  v
Service B Sidecar Envoy
  |
Application Container
```

---

## 8. Tools Used in Istio Security

| Tool / Component      | Purpose                        |
| --------------------- | ------------------------------ |
| Envoy Proxy           | Enforces all security policies |
| Istiod                | Issues certs, pushes config    |
| PeerAuthentication    | Enables mTLS                   |
| DestinationRule       | Client‑side TLS behavior       |
| RequestAuthentication | JWT validation                 |
| AuthorizationPolicy   | Access control                 |
| SPIFFE                | Workload identity format       |
| SPIRE (optional)      | External identity provider     |

---

## 9. Production Best Practices

* Enable mesh‑wide mTLS (STRICT)
* Use namespace‑level policies
* Start with DENY‑all, then ALLOW explicitly
* Avoid PERMISSIVE in long term
* Separate user auth and service auth
* Observe denied requests via metrics

---

## 10. Common Interview Questions (Quick Answers)

**Q: Does Istio require app changes for mTLS?**
No, security is handled by Envoy proxies.

**Q: Difference between Authentication and Authorization?**
Authentication verifies identity; Authorization decides access.

**Q: Can Istio do zero‑trust security?**
Yes, via mTLS + AuthorizationPolicy.

---

