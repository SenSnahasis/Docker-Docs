# 🐳 Docker Cheat Sheet

This README provides a handy reference for essential Docker commands and a simple setup for running MongoDB and Mongo Express using Docker.

---

## 🐳 What is Docker?

**Docker** is an open-source platform designed to help developers build, ship, and run applications in **containers**. It enables you to package applications with all their dependencies into a standardized unit for software development, ensuring consistency across different environments (development, testing, production).

---

## 📦 What is a Docker Image?

A **Docker image** is a lightweight, standalone, and executable package that includes:
- The application code
- Runtime (e.g., Node.js, Python, Java)
- Libraries and dependencies
- Environment variables and configuration

> Think of an image as a **blueprint** or **snapshot** used to create Docker containers.

---

## 📦 What is a Docker Container?

A **Docker container** is a **running instance** of a Docker image. It’s an isolated environment that shares the host OS kernel but runs independently.

- Fast and lightweight
- Starts in seconds
- Can be stopped, restarted, or removed without affecting the image

> You can run multiple containers from the same image.

---

## 🆚 Docker vs Virtual Machine (VM)

| Feature | Docker | Virtual Machine |
|--------|--------|------------------|
| Architecture | Shares host OS kernel | Emulates entire OS (via hypervisor) |
| Performance | Lightweight, fast | Heavier, more resource-intensive |
| Boot Time | Seconds | Minutes |
| Resource Usage | Efficient (uses container engine) | High (requires full OS per VM) |
| Isolation | Process-level | Full OS-level |
| Portability | Highly portable | Less portable due to OS dependency |

---

### 🧠 TL;DR

- Docker **containers** are faster, lighter, and more portable than **VMs**.
- A **Docker image** is the template, and a **container** is the running app based on that image.
- Docker allows you to ship your application **anywhere**, with **confidence** it will work the same way.

---


## 📦 Basic Docker Commands

| Command | Description |
|--------|-------------|
| `docker pull <image_name>` | Pull image from Docker Hub |
| `docker images` | List all downloaded Docker images |
| `docker run <image_name>` | Create and run a container |
| `docker run -it <image_name>` | Run container in interactive terminal mode |
| `docker stop <container_id>` | Stop a running container |
| `docker start <container_id>` | Start an existing (stopped) container |
| `docker ps` | List only running containers |
| `docker ps -a` | List all containers (running + stopped) |
| `docker ps -q` | Get only running container IDs |
| `docker run <image_name>:<version>` | Run a specific image version (e.g., `mysql:8.0`) |
| `docker run --name <container_name> <image_name>` | Assign a custom name to the container |
| `docker run -p <host_port>:<container_port> <image_name>` | Bind local port to container port |
| `docker network ls` | List all Docker networks |
| `docker network create <network_name>` | Create a custom Docker network |
| `docker network rm <network_name>` | Delete a Docker network |
| `docker volume ls` | List all Docker volumes |
| `docker volume prune` | Remove all unused volumes |
| `docker compose -f <file_name> up -d` | Start containers from a Docker Compose file (detached mode) |
| `docker compose -f <file_name> down` | Stop and remove containers defined in the Compose file |
| `docker logs <container_name>   (or <container_id)` | Fetch logs of a container |
| `docker exec -it <container_name> /bin/bash ` | Open shell inside running container |
| `docker build -t <image_name>:<version>` | Build an image from a Dockerfile |


> 📌 **Note:** `docker run` always creates a **new container** while `docker start` is used to run an **existing container**.

---

## 🍃 MongoDB + Mongo Express Setup

Below is a simple two-container setup: one for MongoDB and another for Mongo Express (a web-based MongoDB admin interface).

### 1️⃣ Run MongoDB Container

```bash
docker run -d \
  --name mongo \
  --network mongodb-network \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=secret \
  mongo


docker run -d \
  --name mongo-express \
  --network mongodb-network \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=secret \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:secret@mongo:27017" \
  mongo-express
