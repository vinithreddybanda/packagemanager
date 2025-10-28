# Docker + Jenkins CI/CD Integration Lab

## Aim
To integrate Docker into a Jenkins pipeline by building a Docker image from source code using a Dockerfile and automatically pushing the image to Docker Hub via the pipeline.

## Requirements
- Jenkins installed and running
- Docker installed on Jenkins server/agent
- Docker Hub account ([https://hub.docker.com/](https://hub.docker.com/))
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
    }
    post {
        always {
            cleanWs()
        }
    }
}
```

**Explanation:**
- The pipeline checks out the code, builds the Docker image, logs in to Docker Hub using Jenkins credentials, and pushes the image.
- You must add your Docker Hub credentials in Jenkins (Manage Jenkins > Credentials) and use the correct credentials ID in the Jenkinsfile.

---

## Step 3: Configure Jenkins Job
- Create a new Pipeline job in Jenkins.
- Set your repository URL.
- Ensure Docker is installed and accessible to Jenkins.
- Add Docker Hub credentials in Jenkins and reference them in the Jenkinsfile.

---

## Step 4: Run the Pipeline
- Click **Build Now** in Jenkins job.
- Watch the stages execute:
  - Checkout
  - Build Docker Image
  - Login to Docker Hub
  - Push Image to Docker Hub
- Check Docker Hub to verify the image is pushed.

---

## Explanation of Key Commands
- **docker.build**: Builds a Docker image from the Dockerfile in the workspace.
- **docker login**: Authenticates Jenkins with Docker Hub using credentials.
- **dockerImage.push()**: Pushes the built image to Docker Hub.

---

## Expected Outcome
- Jenkins pipeline builds the Docker image and pushes it to Docker Hub automatically.
- The image is available in your Docker Hub repository.

---

## Notes
- Replace `yourusername` and `dockerhub-creds` with your actual Docker Hub username and Jenkins credentials ID.
- For troubleshooting, check Jenkins build logs and Docker command output.
- For more info, see [Jenkins Docker Pipeline documentation](https://www.jenkins.io/doc/book/pipeline/docker/).
