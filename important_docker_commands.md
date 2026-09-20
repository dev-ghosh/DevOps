# Essential Docker Commands Reference Guide

A comprehensive cheat sheet for developers and DevOps engineers covering the most commonly used Docker commands, categorized by operational context.

---

## 🛠️ General & System Commands

Commands for system maintenance, version checking, and inspecting Docker environment resource usage.

| Command | Description |
| :--- | :--- |
| `docker version` | Display detailed Docker client and server version information |
| `docker info` | Display system-wide information regarding container counts, images, storage driver, etc. |
| `docker system df` | Show Docker disk usage by containers, images, local volumes, and build cache |
| `docker system prune` | Remove unused data (stopped containers, dangling networks, and dangling images) |
| `docker system prune -a --volumes` | Deep clean: Remove **all** unused containers, images, networks, and volumes |
| `docker login` | Log in to a Docker registry (defaults to Docker Hub) |
| `docker logout` | Log out from a Docker registry |

---

## 🖼️ Image-Based Commands

Commands for building, pulling, inspecting, tagging, and managing Docker images.

### 1. Image Lifecycle & Registry
* **Pull an image from Docker Hub / Registry:**
  ```bash
  docker pull <image_name>:<tag>
  # Example: docker pull nginx:latest
  ```

* **Push an image to a registry:**
  ```bash
  docker push <repository_name>/<image_name>:<tag>
  # Example: docker push myusername/my-app:v1.0
  ```

* **Tag an existing image:**
  ```bash
  docker tag <source_image>:<tag> <target_image>:<tag>
  # Example: docker tag my-app:latest myusername/my-app:v1.0
  ```

### 2. Building Images
* **Build an image from a Dockerfile in current directory:**
  ```bash
  docker build -t <image_name>:<tag> .
  ```

* **Build specifying a custom Dockerfile path:**
  ```bash
  docker build -f /path/to/Dockerfile -t <image_name>:<tag> .
  ```

* **Build without using cache:**
  ```bash
  docker build --no-cache -t <image_name>:<tag> .
  ```

### 3. Listing & Inspecting Images
* **List locally stored images:**
  ```bash
  docker image ls
  # Alternative: docker images
  ```

* **Inspect image metadata (JSON format):**
  ```bash
  docker image inspect <image_name_or_id>
  ```

* **Show history/layers of an image:**
  ```bash
  docker image history <image_name_or_id>
  ```

### 4. Cleaning & Removing Images
* **Remove a specific image:**
  ```bash
  docker rmi <image_name_or_id>
  # Alternative: docker image rm <image_name_or_id>
  ```

* **Force remove an image (even if referenced by stopped containers):**
  ```bash
  docker rmi -f <image_name_or_id>
  ```

* **Remove all unused/dangling images:**
  ```bash
  docker image prune
  ```

* **Remove all images not used by existing containers:**
  ```bash
  docker image prune -a
  ```

---

## 📦 Container-Based Commands

Commands for running, managing, monitoring, and executing processes inside containers.

### 1. Creating & Running Containers
* **Run a container in interactive mode with terminal attached:**
  ```bash
  docker run -it <image_name> /bin/bash
  ```

* **Run a container in background (detached mode):**
  ```bash
  docker run -d --name <container_name> <image_name>
  ```

* **Run with port mapping and environment variables:**
  ```bash
  docker run -d     --name web-app     -p 8080:80     -e NODE_ENV=production     nginx:latest
  ```

* **Run a container and auto-remove it upon exit:**
  ```bash
  docker run --rm -it ubuntu:latest bash
  ```

* **Run container with volume mount:**
  ```bash
  docker run -d -v /host/path:/container/path <image_name>
  ```

### 2. Listing & Managing Container States
* **List running containers:**
  ```bash
  docker ps
  # Alternative: docker container ls
  ```

* **List all containers (running and stopped):**
  ```bash
  docker ps -a
  ```

* **Start a stopped container:**
  ```bash
  docker start <container_id_or_name>
  ```

* **Stop a running container gracefully:**
  ```bash
  docker stop <container_id_or_name>
  ```

* **Force kill a running container:**
  ```bash
  docker kill <container_id_or_name>
  ```

* **Restart a container:**
  ```bash
  docker restart <container_id_or_name>
  ```

* **Pause / Unpause container processes:**
  ```bash
  docker pause <container_id_or_name>
  docker unpause <container_id_or_name>
  ```

### 3. Inspecting & Monitoring Containers
* **View container logs:**
  ```bash
  docker logs <container_id_or_name>
  ```

* **Follow live log output (tail mode):**
  ```bash
  docker logs -f --tail 100 <container_id_or_name>
  ```

* **Inspect full details of a container (JSON format):**
  ```bash
  docker inspect <container_id_or_name>
  ```

* **View live resource usage metrics (CPU, Memory, I/O):**
  ```bash
  docker stats
  ```

* **List processes running inside a container:**
  ```bash
  docker top <container_id_or_name>
  ```

* **View changes made to container file system:**
  ```bash
  docker diff <container_id_or_name>
  ```

### 4. Interacting with Running Containers
* **Execute command/shell inside a running container:**
  ```bash
  docker exec -it <container_id_or_name> /bin/bash
  ```

* **Copy files between host and container:**
  ```bash
  # Host to Container
  docker cp /path/on/host <container_id_or_name>:/path/in/container

  # Container to Host
  docker cp <container_id_or_name>:/path/in/container /path/on/host
  ```

### 5. Deleting Containers
* **Remove a stopped container:**
  ```bash
  docker rm <container_id_or_name>
  ```

* **Force remove a running container:**
  ```bash
  docker rm -f <container_id_or_name>
  ```

* **Remove all stopped containers:**
  ```bash
  docker container prune
  ```

---

## 🌐 Bonus: Essential Network & Volume Commands

### Network Operations
```bash
docker network ls                      # List all networks
docker network create <network_name>   # Create a network
docker network connect <net> <container> # Connect container to network
docker network disconnect <net> <container> # Disconnect container
docker network inspect <network_name> # Inspect network details
```

### Volume Operations
```bash
docker volume ls                       # List volumes
docker volume create <volume_name>     # Create a volume
docker volume inspect <volume_name>    # Inspect volume details
docker volume prune                    # Remove unused volumes
```

---
*Created as a handy reference sheet for everyday Docker operations.*
