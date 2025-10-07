# Docker Guide for Contributors

A practical guide to using Docker for AWSCCPUP DevTeam projects. This guide covers the basics you need to containerize applications and work with Docker in development.

---

## What is Docker?

Docker is a platform that packages applications and their dependencies into containers. Containers are lightweight, portable, and ensure your application runs the same way on any machine—whether it's your laptop, a teammate's computer, or a production server.

**Why we use Docker:**
- Consistency across different environments
- Easy setup for new team members
- No "it works on my machine" problems
- Simplifies deployment to cloud platforms like AWS

---

## Prerequisites

Make sure you have Docker Desktop installed. If not, follow the [Environment Setup Guide](./environment-setup.md).

**Verify Docker is installed:**
```bash
docker --version
```

You should see something like `Docker version 20.x.x` or later.

---

## Core Docker Concepts

**Image:** A blueprint for your application. Contains your code, dependencies, and configuration.

**Container:** A running instance of an image. You can have multiple containers from the same image.

**Dockerfile:** A text file with instructions to build a Docker image.

**Docker Compose:** A tool for running multi-container applications (like a web app with a database).

---

## Basic Docker Commands

Here are the essential commands you'll use regularly:

| Command | Purpose |
|---------|---------|
| `docker --version` | Check Docker version |
| `docker images` | List all images on your machine |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped ones) |
| `docker build -t <name> .` | Build an image from a Dockerfile |
| `docker run <image>` | Run a container from an image |
| `docker stop <container-id>` | Stop a running container |
| `docker rm <container-id>` | Remove a stopped container |
| `docker rmi <image-id>` | Remove an image |
| `docker logs <container-id>` | View logs from a container |
| `docker exec -it <container-id> bash` | Access a running container's shell |

---

## Creating a Dockerfile

A Dockerfile defines how to build your application's image.

### Example: Python Flask Application

```dockerfile
# Use official Python runtime as base image
FROM python:3.11-slim

# Set working directory in container
WORKDIR /app

# Copy requirements file
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Expose port the app runs on
EXPOSE 5000

# Command to run the application
CMD ["python", "app.py"]
```

### Example: Node.js Application

```dockerfile
# Use official Node.js runtime as base image
FROM node:18-alpine

# Set working directory in container
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Expose port the app runs on
EXPOSE 3000

# Command to run the application
CMD ["npm", "start"]
```

---

## Building and Running Your First Container

### Step 1: Create a Dockerfile

Create a file named `Dockerfile` (no extension) in your project root.

### Step 2: Build the Image

```bash
docker build -t my-app .
```

- `-t my-app` names your image "my-app"
- `.` tells Docker to look for the Dockerfile in the current directory

### Step 3: Run the Container

```bash
docker run -p 3000:3000 my-app
```

- `-p 3000:3000` maps port 3000 on your machine to port 3000 in the container
- `my-app` is the image name

### Step 4: Access Your Application

Open your browser and go to `http://localhost:3000`

### Step 5: Stop the Container

Press `Ctrl+C` in the terminal, or in a new terminal:

```bash
docker ps
docker stop <container-id>
```

---

## Docker Compose

Docker Compose lets you define and run multi-container applications using a YAML file.

### Example: Web App with Database

Create a file named `docker-compose.yml`:

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
    depends_on:
      - db
    volumes:
      - .:/app
      - /app/node_modules

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Docker Compose Commands

| Command | Purpose |
|---------|---------|
| `docker-compose up` | Start all services |
| `docker-compose up -d` | Start services in detached mode (background) |
| `docker-compose down` | Stop and remove all services |
| `docker-compose logs` | View logs from all services |
| `docker-compose ps` | List running services |
| `docker-compose exec <service> bash` | Access a service's shell |

### Running with Docker Compose

```bash
# Start all services
docker-compose up

# Start in background
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# Restart a specific service
docker-compose restart web
```

---

## Common Dockerfile Instructions

### FROM
Specifies the base image.
```dockerfile
FROM python:3.11-slim
FROM node:18-alpine
```

### WORKDIR
Sets the working directory inside the container.
```dockerfile
WORKDIR /app
```

### COPY
Copies files from your machine to the container.
```dockerfile
COPY requirements.txt .
COPY . .
```

