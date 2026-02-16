# 🏗️ Kubernetes Microservices Architecture (EKS + Jenkins CI/CD)

## 📌 Project Overview
This project demonstrates a production-style microservices deployment on AWS EKS using Jenkins CI/CD and Kubernetes.

The application consists of multiple microservices communicating internally using Kubernetes Services and DNS within the `webapps` namespace.

---

# 🌐 Architecture Summary

## External Access Flow
User → AWS LoadBalancer → Frontend Service → Backend Microservices

Only the frontend is exposed publicly using a LoadBalancer service, while all backend services use ClusterIP for internal communication.

---

# 🧩 Microservices Architecture

## Core Components
- Frontend (Public UI)
- Checkout Service (Orchestrator)
- Product Catalog Service
- Cart Service
- Payment Service
- Shipping Service
- Email Service
- Recommendation Service
- Currency Service
- Redis (Stateful cache)

---

# 🔁 Service Communication (Internal)

All services communicate using Kubernetes internal DNS:
```
<service-name>:<port>
```

Example:
- frontend → checkoutservice:5050
- checkoutservice → paymentservice:50051
- cartservice → redis-cart:6379

Kubernetes automatically resolves:
```
productcatalogservice.webapps.svc.cluster.local
```

---

# 🚀 End-to-End Request Flow

1. User accesses application via AWS LoadBalancer
2. LoadBalancer routes traffic to Frontend Pod
3. Frontend communicates with internal services:
   - Product Catalog
   - Cart Service
   - Recommendation Service
4. Checkout service orchestrates:
   - Payment Service
   - Shipping Service
   - Email Service
   - Currency Service
5. Cart data is stored in Redis (internal ClusterIP)

---

# 🔐 Service Exposure Strategy

| Service | Type | Access |
|--------|------|--------|
| frontend-external | LoadBalancer | Public |
| frontend | NodePort | Internal + Node |
| All backend services | ClusterIP | Internal Only |
| redis-cart | ClusterIP | Internal Only |

---

# ⚙️ Kubernetes Deployment Details

- Namespace: `webapps`
- Platform: AWS EKS
- CI/CD: Jenkins Pipeline
- Container Registry: Docker Hub
- Deployment Method: `kubectl apply -f`

---

# 🛠 CI/CD Flow (Jenkins + EKS)

GitHub → Jenkins Pipeline → kubectl → EKS Cluster (webapps namespace)

Pipeline Responsibilities:
- Pull code from GitHub
- Deploy Kubernetes manifests
- Verify services and pods
- Ensure cluster connectivity

---

# ⚠️ Important DevOps Best Practice

Always define namespace in manifests:
```yaml
metadata:
  name: frontend
  namespace: webapps
```

Otherwise resources will be deployed into the default namespace instead of `webapps`.

---

# 🎯 Interview Explanation (One-Liner)
This architecture follows a Kubernetes microservices pattern where the frontend is exposed via an AWS LoadBalancer, and backend services communicate internally using ClusterIP services and Kubernetes DNS, with Redis acting as the state layer and checkoutservice as the orchestration service.
