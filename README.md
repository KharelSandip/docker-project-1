[![CI Pipeline Docker Project](https://github.com/KharelSandip/github-action-CI-docker/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/KharelSandip/github-action-CI-docker/actions/workflows/main.yml)

# Github-Action-CI

A containerized Node.js application demonstrating Docker best practices, Docker Compose orchestration, and automated CI/CD pipelines with security scanning.

---

## What This Project Demonstrates

This project showcases essential DevOps practices:

- **Docker Containerization**: 
- **Docker Compose**: 
- **GitHub Actions CI**: 
- **Reproducible Builds**: 
- **Container Security Scanning**: 
- **Infrastructure as Code**: 
---

## Project Structure
```
github-action-CI-docker/
├── .github/
│   └── workflows/
│       └── main.yml              # GitHub Actions CI pipeline
├── Dockerfile                  # Container image definition
├── docker-compose.yml          # Multi-container orchestration
├── server.js                   # Node.js application
├── package.json                # Node.js dependencies
├── package-lock.json           # Locked dependency versions
├── .dockerignore               # Build context exclusions
└── .gitignore                  # Version control exclusions
```

**Key Files:**
- `main.yml`: Automates build, test, and security scanning workflows
- `Dockerfile`: Defines container image with Node.js runtime and application code
- `docker-compose.yml`: Configures service deployment with resource limits and networking
- `.dockerignore`: Excludes unnecessary files from image builds

---

## How to Run (Docker Compose)

### Prerequisites
- Docker 20.10+
- Docker Compose 2.0+

### Start the Application
```bash
# Clone the repository
git clone https://github.com/KharelSandip/github-action-CI-docker.git
cd docker-project-1

# Build and start services
docker-compose up --build

# Run in detached mode
docker-compose up --build -d
```

The application runs on **port 3000** by default.

### Stop the Application
```bash
# Stop and remove containers
docker-compose down

# Stop and remove containers, networks, and volumes
docker-compose down -v
```

---

## CI Pipeline Overview

The GitHub Actions workflow (`.github/workflows/ci.yml`) runs automatically on:
- Push to `main` branch
- Pull requests targeting `main`

**Pipeline Steps:**

1. **Checkout Code**: Retrieves repository content
2. **Setup Node.js**: Configures Node.js 20 LTS environment with dependency caching
3. **Install Dependencies**: Runs `npm ci` for reproducible builds
4. **Run Tests**: Executes test suite if `npm test` is configured (continues if tests are not present)
5. **Build Docker Image**: Creates image tagged as `docker-project-1:ci`
6. **Trivy Vulnerability Scan**: Scans image for HIGH and CRITICAL vulnerabilities
7. **Upload Scan Report**: Saves `trivy-report.txt` as artifact

**Artifact Access:**
- Navigate to the Actions tab in GitHub
- Select a workflow run
- Download `trivy-vulnerability-report` from the Artifacts section

---

## Security Scanning

This project uses **Trivy** to scan Docker images for known vulnerabilities.

**Scanning Policy:**
- Targets: HIGH and CRITICAL severity vulnerabilities
- Failure Condition: Pipeline fails if HIGH or CRITICAL vulnerabilities are detected
- Report Format: Table output saved to `trivy-report.txt`

**Why This Matters:**
- Identifies security risks before deployment
- Enforces compliance with security standards
- Provides audit trail for vulnerability management

**Note:** The CI pipeline will fail because vulnerabilities are found. We can adjust the severity threshold in `main.yml` if needed for development workflows.

---

## Why This Is a Short Demo Project

This repository is intentionally scoped as a **portfolio demonstration project** for DevOps skills. The focus is on:

- Clean, production-ready configuration patterns
- Automated CI/CD workflows
- Security scanning integration
- Documentation clarity

This is **not** a full production application. Features like database integration, advanced monitoring, Kubernetes deployments, and multi-environment configurations are intentionally excluded to keep the project focused and maintainable as a learning/portfolio artifact.

---

## Improvements 


- **Kubernetes Manifests**: Add Deployment, Service, ConfigMap, and Ingress configurations
- **Image Registry Integration**: Push images to Docker Hub, GHCR, or private registries with semantic versioning
- **Multi-Stage Builds**: Optimize Dockerfile to reduce final image size by 50-70%
- **Advanced CI Policies**: Branch protection rules, required status checks, automated dependency updates
- **Observability Stack**: Integrate Prometheus metrics, structured logging, and distributed tracing
- **GitOps Workflow**: ArgoCD or FluxCD for declarative continuous deployment


---

This project is licensed by **Sandip Kharel**
