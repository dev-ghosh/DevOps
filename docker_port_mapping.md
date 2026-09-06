# Docker Port Mapping: A Comprehensive Guide

Port mapping (also known as port forwarding) in Docker is a crucial concept that allows external traffic from your host machine or network to reach containerized services running inside an isolated network namespace.

---

## 1. Core Concepts

By default, when a Docker container runs, it operates in an isolated network environment. Even if an application inside a container listens on a port (e.g., port `8000`), that port is **not exposed** to the outside world or the host machine unless explicitly mapped.

* **Host Port:** The port on your local machine or server that external requests hit.
* **Container Port:** The internal port on which the service inside the container is actively listening.

---

## 2. Syntax & Basic Command

To map ports, use the `-p` or `--publish` flag with `docker run`:

```bash
docker run -p <HOST_PORT>:<CONTAINER_PORT> <IMAGE_NAME>
```

### Examples of Port Mapping Syntax

| Syntax / Command | Description |
| :--- | :--- |
| `-p 8080:80` | Maps host port `8080` to container port `80`. |
| `-p 80` | Maps container port `80` to a random available host port. |
| `-p 127.0.0.1:8080:80` | Binds host port `8080` specifically to `localhost` (127.0.0.1). |
| `-p 8080:80/udp` | Maps host UDP port `8080` to container UDP port `80`. |

---

## 3. Step-by-Step Practical Example

Let's walk through an example where we run an **Nginx Web Server** inside Docker and map its internal port to our local host machine.

### Step 1: Run the Nginx Container with Port Mapping

Run the following command in your terminal:

```bash
docker run -d --name my-web-server -p 8080:80 nginx
```

**Explanation of Flags:**
* `-d`: Runs the container in **detached mode** (in the background).
* `--name my-web-server`: Assigns a friendly name to the container.
* `-p 8080:80`: Maps port `8080` on the **host machine** to port `80` (the default Nginx HTTP port) inside the **container**.
* `nginx`: The official Nginx Docker image.

---

### Step 2: Verify Port Mapping

Check running containers to confirm the mapping status:

```bash
docker ps
```

**Sample Output:**
```text
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
a1b2c3d4e5f6   nginx     "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds    0.0.0.0:8080->80/tcp   my-web-server
```

Notice `0.0.0.0:8080->80/tcp`, confirming traffic on host port `8080` routes to port `80` in the container.

---

### Step 3: Test Access

Open your web browser or use `curl` to access the application via the host port:

```bash
curl http://localhost:8080
```

**Expected Result:**
You will receive the standard Nginx welcome HTML response:
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```

---

## 4. Docker Compose Example

Port mapping can also be defined declaratively using `docker-compose.yml`:

```yaml
version: '3.8'

services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

To run it:
```bash
docker compose up -d
```

---

## 5. Summary & Best Practices

1. **Host Conflicts:** Avoid binding multiple containers to the same host port (e.g., `-p 8080:80` and `-p 8080:3000` will conflict).
2. **Security:** By default, `-p 8080:80` binds to `0.0.0.0`, exposing it to all network interfaces. Use `-p 127.0.0.1:8080:80` for local testing to enhance security.
3. **Container-to-Container Communication:** Containers attached to the same Docker network do not require port mapping to speak to each other; they can communicate directly using internal ports and container names.
