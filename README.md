# EventHive DevOps Portfolio Project

This repository demonstrates a production-grade CI/CD pipeline following DevSecOps principles.

## Pipeline Architecture

### 1. Continuous Integration (CI) - `.github/workflows/ci.yml`
**Trigger:** Push to `main`
* **Linting (ESLint):** Enforces code quality and prevents technical debt by catching syntax errors early.
* **Unit Tests:** Ensures business logic (like Auth) remains functional and prevents regressions.
* **SAST & SCA (Trivy):** Scans source code and dependencies for known CVEs (Common Vulnerabilities and Exposures) before building.
* **Docker Build:** Containerizes the application to ensure consistency across environments.
* **Image Scan (Trivy):** Scans the final Docker image for OS-level vulnerabilities (e.g., in Alpine/Node base images).
* **Registry Push:** Pushes the verified, secure image to DockerHub.

### 2. Continuous Deployment (CD) - `.github/workflows/cd.yml`
**Trigger:** Successfully completed CI run.
* **Infrastructure as Code:** Uses Kubernetes manifests (`deployment.yaml`, `service.yaml`) to define state.
* **Ephemeral Environment:** Spins up a **Kind (Kubernetes in Docker)** cluster to simulate a real production environment inside GitHub Actions.
* **Dummy DAST:** Performs a Dynamic Application Security Test against the running container to verify runtime security headers and availability.

## Tech Stack
* **CI/CD:** GitHub Actions
* **Containerization:** Docker
* **Orchestration:** Kubernetes (Kind for testing)
* **Security:** Trivy (Filesystem & Image scanning)
* **Backend:** Node.js / Express