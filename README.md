# 🌐 Hello World K8s

**A simple project to deploy a basic web application on a Kubernetes cluster.**

---

## 📦 Prerequisites

- Docker installed
- Active Kubernetes cluster (local or cloud)
- `kubectl` configured and connected to your cluster

---

## 🧾 Description

This project deploys a simple web page displaying the message "Hello World", along with the container's _hostname_ to confirm that your Kubernetes cluster is running properly.

---

## 📁 Project Structure

```
hola-mundo-k8s/
├── index.html          # Main webpage
├── Dockerfile          # Docker image based on Nginx
├── deployment.yaml     # Kubernetes deployment configuration
├── service.yaml        # Kubernetes NodePort service
└── README.md           # Project documentation
```

---

## 🐳 Docker Image

### Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
EXPOSE 80
```

### Commands

```bash
# Build the image
docker build -t hello-world:v1 .

# Test locally
docker run -p 8080:80 hello-world:v1
```

---

## ☸️ Kubernetes Deployment

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world
          image: hello-world:v1
          ports:
            - containerPort: 80
```

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  type: NodePort
  selector:
    app: hello-world
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30001
```

---

## 🚀 Deployment Steps

```bash
# Apply configuration files
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# Verify resources
kubectl get pods
kubectl get services
```

### Access the Application

- If using Minikube:
  ```bash
  minikube service hello-world-service
  ```
- Or access directly:
  ```
  http://localhost:30001
  ```

---

## 🔧 Useful Commands

```bash
# View deployment logs
kubectl logs deployment/hello-world

# Scale replicas
kubectl scale deployment hello-world --replicas=3

# Delete resources
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```

---

## ✅ Features

- Lightweight and responsive web page
- Shows container hostname
- Based on Nginx Alpine image
- Horizontally scalable
- Ready for Kubernetes environments
