# Part 4 a: Local Kubernetes Deployment of a ToDo App Using Minikube

# 🐳 Deploying a Fullstack ToDo App on Kubernetes using Minikube

This project demonstrates how to deploy a simple Node.js ToDo application connected to a MongoDB database using Kubernetes on your **local machine** with **Minikube**.

---

## 📦 Overview of the App

- **Frontend & Backend**: Node.js app (`mariamwafik333/todo-app`)
- **Database**: MongoDB
- **Deployment**: Using raw Kubernetes YAML files
- **Cluster**: Local Minikube cluster

---

## 🚀 Step-by-Step Guide

---

### ✅ Step 1: Start Minikube on Your Machine

Minikube lets you run a Kubernetes cluster locally for testing and development.

```
minikube start
```
![Screenshot](images/Screenshot%202025-07-28%20231242.png)

Check that everything is running correctly:

```
minikube status
```

You should see:

```
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

---

### ✅ Step 2: Create a Project Directory for Kubernetes Files

Inside your terminal, run:

```
mkdir todo-k8s && cd todo-k8s
```

This will create a folder named `todo-k8s` where we’ll place all YAML files.

---

### ✅ Step 3: Define the Kubernetes Resources Using YAML Files

---

#### 🟡 mongo-deployment.yaml

This file defines both the Deployment and Service for MongoDB.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo
        ports:
        - containerPort: 27017
---
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

![Screenshot](images/Screenshot%202025-07-27%20225519.png)
![Screenshot](images/Screenshot%202025-07-27%20225556.png)



---

#### 🟡 todo-app-deployment.yaml

This file defines the ToDo App Deployment and exposes it using a NodePort service so it’s accessible from your browser.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: todo-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: todo-app
  template:
    metadata:
      labels:
        app: todo-app
    spec:
      containers:
      - name: todo-app
        image: mariamwafik333/todo-app
        ports:
        - containerPort: 4000
        env:
        - name: MONGO_URL
          value: mongodb://mongo:27017/todos
---
apiVersion: v1
kind: Service
metadata:
  name: todo-service
spec:
  type: NodePort
  selector:
    app: todo-app
  ports:
    - port: 80
      targetPort: 4000
      nodePort: 31741
```
![Screenshot](images/Screenshot%202025-07-27%20225817.png)



---

### ✅ Step 4: Deploy Everything to Kubernetes

Now we apply the YAML files to create the resources:

```
kubectl apply -f mongo-deployment.yaml
kubectl apply -f todo-app-deployment.yaml
```
![Screenshot](images/Screenshot%202025-07-28%20231215.png)

Check that both pods are running:

```
kubectl get pods
```

Expected output:

```
mongo-xxxxxxxxxx-xxxxx       1/1     Running   0   10s
todo-app-xxxxxxxxxx-xxxxx    1/1     Running   0   10s
```

Check services:

```
kubectl get svc
```

Expected:

```
mongo         ClusterIP   ...   27017/TCP
todo-service  NodePort    ...   80:31741/TCP
```
![Screenshot](images/Screenshot%202025-07-28%20231215.png)

---

### ✅ Step 5: Open the Application in Your Browser

1. Get the Minikube IP address:

```
minikube ip
```

2. Combine it with the NodePort (`31741`) to access the app:

```
http://<minikube-ip>:31741
```

Example:

```
http://192.168.49.2:31741
```

✅ You should now see the ToDo App running!

---

### 🛠️ Debugging and Useful Commands

If you run into any issues, try these:

Check pods:

```
kubectl get pods
```

See logs from your app:

```
kubectl logs deploy/todo-app
```

You should see:

```
Yupp! Server is running on port 4000
connected to database
```

Test the service internally:

```
minikube ssh
curl http://localhost:31741
```
![Screenshot](images/Screenshot%202025-07-28%20232538.png)

Or open the service directly with:

```
minikube service todo-service
```
![Screenshot](images/Screenshot%202025-07-28%20232957.png)

