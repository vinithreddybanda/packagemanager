# Dockerfile & Image Build Lab (Node.js Web App)

## Aim
To write a Dockerfile for a simple Node.js web application, build a Docker image, run it as a container, and verify the application running on localhost.

## Requirements
- Docker installed and running
- Node.js installed (for local testing)
- Sample Node.js web application (see below)

---

## Sample Node.js Web Application

### Project Structure
```
my-web-app/
├── package.json
├── app.js
├── Dockerfile
```

### 1. Create Application Files

#### `package.json`
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

#### `app.js`
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

### 2. Write the Dockerfile

#### `Dockerfile`
```Dockerfile
# Use official Node.js image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package.json .
RUN npm install

# Copy application code
COPY app.js .

# Expose port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

---

## Procedure

### Step 1: Prepare the Project
- Create the above files in a folder named `my-web-app`.
- Open a terminal in this folder.

### Step 2: Build the Docker Image
```sh
docker build -t my-web-app .
```
- This command builds the image and tags it as `my-web-app`.

### Step 3: Run the Docker Container
```sh
docker run -d -p 3000:3000 my-web-app
```
- Runs the container in detached mode and maps port 3000 of the container to port 3000 on localhost.

### Step 4: Verify the Application
- Open a web browser and go to [http://localhost:3000](http://localhost:3000)
- You should see: `Hello from Dockerized Node.js App!`

### Step 5: Check Container Status
```sh
docker ps
```
- Shows running containers. Confirm your container is listed.

### Step 6: Stop and Remove the Container
- Find the CONTAINER ID from `docker ps`.
```sh
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

---

## Expected Outcome
- Docker image is built successfully.
- Container runs the Node.js app and is accessible on localhost:3000.
- You can stop and remove the container after verification.

---

## Notes
- You can use any simple web app (Flask, Express, etc.) for this lab.
- For troubleshooting, check Docker build and run logs.
- For more info, see [Docker documentation](https://docs.docker.com/).
