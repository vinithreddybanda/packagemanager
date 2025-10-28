# Kubernetes Deployment Lab: Deploying a Dockerized Web Application

## Aim
To deploy a Dockerized web application on a Kubernetes cluster using kubectl, with step-by-step creation of deployment and service YAML files, and verification of running pods and services.

## Requirements
- Docker image for your web app (e.g., pushed to Docker Hub)
- Kubernetes cluster (Minikube, Docker Desktop, or cloud)
- kubectl installed and configured

---

## Step 1: Prepare Docker Image
- Ensure your Docker image is available on Docker Hub (e.g., `yourusername/my-web-app:latest`).

---

## Step 2: Create Kubernetes Deployment YAML
Create a file named `k8s-deployment.yaml`:

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
```

**Explanation:**
- `Deployment` manages the desired state for your app (number of replicas, image, etc.).
- `replicas: 1` means one pod will run.
- `image` specifies the Docker image to use.
- `containerPort: 3000` exposes port 3000 inside the container.

---

## Step 3: Create Kubernetes Service YAML
Create a file named `k8s-service.yaml`:

```yaml
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

**Explanation:**
- `Service` exposes your app to the network.
- `type: NodePort` makes the app accessible on a port of the node (e.g., 32000).
- `selector` matches pods with label `app: my-web-app`.
- `port` is the service port, `targetPort` is the container port, `nodePort` is the external port.

---

## Step 4: Deploy to Kubernetes
Apply the deployment and service files:
```sh
kubectl apply -f k8s-deployment.yaml
kubectl apply -f k8s-service.yaml
```
- This creates the deployment and service in your cluster.

---

## Step 5: Verify Pods and Services
Check that your app is running:
```sh
kubectl get pods
kubectl get svc
```
- `kubectl get pods` lists all pods and their status.
- `kubectl get svc` lists all services and their ports.

---

## Step 6: Access the Application
- Find the NodePort (e.g., 32000) from `kubectl get svc` output.
- Open a browser and go to `http://<NodeIP>:32000` (use `minikube ip` or your node's IP).
- You should see your web app running.

---

## Expected Outcome
- The Dockerized web app is deployed and running in Kubernetes.
- Pods and services are visible and healthy.
- The app is accessible via the NodePort.

---

## Notes
- Replace `yourusername` with your actual Docker Hub username.
- For troubleshooting, use `kubectl describe pod <podname>` and `kubectl logs <podname>`.
- For more info, see [Kubernetes documentation](https://kubernetes.io/docs/).
