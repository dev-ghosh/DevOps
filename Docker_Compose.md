# Docker Compose - Beginner Notes

## What is Docker Compose?

**Docker Compose** is a tool used to define and run **multiple Docker containers as one application**.

Instead of running many `docker run` commands manually, you describe your application's services in a YAML file, usually `compose.yaml` or `docker-compose.yml`.

Then you can start the whole application with:

```bash
docker compose up
```

## Why Do We Need Docker Compose?

Imagine an application with:

```text
Frontend
   +
Backend
   +
PostgreSQL
   +
Redis
```

Without Compose, you would configure each container separately.

With Compose, one YAML file defines the whole application.

## Basic Compose File

```yaml
services:

  backend:
    build: .
    ports:
      - "8000:8000"

  redis:
    image: redis:latest
```

Here, `backend` and `redis` are services.

## Important Concepts

### Services

A service represents a container/application component.

```yaml
services:
  backend:
    image: python:3.12

  redis:
    image: redis
```

### Image

Use an existing Docker image:

```yaml
image: nginx:latest
```

### Build

Build an image using a Dockerfile:

```yaml
build: .
```

### Ports

Map a host port to a container port:

```yaml
ports:
  - "8080:80"
```

This means:

```text
Host:8080 → Container:80
```

### Environment Variables

```yaml
environment:
  DATABASE_URL: postgres://user:password@db:5432/app
```

### Volumes

Volumes allow data to persist:

```yaml
volumes:
  - db_data:/var/lib/postgresql/data
```

### Networks

Compose can create networks so services can communicate:

```yaml
networks:
  app_network:
```

### depends_on

Defines startup dependencies:

```yaml
depends_on:
  - redis
```

It controls startup ordering but does not guarantee that the dependency is fully ready.

# Important Docker Compose Commands

## Start Services

```bash
docker compose up
```

Starts all services.

## Start in Background

```bash
docker compose up -d
```

`-d` means detached mode.

## Build Images

```bash
docker compose build
```

## Build and Start

```bash
docker compose up --build
```

Useful after changing the Dockerfile or dependencies.

## Stop Services

```bash
docker compose stop
```

Stops containers without removing them.

## Stop and Remove Containers

```bash
docker compose down
```

Stops and removes Compose containers and the network.

## Remove Containers and Volumes

```bash
docker compose down -v
```

This also removes named volumes, so database data stored there can be deleted.

## View Running Services

```bash
docker compose ps
```

## View Logs

```bash
docker compose logs
```

For one service:

```bash
docker compose logs backend
```

Follow logs:

```bash
docker compose logs -f
```

## Execute a Command Inside a Container

```bash
docker compose exec backend bash
```

Example:

```bash
docker compose exec backend python --version
```

## Restart Services

```bash
docker compose restart
```

Or:

```bash
docker compose restart backend
```

# Example: FastAPI + Redis

```yaml
services:

  backend:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - redis

  redis:
    image: redis:latest
```

Architecture:

```text
User
 │
 ▼
FastAPI
 │
 ▼
Redis
```

Both components run as separate containers.

# Docker Compose vs Docker

### Docker

Docker manages individual containers and images.

```bash
docker run nginx
```

### Docker Compose

Compose manages multiple related containers.

```bash
docker compose up
```

Think:

```text
Docker
  ↓
Individual Containers

Docker Compose
  ↓
Application made of Multiple Containers
```

# Typical Workflow

```text
Create Dockerfile
       ↓
Create compose.yaml
       ↓
Define services
       ↓
docker compose build
       ↓
docker compose up
       ↓
docker compose ps
       ↓
docker compose logs
       ↓
docker compose down
```

# Docker Compose in AI Projects

AI applications often contain multiple services:

```text
AI Application
│
├── FastAPI Backend
├── PostgreSQL
├── Redis
├── Qdrant
└── Frontend
```

Docker Compose can define all of these services in one configuration file.

# Common Project Structure

```text
my-project/
│
├── Dockerfile
├── compose.yaml
├── .env
├── requirements.txt
└── app/
    └── main.py
```

# Key Takeaways

- Docker Compose manages **multiple containers as one application**.
- Configuration is written in YAML.
- A Compose file defines **services**.
- `image:` uses an existing image.
- `build:` builds an image using a Dockerfile.
- `ports:` maps host ports to container ports.
- `volumes:` provides persistent storage.
- `environment:` defines environment variables.
- `depends_on:` defines service startup dependencies.
- `docker compose up` starts the application.
- `docker compose down` stops and removes Compose containers and the network.
- Compose is especially useful for applications containing a backend, database, cache, frontend, and AI infrastructure.
