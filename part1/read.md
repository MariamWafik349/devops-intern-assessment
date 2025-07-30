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
![Screenshot](images/Screenshot%202025-07-26%20162101.png)

![Screenshot](images/Screenshot%202025-07-26%20221329.png)
![Screenshot](images/Screenshot%202025-07-26%20221348.png)

![Screenshot](images/Screenshot%202025-07-26%20180800.png)




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

![Screenshot 2025-07-26 160800](images/Screenshot%202025-07-26%20160800.png)



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
