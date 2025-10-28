# Full CI/CD Deployment Lab: Jenkins Pipeline for Node.js, Docker, and Kubernetes

## Aim
To implement a complete CI/CD pipeline using Jenkins that:
- Builds the code using a package manager (npm)
- Builds a Docker image
- Pushes the image to Docker Hub
- Deploys the image on Kubernetes

## Requirements
- Jenkins installed and running
- Docker installed on Jenkins server/agent
- Docker Hub account ([https://hub.docker.com/](https://hub.docker.com/))
- Kubernetes cluster (local Minikube, Docker Desktop, or cloud)
- Node.js sample project with Dockerfile (see below)

---

## Step 1: Prepare Node.js Project with Dockerfile

### Project Structure
```
my-web-app/
├── package.json
├── app.js
├── Dockerfile
├── Jenkinsfile
├── k8s-deployment.yaml
```

### `package.json`
```json
{
  "name": "my-web-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

### `app.js`
```js
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Dockerized Node.js App!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### `Dockerfile`
```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY app.js .
EXPOSE 3000
CMD ["npm", "start"]
```

### `k8s-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-web-app
  template:
    metadata:
      labels:
        app: my-web-app
    spec:
      containers:
      - name: my-web-app
        image: yourusername/my-web-app:latest # Replace with your Docker Hub username
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: my-web-app-service
spec:
  type: NodePort
  selector:
    app: my-web-app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 32000
```

---

## Step 2: Create Jenkinsfile for CI/CD Pipeline

### `Jenkinsfile`
```groovy
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credentials ID
        DOCKERHUB_USER = 'yourusername' // Replace with your Docker Hub username
        IMAGE_NAME = "my-web-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:latest")
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_USER --password-stdin"
                }
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                script {
                    dockerImage.push()
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
```

**Explanation:**
- The pipeline checks out code, installs npm dependencies, builds and pushes the Docker image, and deploys to Kubernetes using `kubectl`.
- You must add your Docker Hub credentials in Jenkins and use the correct credentials ID.
- Jenkins agent must have `kubectl` installed and configured to access your Kubernetes cluster.

---

## Step 3: Configure Jenkins Job
- Create a new Pipeline job in Jenkins.
- Set your repository URL.
- Ensure Docker and kubectl are installed and accessible to Jenkins.
- Add Docker Hub credentials in Jenkins and reference them in the Jenkinsfile.
- Ensure your Kubernetes cluster is running and accessible.

---

## Step 4: Run the Pipeline
- Click **Build Now** in Jenkins job.
- Watch the stages execute:
  - Checkout
  - Install Dependencies
  - Build Docker Image
  - Login to Docker Hub
  - Push Image to Docker Hub
  - Deploy to Kubernetes
- Check Docker Hub for the image and Kubernetes for the running app.

---

## Explanation of Key Commands
- **npm install**: Installs Node.js dependencies.
- **docker.build**: Builds a Docker image from the Dockerfile.
- **docker login**: Authenticates Jenkins with Docker Hub.
- **dockerImage.push()**: Pushes the built image to Docker Hub.
- **kubectl apply -f**: Deploys the app to Kubernetes using the deployment YAML file.

---

## Expected Outcome
- Jenkins pipeline builds the code, Docker image, pushes to Docker Hub, and deploys to Kubernetes automatically.
- The app is available in your Kubernetes cluster and accessible via NodePort 32000.

---

## Notes
- Replace `yourusername` and `dockerhub-creds` with your actual Docker Hub username and Jenkins credentials ID.
- For troubleshooting, check Jenkins build logs, Docker command output, and Kubernetes status.
- For more info, see [Jenkins Pipeline documentation](https://www.jenkins.io/doc/book/pipeline/) and [Kubernetes documentation](https://kubernetes.io/docs/).
