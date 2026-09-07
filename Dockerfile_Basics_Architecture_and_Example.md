# Dockerfile: What It Is, Uses, Architecture, and Example

## What is a Dockerfile?

A **Dockerfile** is a text file containing instructions that Docker uses to **build a Docker image**.

It defines the environment required by an application, including:
- Base image/runtime
- Dependencies
- Application code
- Working directory
- Ports
- Startup command

A Dockerfile is normally named exactly:

```text
Dockerfile
```

## Why Do We Use a Dockerfile?

Without a Dockerfile, you may have to manually configure the same environment on every machine:

```text
Install Python
Install dependencies
Copy project
Configure environment
Run application
```

This can lead to:

> "It works on my machine."

A Dockerfile makes the environment **reproducible**. You define it once and Docker can build the same image again.

## Dockerfile vs Image vs Container

```text
Dockerfile
    │
    │ docker build
    ▼
Docker Image
    │
    │ docker run
    ▼
Docker Container
```

### Dockerfile
Instructions for building an image.

### Docker Image
A packaged, read-only template containing the application and its dependencies.

### Docker Container
A running instance of an image.

# Docker Architecture

```text
                 Docker Client
                      │
                docker build
                docker run
                      │
                      ▼
                Docker Engine
                 /          \
                /            \
               ▼              ▼
        Docker Images     Containers
               │              │
               └──────┬───────┘
                      │
                 Docker Registry
                  (e.g. Docker Hub)
```

### Docker Client
The CLI through which you run commands such as:

```bash
docker build
docker run
docker ps
docker stop
```

### Docker Engine
The main Docker service that builds images and creates/runs containers.

### Docker Image
Contains the packaged application environment.

### Container
An isolated running environment created from an image.

### Docker Registry
A place where images are stored and shared.

Examples:
- Docker Hub
- GitHub Container Registry
- Amazon ECR

# Important Dockerfile Instructions

## FROM

Specifies the base image.

```dockerfile
FROM python:3.11-slim
```

## WORKDIR

Sets the working directory inside the container.

```dockerfile
WORKDIR /app
```

## COPY

Copies files from your computer into the image.

```dockerfile
COPY requirements.txt .
COPY . .
```

## RUN

Executes a command while building the image.

```dockerfile
RUN pip install -r requirements.txt
```

## EXPOSE

Documents the port that the application listens on.

```dockerfile
EXPOSE 8000
```

Important: `EXPOSE` does **not** publish the port to your computer. Port mapping is still required:

```bash
docker run -p 8000:8000 my-app
```

## CMD

Specifies the default command that runs when the container starts.

```dockerfile
CMD ["python", "app.py"]
```

# Example: Python Application

Suppose your project contains:

```text
my-project/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

### app.py

```python
print("Hello from Docker!")
```

### requirements.txt

```text
flask
```

### Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

## Building the Image

Run:

```bash
docker build -t my-app .
```

The result is:

```text
Dockerfile
     │
     │ docker build
     ▼
my-app image
```

## Running the Image

```bash
docker run my-app
```

Docker creates a container from the image and runs:

```bash
python app.py
```

Output:

```text
Hello from Docker!
```

# Dockerfile for a Web Application

For a FastAPI application:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build:

```bash
docker build -t my-fastapi-app .
```

Run:

```bash
docker run -p 8000:8000 my-fastapi-app
```

Architecture:

```text
Browser
   │
   │ localhost:8000
   ▼
Host Port 8000
   │
   │ Port Mapping
   ▼
Container Port 8000
   │
   ▼
FastAPI
```

# Dockerfile Layers

Dockerfiles are built in **layers**.

For example:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .
```

Docker can reuse unchanged layers during future builds. This is called the **Docker build cache**.

For example, if you only change `app.py`, Docker may reuse the dependency installation layer instead of installing everything again.

# Dockerfile vs Docker Compose

### Dockerfile

Defines:

> "How should I build my application image?"

### Docker Compose

Defines:

> "How should multiple containers work together?"

For example:

```text
Docker Compose
│
├── FastAPI Container
├── PostgreSQL Container
└── Redis Container
```

The FastAPI container can be built using a Dockerfile.

# When Should You Use a Dockerfile?

Use a Dockerfile when you want to:

- Package an application
- Create reproducible environments
- Deploy applications
- Avoid dependency conflicts
- Share an application environment
- Run the same application locally and in production
- Build CI/CD pipelines

# Dockerfile in an AI Project

Dockerfiles are useful for AI applications such as:

```text
AI Application
│
├── FastAPI
├── LangChain
├── LangGraph
├── RAG
├── Qdrant
└── LLM API
```

A Dockerfile can package the application and its Python dependencies into an image.

Example:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

# Key Takeaways

- A **Dockerfile** contains instructions for building a Docker image.
- A **Docker image** is a packaged template for an application.
- A **container** is a running instance of an image.
- `FROM` selects the base image.
- `WORKDIR` sets the working directory.
- `COPY` copies files into the image.
- `RUN` executes commands during image building.
- `EXPOSE` documents a container port.
- `CMD` specifies the default startup command.
- `docker build` creates an image from a Dockerfile.
- `docker run` creates and starts a container from an image.
- Dockerfiles make application environments reproducible and easier to deploy.
