# 🚀 TaskFlow — End-to-End Kubernetes CI/CD Pipeline with Jenkins

A complete, real-world **3-Tier Application** (React + Node.js + MongoDB) deployed on **Kubernetes**, fully automated using a **Jenkins CI/CD pipeline**. This project covers Kubernetes concepts from the ground up — Pods, Deployments, Services, Ingress, Autoscaling, Node Scheduling, and more — with a fully working automated deployment flow. 🎯

---

## 📌 Project Overview

TaskFlow is a simple Todo application built to demonstrate **production-style DevOps practices**:

```
Developer pushes code → GitHub → Jenkins triggers pipeline →
Builds Docker images → Pushes to Docker Hub →
Deploys to Kubernetes → App live for users 🎉
```

No manual steps. Just `git push` and watch it deploy itself. ✨

---

## 🏗️ Architecture

| Layer | Technology |
|---|---|
| 🎨 Frontend | React + Nginx |
| ⚙️ Backend | Node.js + Express |
| 🗄️ Database | MongoDB |
| 📦 Containerization | Docker |
| ☸️ Orchestration | Kubernetes |
| 🔧 CI/CD | Jenkins |
| 🌐 Traffic Routing | Nginx Ingress Controller |

---

## ✅ Kubernetes Concepts Covered

- 📁 **Namespace** — isolated environment for project resources
- 🔐 **Secrets** — secure storage for DB credentials
- ⚙️ **ConfigMaps** — clean management of non-sensitive config
- 📦 **Deployments & ReplicaSets** — self-healing, always-on app instances
- 🌐 **Services** (ClusterIP & NodePort) — internal and external communication
- 💾 **PersistentVolumeClaims (PVC)** — durable database storage
- 🚦 **Ingress** — smart single-entry traffic routing
- 📈 **Horizontal Pod Autoscaler (HPA)** — auto-scales pods under load
- 🩺 **Liveness & Readiness Probes** — zero-downtime reliability
- 🚫 **Taints & Tolerations** — restrict which pods can run on which nodes
- 🧭 **Node Affinity** — pods choosing their preferred nodes
- 🔄 **Rolling Updates & Rollbacks** — safe, zero-downtime deployments

---

## 🔧 CI/CD Pipeline (Jenkins)

The Jenkins pipeline automates the **entire deployment lifecycle**:

1. 📥 **Checkout** — pulls the latest code from GitHub
2. 🐳 **Build** — creates Docker images for frontend & backend
3. 📤 **Push** — uploads images to Docker Hub
4. ☸️ **Deploy** — applies updated manifests to the Kubernetes cluster
5. ✅ **Verify** — confirms successful rollout

**Credentials configured in Jenkins:**
- 🔑 Docker Hub (username/password)
- 🔑 GitHub (for repo access)
- 🔑 Kubeconfig (for cluster deployment)

**Trigger:** GitHub Webhook → automatic pipeline run on every `git push` 🔄

---

## 📂 Project Structure

```
taskflow/
├── backend/
│   ├── Dockerfile
│   └── server.js
├── frontend/
│   ├── Dockerfile
│   └── src/
├── k8s/
│   ├── namespace.yaml
│   ├── mongo-secret.yaml
│   ├── configmap.yaml
│   ├── mongo-pvc.yaml
│   ├── mongo-deployment.yaml
│   ├── backend-deployment.yaml
│   ├── frontend-deployment.yaml
│   ├── ingress.yaml
│   └── backend-hpa.yaml
├── Jenkinsfile
└── README.md
```

---

## 🛠️ How to Run This Project

### 1️⃣ Clone the repository
```bash
git clone https://github.com/yourusername/taskflow.git
cd taskflow
```

### 2️⃣ Build and push Docker images (first time manual run)
```bash
docker build -t yourdockerhub/todo-backend:v1 ./backend
docker push yourdockerhub/todo-backend:v1

docker build -t yourdockerhub/todo-frontend:v1 ./frontend
docker push yourdockerhub/todo-frontend:v1
```

### 3️⃣ Deploy to Kubernetes
```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/mongo-secret.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/mongo-pvc.yaml
kubectl apply -f k8s/mongo-deployment.yaml
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/frontend-deployment.yaml
kubectl apply -f k8s/ingress.yaml
kubectl apply -f k8s/backend-hpa.yaml
```

### 4️⃣ Verify everything is running
```bash
kubectl get all -n todo-app
kubectl get ingress -n todo-app
kubectl get hpa -n todo-app
```

### 5️⃣ Set up Jenkins for full automation
- Install Jenkins + required plugins (Docker Pipeline, Git, Kubernetes CLI)
- Add credentials: `dockerhub-creds`, `github-creds`, `kubeconfig-creds`
- Create a Pipeline job pointing to this repo's `Jenkinsfile`
- Add a GitHub webhook for auto-triggering

Now every `git push` automatically builds, pushes, and deploys your app. 🎉

---

## 📈 What Makes This "Production-Level"

- ✅ Self-healing pods (auto-restart on failure)
- ✅ Auto-scaling based on real CPU load
- ✅ Zero-downtime rolling updates with rollback support
- ✅ Secrets kept separate from code
- ✅ Smart node scheduling (Taints/Tolerations + Node Affinity)
- ✅ Fully automated CI/CD — no manual deployment steps

---

## 🎯 Key Learnings from This Project

- How Kubernetes objects work together in a real application, not in isolation
- How to secure and separate configuration from sensitive data
- How to control pod scheduling using Taints, Tolerations, and Node Affinity
- How to build a complete CI/CD pipeline with Jenkins from scratch
- How production systems achieve reliability and self-healing

---

## 🙌 Feedback & Contributions

If you're learning Kubernetes or Jenkins and want to discuss this project, feel free to reach out or open an issue. Always happy to help others build their own version of this! 🌱

---

### 🏷️ Tags
`#Kubernetes` `#Jenkins` `#DevOps` `#CICD` `#Docker` `#CloudNative` `#SRE` `#MERN`
