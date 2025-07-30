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
