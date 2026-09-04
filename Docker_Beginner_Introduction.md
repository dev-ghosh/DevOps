# Docker - Beginner Introduction

## What is Docker?

**Docker** is a platform that allows developers to package an application together with everything it needs to run, such as code, libraries, dependencies, and configuration, into a portable unit called a **container**.

The main idea is:

> **Build once, run consistently anywhere.**

Docker helps solve the common problem where an application works on one computer but fails on another because of different versions or dependencies.

## Why Do We Need Docker?

Without Docker:

```text
Developer Computer
Python 3.11
Library Version A
Works

Server
Python 3.9
Library Version B
Doesn't work
```

With Docker:

```text
Application + Dependencies + Runtime
                 ↓
             Container
                 ↓
       Consistent Environment
```

## Important Docker Terms

### 1. Docker Image

A **Docker Image** is a read-only blueprint used to create containers.

It can contain application code, runtime, libraries, dependencies, and configuration.

Example:

```bash
docker pull nginx
```

This downloads the `nginx` image from a container registry.

### 2. Docker Container

A **Container** is a running instance of a Docker image.

```text
Docker Image
     ↓
  Container
```

Example:

```bash
docker run nginx
```

### 3. Docker Hub

**Docker Hub** is a public registry where Docker images can be stored and downloaded.

Example:

```bash
docker pull nginx
```

### 4. Docker Engine

**Docker Engine** is the technology that builds and runs containers.

### 5. Dockerfile

A **Dockerfile** contains instructions for creating a Docker image.

Example:

```dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Build it with:

```bash
docker build -t my-app .
```

### 6. Docker Registry

A **Docker Registry** stores Docker images.

Examples:
- Docker Hub
- GitHub Container Registry
- Amazon ECR

### 7. Docker Volume

A **Docker Volume** provides persistent storage for data that should survive container removal.

Example:

```bash
docker volume create my-data
```

### 8. Port Mapping

Port mapping makes a container port accessible from the host machine.

Example:

```bash
docker run -p 8080:80 nginx
```

This means:

```text
Host Port 8080
      ↓
Container Port 80
```

So `localhost:8080` can access Nginx running inside the container.

### 9. Docker Network

A **Docker Network** allows containers to communicate with each other.

```text
Frontend Container
       ↓
Backend Container
       ↓
Database Container
```

### 10. Docker Compose

**Docker Compose** is used to define and run applications containing multiple services.

For example:

```text
Frontend
Backend
Database
Redis
```

A Compose configuration can define these services and they can be started with:

```bash
docker compose up
```

## Image vs Container

| Docker Image | Docker Container |
|---|---|
| Blueprint | Running instance |
| Template | Actual running environment |
| Used to create containers | Created from an image |
| One image can create many containers | Represents a running instance |

Simple analogy:

```text
Class      → Object
Image      → Container
```

## Basic Docker Workflow

```text
Write Application
       ↓
Create Dockerfile
       ↓
docker build
       ↓
Docker Image
       ↓
docker run
       ↓
Docker Container
       ↓
Application Running
```

## Example: Running Nginx

```bash
docker pull nginx
docker run -p 8080:80 nginx
```

Workflow:

```text
Browser
   │
   │ localhost:8080
   ▼
Host Machine
   │
   │ Port Mapping
   ▼
Container
   │
   │ Port 80
   ▼
Nginx
```

## Common Docker Commands

```bash
docker --version
docker pull nginx
docker run nginx
docker run -p 8080:80 nginx
docker ps
docker ps -a
docker stop <container_id>
docker rm <container_id>
docker images
```

## Docker in Real-World Development

Docker is commonly used for:

- Application deployment
- Development environments
- Microservices
- Databases
- CI/CD pipelines
- Cloud deployments
- Testing
- AI/ML applications

For example, an AI application could contain:

```text
Docker
│
├── FastAPI Backend
├── RAG Application
├── Redis
└── PostgreSQL
```

Each service can run in its own container.

## Key Takeaways

- **Docker** packages and runs applications in containers.
- An **Image** is the blueprint.
- A **Container** is a running instance of an image.
- A **Dockerfile** contains instructions for building an image.
- **Docker Hub** is a popular image registry.
- **Port mapping** connects a host port to a container port.
- **Volumes** provide persistent storage.
- **Networks** allow containers to communicate.
- **Docker Compose** manages multi-container applications.
- Docker helps applications run consistently across different environments.
