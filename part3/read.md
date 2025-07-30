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
version: '3.8'

services:
  todo-app:
    image: mariamwafik333/todo-app:latest
    container_name: todo-app
    restart: always
    env_file:
      - .env
    ports:
      - "4000:4000"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000"]
      interval: 30s
      timeout: 10s
      retries: 3

  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --interval 30
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
