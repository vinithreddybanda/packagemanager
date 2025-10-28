# Docker Installation & Basic Commands Lab

## Aim
To install Docker, verify the installation, and demonstrate essential Docker commands for container lifecycle management:
- `docker pull`
- `docker run`
- `docker ps`
- `docker stop`
- `docker rm`

## Requirements
- A computer with Windows, Linux, or macOS
- Internet connection
- Administrator privileges

---

## Procedure

### 1. Install Docker
#### Windows
- Download Docker Desktop from [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
- Run the installer and follow the prompts
- After installation, restart your computer if prompted

#### Linux (Ubuntu Example)
```sh
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

#### macOS
- Download Docker Desktop from [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
- Install and launch Docker Desktop

### 2. Verify Docker Installation
Open a terminal or command prompt and run:
```sh
docker --version
docker info
```
- You should see Docker version and system info if installed correctly.

### 3. Demonstrate Docker Commands

#### a) Pull an Image
```sh
docker pull hello-world
```
- Downloads the `hello-world` image from Docker Hub.

#### b) Run a Container
```sh
docker run hello-world
```
- Runs the `hello-world` container, which prints a welcome message and exits.

#### c) List Running Containers
```sh
docker ps
```
- Shows all currently running containers.

#### d) List All Containers (including stopped)
```sh
docker ps -a
```
- Shows all containers, running and stopped.

#### e) Stop a Running Container
- First, run a container that stays active:
  ```sh
  docker run -d nginx
  ```
- List running containers and note the CONTAINER ID:
  ```sh
  docker ps
  ```
- Stop the container:
  ```sh
  docker stop <CONTAINER_ID>
  ```

#### f) Remove a Container
- Remove the stopped container:
  ```sh
  docker rm <CONTAINER_ID>
  ```

---

## Container Lifecycle Example
1. **Pull** an image (`docker pull nginx`)
2. **Run** a container (`docker run -d nginx`)
3. **List** containers (`docker ps`)
4. **Stop** the container (`docker stop <CONTAINER_ID>`)
5. **Remove** the container (`docker rm <CONTAINER_ID>`)

---

## Expected Outcome
- Docker is installed and verified.
- Images are pulled and containers are run.
- You can list, stop, and remove containers, demonstrating the container lifecycle.

---

## Notes
- Replace `<CONTAINER_ID>` with the actual ID from `docker ps` output.
- You can repeat these steps with other images (e.g., `ubuntu`, `node`, etc.).
- For troubleshooting, check Docker documentation: [https://docs.docker.com/](https://docs.docker.com/)
