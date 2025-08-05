# DevOps Internship Assessment 🚀

## 📌 Project Overview

The aim is to take a simple Node.js TODO application and apply modern DevOps practices including containerization, configuration management, deployment automation, and continuous delivery using Kubernetes and ArgoCD.

Each part of this assessment focuses on a different key DevOps concept and tool.

---

## 🧩 Project Parts Breakdown

### Part 1: Dockerize the Application and CI Pipeline
- **Clone** the original repository: [https://github.com/Ankit6098/Todo-List-nodejs](https://github.com/Ankit6098/Todo-List-nodejs)
- **Configure** your own MongoDB database by updating the `.env` file.
- **Dockerize** the Node.js application using a `Dockerfile`.
- **Create a CI pipeline** using GitHub Actions that builds the Docker image and pushes it to a **private Docker registry**.

---

### Part 2: VM Provisioning and Docker Setup Using Ansible
- **Create a Linux virtual machine** (VM) either on your local machine or in the cloud.
- **Use Ansible** to provision the VM and install necessary packages, such as Docker.
- **Run Ansible locally**, targeting the remote VM using SSH.

---

### Part 3: Application Deployment with Docker Compose and Auto-Update
- On the **remote VM**, deploy the application using **Docker Compose**.
- Configure **health checks** for the application and MongoDB containers.
- Implement **automatic image updates**: continuously monitor the Docker registry for new versions and pull updated images.
- You may use **any auto-update tool of your choice**, but make sure to **justify your choice** clearly.

---

### Part 4: Kubernetes Deployment and Continuous Delivery with ArgoCD
- Replace Docker Compose by deploying the application on **Kubernetes**.
- Use **ArgoCD** to implement **Continuous Delivery (CD)**, enabling GitOps-style deployment directly from a Git repository to the Kubernetes cluster.

---

## 🛠️ Tools & Technologies Used

| Tool            | Purpose | Link |
|-----------------|---------|------|
| **Git**         | Clone the original repo and manage code versions | [https://git-scm.com/downloads](https://git-scm.com/downloads) |
| **Node.js + npm** | Run and test the Node.js app locally before Dockerizing | [https://nodejs.org/en/download](https://nodejs.org/en/download) |
| **Docker Desktop** | Create Dockerfile, build images, and run containers | [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) |
| **VS Code**     | Edit and manage project files | [https://code.visualstudio.com/](https://code.visualstudio.com/) |
| **MongoDB Atlas** *(optional)* | Cloud MongoDB service for development or testing | [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas) |
| **Ansible**     | Automate provisioning and configuration of the VM | [https://www.ansible.com/](https://www.ansible.com/) |
| **VMware Workstation** | Local virtualization software to create Linux VMs | [https://www.vmware.com/products/workstation-pro.html](https://www.vmware.com/products/workstation-pro.html) |
| **Minikube**    | Lightweight local Kubernetes cluster for development | [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/) |
| **kubectl**     | Command-line tool to manage Kubernetes | [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/) |
| **ArgoCD**      | GitOps-based Continuous Delivery tool for Kubernetes | [https://argo-cd.readthedocs.io/en/stable/](https://argo-cd.readthedocs.io/en/stable/) |

---

## 📝 Note

> This project was built for learning and demonstration purposes. The original Node.js code was cloned from [this repo](https://github.com/Ankit6098/Todo-List-nodejs) and customized as part of the DevOps internship challenge.

## ✅ Part 1: Clone, Configure, Dockerize & Automate CI

### 📁 Step 1: Clone the Project

Make sure Docker Desktop is installed and running.

Open PowerShell or Command Prompt and run:

```
git clone https://github.com/Ankit6098/Todo-List-nodejs
cd Todo-List-nodejs
```
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20142254.png)
![Screenshot](images/Screenshot%202025-07-26%20162003.png)





---

### 🧪 Step 2: Run the App Locally with MongoDB

🔧 Install dependencies

```
npm install
```

🐳 Start MongoDB using Docker

```
docker run -d -p 27017:27017 --name mongo mongo:latest
```

✍️ Create `.env` file

```
MONGO_URI=mongodb://localhost:27017/todo
PORT=3000
```

🚀 Start the application

```
npm start
```

Open your browser and navigate to:

```
http://localhost:4000
```
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20142052.png)

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20142008.png)

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20142636.png)
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20152928.png)


---

### 🐳 Step 3: Dockerize the Application

📄 Create a Dockerfile

```
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

📄 Create `.dockerignore`

```
node_modules
npm-debug.log
```

🧱 Build the Docker Image

```
docker build -t todo-app .
```

▶️ Run the Docker Container

```
docker run -p 3000:3000 --env-file .env todo-app
```

Navigate to:

```
http://localhost:4000
```

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20151457.png)

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20151355.png)
![Screenshot](images/Screenshot%202025-07-26%20165846.png)

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20152956.png)


![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20151516.png)



![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20151336.png)
---

### 📤 Step 4: Push Docker Image to Docker Hub

🐳 Login to Docker Hub

```
docker login
```

Enter:

- Username: `mariamwafik333`
- Password: `your Docker Hub password`

🏷️ Tag the Image

```
docker tag todo-app mariamwafik333/todo-app:latest
```

📦 Push the Image

```
docker push mariamwafik333/todo-app:latest
```

You can now view your image on Docker Hub:  
`https://hub.docker.com/r/mariamwafik333/todo-app`

![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20153352.png)
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20153336.png)
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20153754.png)
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20153939.png)
![Screenshot](https://raw.githubusercontent.com/MariamWafik349/devops-intern-assessment/main/images/Screenshot%202025-07-26%20153939.png)
![Screenshot](images/Screenshot%202025-07-26%20181259.png)


---

### ⚙️ Step 5: GitHub Actions CI Pipeline

📁 Create a GitHub repository:  
`https://github.com/MariamWafik349/devops-intern-assessment.git`

⬆️ Push your project to GitHub

```
git init
git remote add origin https://github.com/MariamWafik349/devops-intern-assessment.git
git add .
git commit -m "Initial commit with Dockerfile"
git branch -M main
git push -u origin main
```
![Screenshot 2025-07-26 160800](images/Screenshot%202025-07-26%20160800.png)
![Screenshot](images/Screenshot%202025-07-26%20162101.png)



🔐 Add GitHub Secrets

Go to your repo → **Settings** → **Secrets and variables** → **Actions**

Add:

- `DOCKER_USERNAME` = `mariamwafik333`
- `DOCKER_PASSWORD` = `your Docker Hub password`

🛠️ Create GitHub Actions Workflow

Create folder and file:

```
mkdir -p .github/workflows
touch .github/workflows/docker-build.yml
```

Paste this in `docker-build.yml`:

```
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: |
          docker build -t mariamwafik333/todo-app:latest .

      - name: Push Docker image
        run: |
          docker push mariamwafik333/todo-app:latest
```

✅ Test the Pipeline

```
git add .
git commit -m "Add GitHub Actions workflow for Docker build"
git push origin main
```

Check your GitHub Actions tab to see the build running.

![Screenshot](images/Screenshot%202025-07-26%20221329.png)
![Screenshot](images/Screenshot%202025-07-26%20221348.png)

![Screenshot](images/Screenshot%202025-07-26%20180800.png)

# Updated 'docker-build.yaml':
Paste this in `docker-build.yml`:

```
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set Docker Tag (date only)
        run: echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_ENV

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: mariamwafik333/todo-app:${{ env.IMAGE_TAG }}

  scan:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Set Docker Tag (date only)
        run: echo "IMAGE_TAG=${GITHUB_SHA::7}" >> $GITHUB_ENV

      - name: Scan Docker Image with Trivy
        uses: aquasecurity/trivy-action@0.19.0
        with:
          image-ref: mariamwafik333/todo-app:${{ env.IMAGE_TAG }}
          format: table
          exit-code: 0
          ignore-unfixed:
```

✅ Test the Pipeline

```
git add .
git commit -m "Add GitHub Actions workflow for Docker build"
git push origin main
```

Check your GitHub Actions tab to see the build running.

![App Screenshot](./images/Screenshot%202025-08-05%20115651.png)

![App Screenshot](./images/Screenshot%202025-08-05%20115710.png)


![App Screenshot](./images/Screenshot%202025-08-05%20115355.png)

---

### 🎉 Summary

- ✅ Cloning and preparing the Node.js project
- ✅ Running MongoDB in Docker
- ✅ Dockerizing the application
- ✅ Pushing Docker image to Docker Hub
- ✅ Setting up GitHub Actions for CI/CD

Your app is now available at:  
```
http://localhost:4000
```

And your Docker image is published at:  
`https://hub.docker.com/r/mariamwafik333/todo-app`
## ✅ Part 2: Provisioning a VM and Installing Docker using Ansible

---

### ✅ Step 1: Create an EC2 Linux VM on AWS

#### 🎯 Goal:

Create an Ubuntu server on AWS EC2 and connect to it via SSH.

#### 🔷 Step-by-step Instructions:

1. **Login to AWS Management Console**
   Go to [https://aws.amazon.com](https://aws.amazon.com), click **Sign In**, and enter your AWS credentials.

2. **Open EC2 Service**
   Use the top search bar to find `EC2`. Click the result: **EC2 - Virtual Servers in the Cloud**.

3. **Launch a new Instance**

| Setting       | Value                       |
| ------------- | --------------------------- |
| Name          | `devops-vm`                 |
| AMI           | `Ubuntu 22.04 LTS`          |
| Instance type | `t2.micro` (Free tier)      |
| Key pair      | Click `Create new key pair` |
| Key name      | `devops-key`                |
| Type          | `RSA`                       |
| Format        | `.pem`                      |

Click **Create key pair** → A file named `devops-key.pem` will download → Save it securely.

4. **Configure Network Settings**
   Click **Edit** in "Network settings" section:

* Set **Allow SSH traffic from:** `Anywhere (0.0.0.0/0)`

Click **Launch Instance**.

5. **Get the Public IP Address**
   Navigate to **Instances** in the left panel.

* Locate your instance `devops-vm`
* Copy the value under **Public IPv4 address** (e.g., `3.123.45.67`)

---

### ✅ Step 2: SSH into EC2 from Your Local Machine

#### 🎯 Goal:

Remotely connect to the EC2 instance using your `.pem` key.

1. **Open Git Bash**
   If not installed, download from: [https://git-scm.com/downloads](https://git-scm.com/downloads)

2. **Navigate to the Key File Location**

```
cd ~/Downloads
# Or wherever your devops-key.pem is stored
```

3. **Change Key File Permissions**

```
chmod 400 devops-key.pem
```

4. **SSH into EC2**

```
ssh -i devops-key.pem ubuntu@3.123.45.67
# Replace the IP with your EC2's public IP
```

If successful, you're inside the server 🎉

---
![Screenshot](images/Screenshot%202025-07-27%20173937.png)


![Screenshot](images/Screenshot%202025-07-26%20225959.png)


### ✅ Step 3: Install Ansible on Your Local Machine

> If you're using WSL (Ubuntu on Windows):

Open Ubuntu terminal and run:

```
sudo apt update
sudo apt install ansible -y
```
![Screenshot](images/Screenshot%202025-07-26%20232301.png)

![Screenshot](images/Screenshot%202025-07-26%20225331.png)


---

### ✅ Step 4: Create Ansible Inventory and Playbook

1. **Create Project Folder**

```
mkdir aws-ansible
cd aws-ansible
```

2. **Create Inventory File**

```
nano inventory.ini
```

Paste the following (replace the IP with your EC2 IP):

```
[ec2]
3.123.45.67 ansible_user=ubuntu ansible_ssh_private_key_file=~/Downloads/devops-key.pem ansible_python_interpreter=/usr/bin/python3
```

Save with: `Ctrl + O` → Enter → `Ctrl + X`

3. **Create Ansible Playbook File**

```
nano install-docker.yml
```

Paste the following YAML:

```
---
- name: Install Docker on EC2 instance
  hosts: ec2
  become: true

  tasks:
    - name: Update apt
      apt:
        update_cache: yes

    - name: Install dependencies
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
        state: present

    - name: Add Docker GPG key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest

    - name: Start and enable Docker
      systemd:
        name: docker
        enabled: yes
        state: started
```

![Screenshot](images/Screenshot%202025-07-27%20173420.png)


---

### ✅ Step 5: Run Ansible Playbook

From the `aws-ansible` directory:

```
ansible-playbook -i inventory.ini install-docker.yml
```

You should see progress logs. If all tasks show `ok` or `changed`, you're good!

---

### ✅ Verify Docker Installation

SSH into your EC2 again:

```
ssh -i ~/Downloads/devops-key.pem ubuntu@3.123.45.67
```

Run:

```
docker --version
```

Expected output:

```
Docker version 24.0.x, build xxxxx
```
# Part 3: Deploy Todo App on Remote VM using Docker Compose

## 🎯 Objective

Deploy the Node.js Todo App (`mariamwafik333/todo-app`) on a remote Ubuntu VM using Docker Compose. The app connects to MongoDB Atlas using a `.env` file, includes a health check for the app, and uses Watchtower to automatically pull updated images from Docker Hub when pushed.

---

## ✅ Step 1: Prepare the Remote Ubuntu VM

### 🔹 Connect to VM via SSH
```
ssh -i ~/Downloads/devops-key.pem ubuntu@<YOUR_PUBLIC_IP>
```

### 🔹 Update Packages and Install Docker + Docker Compose
```
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl start docker
sudo systemctl enable docker
```
![Screenshot](images/Screenshot%202025-07-27%20175100.png)

---

## ✅ Step 2: Setup Project Directory and Files

### 🔹 Create Working Directory
```
mkdir ~/todo-compose
cd ~/todo-compose
```

### 🔹 Create `.env` File for MongoDB Connection
```
nano .env
```

Paste your MongoDB Atlas URI (replace `<username>` and `<password>`):
```env
MONGO_URL=mongodb+srv://<username>:<password>@cluster0.mongodb.net/todo?retryWrites=true&w=majority
```

## ✅ Step 3: Create Docker Compose File

### 🔹 Create `docker-compose.yml`
```
nano docker-compose.yml
```

Paste the following configuration:
```
version: "3.8"

services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    healthcheck:
      test: ["CMD", "mongo", "--eval", "db.adminCommand('ping')"]
      interval: 10s
      timeout: 5s
      retries: 5

  todo-app:
    image: mariamwafik333/todo-app
    container_name: todo-app
    ports:
      - "3000:3000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017/test
    depends_on:
      - mongodb
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000"]
      interval: 10s
      timeout: 5s
      retries: 5

  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
    command: --interval 30

volumes:
  mongo-data: {}

```
![Screenshot](images/Screenshot%202025-07-27%20193509.png)
![Screenshot](images/Screenshot%202025-07-27%20193631.png)




## ✅ Step 4: Start the App Using Docker Compose

### 🔹 Launch the app
```
sudo docker-compose up -d
```
![Screenshot](images/Screenshot%202025-07-27%20192435.png)
![Screenshot](images/Screenshot%202025-07-27%20192503.png)
![Screenshot](images/Screenshot%202025-07-27%20193056.png)

---

## ✅ Step 5: Verify the Deployment

### 🔹 View Running Containers
```
sudo docker ps
```

You should see:
- `todo-app`
- `watchtower`

### 🔹 Check App Health
```
sudo docker inspect --format='{{json .State.Health}}' todo-app
```

Expected output:
```json
{"Status":"healthy", ...}
```

### 🔹 Access the Todo App in Browser

Visit:
```
http://<YOUR_PUBLIC_IP>:4000
```

You should see the Node.js Todo App interface.

---

## ✅ Step 6: Enable Auto-Update with Watchtower

Watchtower checks every **30 seconds** for a new version of:

```
docker.io/mariamwafik333/todo-app:latest
```

If found, it pulls and redeploys the updated image automatically.


![Screenshot](images/Screenshot%202025-07-27%20193108.png)
![Screenshot](images/Screenshot%202025-07-27%20193436.png)
![Screenshot](images/Screenshot%202025-07-27%20193446.png)

---

## ✅ Directory Structure

```
~/todo-compose/
├── .env
└── docker-compose.yml
```

---
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