### RUN
Executes commands during the build process.
```dockerfile
RUN pip install -r requirements.txt
RUN npm install
```

### EXPOSE
Documents which port the container listens on.
```dockerfile
EXPOSE 3000
```

### CMD
Specifies the command to run when the container starts.
```dockerfile
CMD ["python", "app.py"]
CMD ["npm", "start"]
```

### ENV
Sets environment variables.
```dockerfile
ENV NODE_ENV=production
ENV PORT=3000
```

---

## Best Practices

**Use official base images:**
```dockerfile
FROM python:3.11-slim
FROM node:18-alpine
```

**Use .dockerignore:**
Create a `.dockerignore` file to exclude unnecessary files from your image:
```
node_modules
.git
.env
*.log
__pycache__
.pytest_cache
```

**Keep images small:**
- Use alpine-based images when possible
- Combine RUN commands to reduce layers
- Remove unnecessary dependencies

**Don't run as root:**
```dockerfile
RUN useradd -m appuser
USER appuser
```

**Use multi-stage builds for production:**
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm install --production
CMD ["node", "dist/server.js"]
```

---

## Working with Volumes

Volumes persist data between container restarts and allow you to share files between your machine and containers.

**Mount a volume:**
```bash
docker run -v $(pwd):/app -p 3000:3000 my-app
```

**In docker-compose.yml:**
```yaml
services:
  web:
    volumes:
      - .:/app
      - /app/node_modules
```

This setup:
- Mounts your current directory to `/app` in the container
- Excludes `node_modules` to avoid conflicts

---

## Environment Variables

**Pass environment variables when running:**
```bash
docker run -e DATABASE_URL=postgresql://localhost/mydb my-app
```

**Use an .env file with docker-compose:**

Create a `.env` file:
```
DATABASE_URL=postgresql://user:password@db:5432/mydb
SECRET_KEY=your-secret-key
```

Reference it in `docker-compose.yml`:
```yaml
services:
  web:
    env_file:
      - .env
```

---

## Debugging Containers

**View container logs:**
```bash
docker logs <container-id>
docker logs -f <container-id>  # Follow logs in real-time
```

**Access a running container:**
```bash
docker exec -it <container-id> bash
# or for alpine images
docker exec -it <container-id> sh
```

**Inspect a container:**
```bash
docker inspect <container-id>
```

**Check resource usage:**
```bash
docker stats
```

---

## Cleaning Up

**Remove stopped containers:**
```bash
docker container prune
```

**Remove unused images:**
```bash
docker image prune
```

**Remove everything (containers, images, volumes):**
```bash
docker system prune -a
```

**Remove a specific container:**
```bash
docker rm <container-id>
```

**Remove a specific image:**
```bash
docker rmi <image-id>
```

---

## Common Issues and Solutions

**Port already in use:**
```bash
# Use a different port
docker run -p 3001:3000 my-app
```

**Container exits immediately:**
```bash
# Check logs to see what went wrong
docker logs <container-id>
```

**Changes not reflecting:**
```bash
# Rebuild the image
docker build -t my-app .
# Or with docker-compose
docker-compose up --build
```

**Permission denied errors:**
- Make sure Docker Desktop is running
- On Linux, add your user to the docker group:
```bash
sudo usermod -aG docker $USER
```
Then log out and back in.

**Container can't connect to database:**
- Make sure services are in the same Docker network
- Use service names (not localhost) in connection strings
- Check that the database is ready before the app starts

---

## Deploying to AWS

Docker containers can be deployed to AWS using services like:

**AWS ECS (Elastic Container Service):**
- Managed container orchestration
- Good for production workloads

**AWS App Runner:**
- Simplest option for containerized web apps
- Automatic scaling and deployment

**AWS Lambda with Container Images:**
- Serverless container execution
- Good for event-driven workloads

We'll cover deployment in separate guides specific to each project.

---

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/) - Public container registry
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Cheat Sheet](https://dockerlabs.collabnix.com/docker/cheatsheet/)

---

## Practice Exercise

Try containerizing a simple application:

1. Create a simple Python or Node.js app
2. Write a Dockerfile
3. Build the image
4. Run the container
5. Access the app in your browser
6. Stop and remove the container

If you need help, reach out to **JP Curada** or post in the DevTeam channel.

---

**Now you're ready to work with Docker. Let's containerize our projects.**