![Screenshot](images/Screenshot%202025-07-28%20232939.png)


---

# part 4 b : Deploying Todo App with Kubernetes and ArgoCD (Bonus)

## ✅ Overview

In this bonus part, I deployed the Node.js Todo App (`mariamwafik333/todo-app`) using Kubernetes (Minikube) and managed the deployment with ArgoCD for Continuous Delivery (CD). The application connects to MongoDB Atlas and is exposed via a NodePort service. ArgoCD monitors the GitHub repository and automatically syncs Kubernetes manifests stored under the `todo-k8s` directory.

---

## 📁 Project Structure

```
~/todo-k8s/
├── deployment.yaml
├── service.yaml
├── namespace.yaml
└── argocd-app.yaml
```

---

## 🧩 Kubernetes Manifest Files

### 1. `namespace.yaml`

```
apiVersion: v1
kind: Namespace
metadata:
  name: default
```

---

### 2. `deployment.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: todo-app
  labels:
    app: todo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: todo
  template:
    metadata:
      labels:
        app: todo
    spec:
      containers:
      - name: todo-app
        image: mariamwafik333/todo-app:latest
        ports:
        - containerPort: 3000
        env:
        - name: MONGO_URI
          value: "<your-mongodb-atlas-uri>"
        - name: DB_NAME
          value: "<your-db-name>"
```

---

### 3. `service.yaml`

```
apiVersion: v1
kind: Service
metadata:
  name: todo-service
spec:
  selector:
    app: todo
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
      nodePort: 31741
```

---

### 4. `argocd-app.yaml`

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: todo-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/MariamWafik349/todo-list-nodejs.git'
    targetRevision: HEAD
    path: todo-k8s
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: default
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
```

---

## 🚀 Deployment Steps

### ✅ 1. Start Minikube and Enable Dashboard

```
minikube start
minikube dashboard &
```
![Screenshot](images/Screenshot%202025-07-28%20234308.png)

---

### ✅ 2. Install ArgoCD

```
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

### ✅ 3. Access ArgoCD UI

Check the NodePort for argocd-server:

```
kubectl get svc -n argocd
```

```
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
argocd-server   NodePort   10.96.99.123    <none>        443:31443/TCP                1m
```
![Screenshot](images/Screenshot%202025-07-28%20234423.png)


Visit ArgoCD UI in the browser:

```
https://192.168.49.2:31443
```

⚠️ Use `https://`, not `http://`.  
Accept SSL warning → Click "Advanced" → "Proceed anyway"

![Screenshot](images/Screenshot%202025-07-28%20235517.png)

---

### ✅ 4. Login to ArgoCD

Get the default admin password:

```
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d && echo
```

Login:
- **Username:** `admin`
- **Password:** (use the above command)

![Screenshot](images/Screenshot%202025-07-28%20235517.png)

![Screenshot](images/Screenshot%202025-07-29%20000153.png)

---

### ✅ 5. Connect App via ArgoCD

Apply the ArgoCD application:

```
kubectl apply -f argocd-app.yaml -n argocd
```

ArgoCD will auto-sync the manifests from:

```
https://github.com/MariamWafik349/todo-list-nodejs.git
```

under the folder:

```
todo-k8s
```
![Screenshot](images/Screenshot%202025-07-29%20004637.png)
![Screenshot](images/Screenshot%202025-07-29%20004727.png)

---

### ✅ 6. Access the Todo App

Get the NodePort for `todo-service`:

```
kubectl get svc todo-service
```

Example output:

```
NAME           TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
todo-service   NodePort   10.100.156.179   <none>        80:31741/TCP   100m
```

Get Minikube IP:

```
minikube ip
```
![Screenshot](images/Screenshot%202025-07-29%20011013.png)


Open the app in browser:

```
http://192.168.49.2:31741
```

✅ You should now see the Todo List app!
![Screenshot](images/Screenshot%202025-07-29%20010737.png)

![Screenshot](images/Screenshot%202025-07-29%20010717.png)

---



