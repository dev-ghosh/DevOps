# Docker Networks

## What is a Docker Network?

A **Docker network** allows Docker containers to communicate with other containers, the host machine, and external networks.

Example:

```text
Frontend Container
       |
       | HTTP request
       v
Backend Container
```

A Docker network provides the connection between these containers.

---

## Why Do We Need Docker Networks?

Imagine an application with:

- React frontend
- FastAPI backend
- PostgreSQL database

```text
Frontend
   |
   v
Backend
   |
   v
Database
```

Docker networks allow these containers to communicate without exposing every service to the outside world.

---

# Docker Network Types

Docker provides several network drivers:

1. **bridge**
2. **host**
3. **none**
4. **overlay**
5. **macvlan**

The **bridge** network is the most common for normal Docker development.

---

# 1. Bridge Network

The **bridge** network is the default network driver for standalone Docker containers.

Containers connected to the same bridge network can communicate with each other.

### Example

Create a network:

```bash
docker network create my-network
```

Run a container:

```bash
docker run -d --name backend --network my-network nginx
```

Run another container:

```bash
docker run -d --name frontend --network my-network nginx
```

Now:

```text
my-network

frontend -------- backend
```

The containers can communicate using container names.

For example:

```text
http://backend
```

Docker's internal DNS resolves `backend` to the appropriate container.

### Why is this useful?

You don't need to remember the backend container's IP address.

Instead of:

```text
http://172.x.x.x
```

you can use:

```text
http://backend
```

---

# 2. Host Network

With the **host** network, the container shares the host machine's network namespace.

Example:

```bash
docker run --network host nginx
```

Concept:

```text
Host Machine
     |
     +--- Container
          (shares host network)
```

### When is it useful?

- Applications that need high network performance
- Situations where separate container network isolation is not required

**Note:** Host networking behaves differently depending on the operating system and Docker environment, especially on Docker Desktop.

---

# 3. None Network

The **none** network disables networking for the container.

Example:

```bash
docker run --network none nginx
```

Concept:

```text
Container

   X

No network connectivity
```

### When is it useful?

When a container does not need network access and you want strong network isolation.

---

# 4. Overlay Network

An **overlay network** connects containers across multiple Docker hosts.

It is mainly associated with **Docker Swarm** and multi-host container environments.

Concept:

```text
Docker Host 1                 Docker Host 2

Container A  ---------------  Container B
             Overlay Network
```

### When is it useful?

- Multi-host applications
- Docker Swarm
- Distributed container environments

For simple Docker projects, you usually won't need an overlay network.

---

# 5. Macvlan Network

A **macvlan** network gives containers their own MAC addresses and allows them to appear as physical devices on the network.

Concept:

```text
Physical Network
       |
   +---+---+
   |       |
 Host   Container
         Container
```

### When is it useful?

- Legacy applications
- Applications that need to appear directly on a physical network
- Specialized networking requirements

It is less common for typical web application development.

---

# Docker Network Comparison

| Network Type | Main Purpose | Common Usage |
|---|---|---|
| Bridge | Container-to-container communication | Most common |
| Host | Share host networking | Specialized |
| None | No networking | Isolation |
| Overlay | Multi-host communication | Docker Swarm |
| Macvlan | Containers appear on physical network | Specialized |

---

# Docker Compose and Networks

Docker Compose automatically creates a network for services in the same Compose project.

Example:

```yaml
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"

  backend:
    build: ./backend
    ports:
      - "8000:8000"

  database:
    image: postgres
```

Compose creates a network connecting:

```text
frontend
   |
   v
backend
   |
   v
database
```

The services can communicate using their service names.

For example, the backend can connect to PostgreSQL using:

```text
database:5432
```

instead of:

```text
localhost:5432
```

---

# Important: localhost vs Container Name

This is a very important Docker networking concept.

Suppose:

```text
backend container
     |
     v
database container
```

Inside the backend container:

```text
localhost
```

means:

> The backend container itself.

It does **not** mean the database container.

Therefore, use the database service/container name:

```text
database
```

Example:

```text
postgresql://user:password@database:5432/mydb
```

---

# Useful Docker Network Commands

## List Networks

```bash
docker network ls
```

## Create a Network

```bash
docker network create my-network
```

## Inspect a Network

```bash
docker network inspect my-network
```

## Connect a Running Container

```bash
docker network connect my-network container-name
```

## Disconnect a Container

```bash
docker network disconnect my-network container-name
```

## Remove a Network

```bash
docker network rm my-network
```

---

# Port Mapping vs Docker Networking

These are related but different concepts.

## Port Mapping

```bash
docker run -p 8080:80 nginx
```

Means:

```text
Host Port 8080
      |
      v
Container Port 80
```

It allows the host/external users to access the container service.

## Docker Network

```text
Container A
     |
     v
Docker Network
     |
     v
Container B
```

It allows containers to communicate with each other.

---

# Real-World Example

Imagine a production-style application:

```text
                    Internet
                       |
                       v
                   Frontend
                  Port 3000
                       |
                       v
                    Backend
                  Port 8000
                       |
                       v
                   PostgreSQL
                    Port 5432
```

You might expose:

```text
3000 -> Frontend
8000 -> Backend
```

But PostgreSQL may not need to be exposed to the host at all.

All three containers can communicate through an internal Docker network.

---

# Key Takeaways

- A **Docker network** allows containers to communicate.
- **Bridge** is the most common network type for standalone Docker applications.
- **Host** makes the container share the host's network.
- **None** provides network isolation.
- **Overlay** is designed for communication across multiple Docker hosts.
- **Macvlan** makes containers appear as devices on a physical network.
- Docker Compose automatically creates a network for services in a Compose project.
- Containers on the same network can communicate using **container/service names**.
- `localhost` inside a container refers to that container itself.
- **Port mapping (`-p`)** exposes a container service to the host/outside world, while Docker networks enable container-to-container communication.
