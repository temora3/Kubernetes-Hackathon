# Kubernetes Lab Exercises

## Overview

This project provides hands-on Kubernetes lab exercises designed to enhance your skills in container orchestration, security (RBAC), CI/CD with Jenkins, and working with BuildKit for efficient Docker builds.

It includes:
- **BuildKit Lab**: Learn how to optimize Docker builds using BuildKit.
- **RBAC Lab**: Understand Kubernetes Role-Based Access Control (RBAC) and how to apply them securely.
- **Jenkinsfile**: Example Jenkins pipeline for integrating Kubernetes tasks into your CI/CD workflows.

---

## Prerequisites

- Docker Desktop with Kubernetes enabled (or a Kubernetes cluster)
- kubectl CLI configured
- Jenkins installed (for the Jenkinsfile example)
- Optional: BuildKit CLI installed locally

---

## Installation

1. Clone the repository:
    ```bash
    git clone <your-repo-url>
    cd Kubernetes_Lab
    ```

2. Verify Kubernetes connectivity:
    ```bash
    kubectl get nodes
    ```

3. Optional: Set up BuildKit locally:
    ```bash
    docker buildx create --use
    ```

---

## Usage Guidelines

### BuildKit Lab
- Navigate to:
    ```bash
    cd labs/buildkit
    ```
- Review and apply Kubernetes specs:
    ```bash
    kubectl apply -f specs/buildkitd/buildkitd.yaml
    ```

### RBAC Lab
- Navigate to:
    ```bash
    cd labs/rbac
    ```
- Apply RBAC YAML:
    ```bash
    kubectl apply -f specs/sleep.yaml
    ```

### Jenkins Pipeline
- Review the `Jenkinsfile` and integrate it into your Jenkins pipeline.

---

## Code Documentation

All YAML specs and pipeline scripts are commented for clarity.  
Ensure:
- Consistent indentation and naming conventions (`camelCase` for variables, `lowercase-hyphenated` for resources)
- Each lab includes its own `README.md`, `hints.md`, and `solution.md`

---

## Testing

### Manual Tests
- Use `kubectl get` and `kubectl describe` to validate resource deployments.
- Test RBAC by impersonating users:
    ```bash
    kubectl auth can-i get pods --as user1
    ```

### Automated Test Example (Optional)
You can use tools like `kubetest` or Bash scripts to verify deployments.

### Running Tests
```bash
bash test-scripts/run-all-tests.sh
