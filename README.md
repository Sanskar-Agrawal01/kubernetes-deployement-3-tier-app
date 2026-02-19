# 🚀 Full Stack Chat App – Kubernetes Deployment

This project demonstrates deployment of a full-stack real-time chat application on Kubernetes using a production-style architecture.

---

## 🏗️ Architecture Overview

Application Flow:

User → Frontend → Backend → MongoDB

### Deployment Order

1. MongoDB (StatefulSet + PVC + Secret)
2. Backend (Deployment + Service + ConfigMap/Secret)
3. Frontend (Deployment + Service)
4. Optional: Ingress (for external access)

---

# 📦 Kubernetes Components

## 1️⃣ MongoDB (Database Layer)

MongoDB is deployed as a **StatefulSet** because:

- Requires persistent storage
- Needs stable pod identity
- Data must survive restarts

### Resources Used

- StatefulSet  
- PersistentVolumeClaim (PVC)  
- Secret (DB credentials)  
- ClusterIP Service  

MongoDB stores:
- Users  
- Messages  
- Chat metadata  

---

## 2️⃣ Backend (Node.js + Express + Socket.io)

Backend is deployed using a **Deployment**.

It:
- Handles authentication (JWT)
- Manages REST APIs
- Handles real-time communication
- Connects to MongoDB

### Resources Used

- Deployment  
- ClusterIP Service  
- ConfigMap (environment configs)  
- Secret (JWT + DB credentials)  

---

## 3️⃣ Frontend (React + Nginx)

Frontend is deployed using a **Deployment** and exposed via a **Service**.

It:
- Communicates with backend via REST + WebSocket
- Runs behind Nginx inside container

### Resources Used

- Deployment  
- NodePort / LoadBalancer Service  

---

# 🔐 Secrets & ConfigMaps

### Create Secret

```bash
kubectl create secret generic app-secrets \
  --from-literal=MONGO_USER=mongoadmin \
  --from-literal=MONGO_PASS=secret \
  --from-literal=JWT_SECRET=your_jwt_secret
