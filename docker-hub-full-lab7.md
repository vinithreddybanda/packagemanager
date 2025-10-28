# Complete Lab: Build, Tag, and Push Node.js Docker Image to Docker Hub

## Aim
To build a Docker image for a Node.js web application, tag it, push it to Docker Hub, and verify by pulling and running the image on another system.

## Requirements
- Docker installed and running
- Node.js installed (for local testing)
- Docker Hub account ([https://hub.docker.com/](https://hub.docker.com/))

---

## Step 1: Create a Simple Node.js Web Application

### Project Structure
```
my-web-app/
├── package.json
├── app.js
├── Dockerfile
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

## Step 2: Build the Docker Image
Open a terminal in the project folder and run:
```sh
docker build -t my-web-app .
```
- This command builds the image and tags it as `my-web-app`.

---

## Step 3: Run and Test the Image Locally
```sh
docker run -d -p 3000:3000 my-web-app
```
- Runs the container in detached mode and maps port 3000 of the container to port 3000 on localhost.
- Open a browser and visit [http://localhost:3000](http://localhost:3000) to verify the app is running.

---

## Step 4: Login to Docker Hub
```sh
docker login
```
- Enter your Docker Hub username and password to authenticate.

---

## Step 5: Tag the Image for Docker Hub
Suppose your Docker Hub username is `yourusername`:
```sh
docker tag my-web-app yourusername/my-web-app:latest
```
- Tags the image for your Docker Hub repository.

---

## Step 6: Push the Image to Docker Hub
```sh
docker push yourusername/my-web-app:latest
```
- Uploads the image to your Docker Hub account.

---

## Step 7: Pull and Run the Image on Another System
On any system with Docker installed:
```sh
docker pull yourusername/my-web-app:latest
docker run -d -p 3000:3000 yourusername/my-web-app:latest
```
- Pulls the image from Docker Hub and runs it.
- Verify by visiting [http://localhost:3000](http://localhost:3000).

---

## Explanation of Commands
- **docker build**: Creates a Docker image from your project files and Dockerfile.
- **docker run**: Starts a container from the image.
- **docker login**: Authenticates your Docker client with Docker Hub.
- **docker tag**: Prepares your image for upload to Docker Hub by naming it appropriately.
- **docker push**: Uploads your image to Docker Hub.
- **docker pull**: Downloads your image from Docker Hub to any system.

---

## Expected Outcome
- Node.js app is containerized, image is built, tagged, pushed, and pulled from Docker Hub.
- App runs successfully on localhost after pulling from Docker Hub.

---

## Notes
- Replace `yourusername` with your actual Docker Hub username.
- For troubleshooting, check Docker command output and Docker Hub documentation: [https://docs.docker.com/](https://docs.docker.com/)
