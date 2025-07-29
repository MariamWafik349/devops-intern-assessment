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
PORT=4000
```

🚀 Start the application

```
npm start
```

Open your browser and navigate to:

```
http://localhost:4000
```

---

### 🐳 Step 3: Dockerize the Application

📄 Create a Dockerfile

```
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 4000

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
docker run -p 4000:4000 --env-file .env todo-app
```

Navigate to:

```
http://localhost:4000
```

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

---

### ⚙️ Step 5: GitHub Actions CI Pipeline

📁 Create a GitHub repository:  
`https://github.com/MariamWafik349/todo-list-nodejs.git`

⬆️ Push your project to GitHub

```
git init
git remote add origin https://github.com/MariamWafik349/todo-list-nodejs.git
git add .
git commit -m "Initial commit with Dockerfile"
git branch -M main
git push -u origin main
```

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
## 🚀 Part 2: VM Provisioning and Docker Setup Using Ansible

This part covers setting up an EC2 instance on AWS and provisioning it using Ansible to install Docker.

---

### ✅ Step 1: Create a Linux VM on AWS (EC2)

#### 🎯 Goal:

Create an Ubuntu server on AWS and access it via SSH.

#### 🔷 Launch EC2 Instance

1. Go to: [https://aws.amazon.com](https://aws.amazon.com)
2. Click **Sign In** and login to your AWS account.
3. In the AWS Management Console, search for `EC2` in the top search bar.
4. Click **EC2 - Virtual Servers in the Cloud**.
5. Click **Launch Instance** and fill in the following:

| Field                          | Value                  |
| ------------------------------ | ---------------------- |
| Name                           | `devops-vm`            |
| Application and OS Image (AMI) | `Ubuntu 22.04 LTS`     |
| Instance type                  | `t2.micro` (Free tier) |
| Key pair                       | Create new key pair    |
| Key name                       | `devops-key`           |
| Type                           | RSA                    |
| Format                         | `.pem`                 |

6. Click **Create key pair** → the file `devops-key.pem` will be downloaded. Save it in a known directory.
7. Under **Network settings**, click **Edit** → set **Allow SSH traffic from** to `Anywhere (0.0.0.0/0)`.
8. Click **Launch Instance**.

#### 🔷 Get the Public IP

* From the left sidebar, click **Instances**.
* Locate `devops-vm` and copy the **Public IPv4 address** (e.g., `3.123.45.67`).

---

### ✅ Step 2: Connect to EC2 using SSH

#### 🎯 Goal:

Connect to the server remotely using the `.pem` key.

#### 🔷 Open Git Bash

If not installed, download it from:
[https://git-scm.com/downloads](https://git-scm.com/downloads)

Navigate to the folder containing `devops-key.pem`:

```
cd ~/Downloads
# Or specify the correct path:
cd D:/Keys
```

#### 🔷 Set Key Permissions

```
chmod 400 devops-key.pem
```

#### 🔷 Connect to EC2 Instance

```
ssh -i devops-key.pem ubuntu@3.123.45.67
# Replace with your actual IP address
```

✅ If you're connected with no errors, you're inside the server! 💪

---

### ✅ Step 3: Install Ansible on Your Local Machine

If you’re using WSL (Ubuntu on Windows):

```
sudo apt update
sudo apt install ansible -y
```

---

### ✅ Step 4: Create Ansible Files

#### 📁 Create Working Directory

```
mkdir aws-ansible
cd aws-ansible
```

#### 📄 Create Inventory File

```
nano inventory.ini
```

Add the following:

```
[ec2]
3.123.45.67 ansible_user=ubuntu ansible_ssh_private_key_file=~/Downloads/devops-key.pem ansible_python_interpreter=/usr/bin/python3
```

Replace with your actual IP.

To save and exit:

* Press `Ctrl + O`, then `Enter`
* Press `Ctrl + X`

#### 📄 Create Playbook to Install Docker

```
nano install-docker.yml
```

Paste the following YAML code:

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

To save and exit:

* Press `Ctrl + O`, then `Enter`
* Press `Ctrl + X`

---

### ✅ Step 5: Run Ansible Playbook

Run the following command in the same directory:

```
ansible-playbook -i inventory.ini install-docker.yml
```

Watch the logs. If you see `ok` or `changed` on all tasks, everything is set up correctly. 👌

---

### ✅ Step 6: Verify Docker Installation on EC2

SSH again into the EC2 instance:

```
ssh -i ~/Downloads/devops-key.pem ubuntu@3.123.45.67
```

Then run:

```
docker --version
```

If you see something like:

```
Docker version 24.0.x, build xxxxx
```

You're done! 🎉


