# Docker-project

A containerized Node.js application demonstrating production-ready Docker practices and DevOps workflows.

---


## Tech Stack

- **Runtime**: Node.js
- **Containerization**: Docker
- **Application**: Server.js
- **Package Management**: npm
- **Version Control**: Git/GitHub

---

## Project Structure
```
docker-project-1/
├── Dockerfile              # Container image definition
├── server.js               # Node.js application entry point
├── package.json            # Node.js dependencies and scripts
├── package-lock.json       # Locked dependency versions
├── .dockerignore           # Files excluded from Docker build context
```

**Key Files:**
- `Dockerfile`: Defines the container image, dependencies, and runtime configuration
- `server.js`: Main application logic and HTTP server
- `package.json`: Defines Node.js dependencies, scripts, and project metadata
- `.dockerignore`: Excludes `node_modules/`, logs, and other unnecessary files from image builds

---

## Prerequisites

- **Docker**: Version 20.10+ ([Install Docker](https://docs.docker.com/get-docker/))
- **Node.js**: Version 14+ (for local development only)
- **npm**: Version 6+ (comes with Node.js)
- **Git**: For cloning the repository

Verify installations:
```bash
docker --version
node --version
npm --version
```

---

## Quickstart (Local)

Run the application directly on your host machine:
```bash
# Clone the repository
git clone https://github.com/KharelSandip/docker-project-1.git
cd docker-project-1

# Install dependencies
npm install

# Start the application
npm start
```

The application will start on the configured port (default: 3000).

---

## Quickstart (Docker)

### Build the Image
```bash
# Build the Docker image
docker build -t docker-project-1:latest .

# Verify the image was created
docker images | grep docker-project-1
```

### Run the Container
```bash
# Run container with port mapping
docker run -d \
  --name docker-project-1-app \
  -p 3000:3000 \
  docker-project-1:latest

# Check container status
docker ps
```

### Verify It Works
```bash
# Test the application (if "/" endpoint is implemented)
curl http://localhost:3000

# Test health endpoint (if "/health" endpoint is implemented)
curl http://localhost:3000/health
```

### Stop and Remove
```bash
# Stop the container
docker stop docker-project-1-app

# Remove the container
docker rm docker-project-1-app
```

---

## Configuration

### Environment Variables

| Name | Default | Description |
|------|---------|-------------|
| `PORT` | `3000` | HTTP port the application listens on |
| `NODE_ENV` | `production` | Node.js environment (development/production) |
| `LOG_LEVEL` | `info` | Application logging verbosity |

**Example with custom environment variables:**
```bash
docker run -d \
  --name docker-project-1-app \
  -p 8080:8080 \
  -e PORT=8080 \
  -e NODE_ENV=development \
  -e LOG_LEVEL=debug \
  docker-project-1:latest
```

### Ports

- **Default Application Port**: `3000`
- **Container Port**: Matches `PORT` environment variable
- **Host Port**: Configure via `-p` flag (e.g., `-p 8080:3000`)

---

## Troubleshooting

### 1. Port Already in Use

**Error**: `bind: address already in use`

**Solution**:
```bash
# Find process using the port
lsof -i :3000  # macOS/Linux
netstat -ano | findstr :3000  # Windows

# Kill the process or use a different port
docker run -p 8080:3000 docker-project-1:latest
```

### 2. `node_modules` Missing (Local)

**Error**: `Cannot find module 'express'`

**Solution**:
```bash
# Remove existing modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### 3. Docker Build Cache Issues

**Problem**: Changes not reflected in new build

**Solution**:
```bash
# Build without cache
docker build --no-cache -t docker-project-1:latest .
```

### 4. Permission Denied (Docker Socket)

**Error**: `permission denied while trying to connect to the Docker daemon socket`

**Solution**:
```bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER
newgrp docker

# Or run with sudo (not recommended for production)
sudo docker build -t docker-project-1:latest .
```

### 5. Container Exits Immediately

**Problem**: `docker ps` shows no running containers

**Solution**:
```bash
# Check container logs
docker logs docker-project-1-app

# Run container in foreground to see output
docker run --rm -p 3000:3000 docker-project-1:latest

# Check if application has syntax errors
npm start
```

### 6. Image Build Fails - `COPY` Error

**Error**: `COPY failed: file not found`

**Solution**:
```bash
# Verify files exist
ls -la

# Check .dockerignore isn't excluding required files
cat .dockerignore

# Ensure you're in the correct directory
pwd
```

### 7. Cannot Access Application on `localhost`

**Problem**: `curl: (7) Failed to connect to localhost port 3000`

**Solution**:
```bash
# Verify container is running
docker ps

# Check port mapping
docker port docker-project-1-app

# Try 127.0.0.1 instead of localhost
curl http://127.0.0.1:3000

# On macOS with Docker Desktop, use host.docker.internal
```

### 8. Old Image Still Running After Rebuild

**Problem**: Changes not visible after rebuilding

**Solution**:
```bash
# Stop and remove old container
docker stop docker-project-1-app
docker rm docker-project-1-app

# Remove old image
docker rmi docker-project-1:latest

# Rebuild and run
docker build -t docker-project-1:latest .
docker run -d --name docker-project-1-app -p 3000:3000 docker-project-1:latest
```

### 9. High Memory Usage

**Problem**: Container consuming excessive memory

**Solution**:
```bash
# Run with memory limits
docker run -d \
  --name docker-project-1-app \
  -p 3000:3000 \
  --memory="512m" \
  --memory-swap="512m" \
  docker-project-1:latest

# Monitor resource usage
docker stats docker-project-1-app
```

### 10. `npm install` Fails During Build

**Error**: Network timeout or package resolution errors

**Solution**:
```bash
# Use npm ci for faster, more reliable installs
# Modify Dockerfile: RUN npm ci --only=production

# Or clear npm cache locally
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

---

## Next Improvements


- **Multi-stage Docker Build**: Separate build and runtime stages to reduce image size by 50-70%
- **Non-root User**: Run container as unprivileged user for improved security posture
- **Health Checks**: Add `HEALTHCHECK` directive in Dockerfile for container orchestration
- **Docker Compose**: Multi-container setup with databases, caching layers, and service dependencies
- **CI/CD Pipeline**: GitHub Actions workflow for automated testing, building, and Docker Hub publishing
- **Image Scanning**: Integrate Trivy/Snyk for vulnerability detection in container images
- **Kubernetes Manifests**: Deployment, Service, and Ingress configs for cloud-native deployments
- **Observability**: Integrate Prometheus metrics and structured logging (JSON output)
- **Resource Limits**: Define CPU and memory constraints in Dockerfile and orchestration configs
- **Semantic Versioning**: Automated image tagging based on Git tags/releases
- **Registry Management**: Push to private registry with image signing and SBOM generation

---

