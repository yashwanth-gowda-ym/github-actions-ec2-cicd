
# GitHub Actions CI/CD to AWS EC2

This repository demonstrates a CI/CD pipeline using GitHub Actions to build and deploy a containerized application to AWS EC2.
---
# CI/CD Pipeline: GitHub Actions → AWS EC2

## 1. Overview

This repository demonstrates a CI/CD pipeline that builds and deploys a containerized application from an on-premise development environment to an AWS EC2 instance using **GitHub Actions**.

The primary focus of this solution is **CI/CD automation, security, networking, and documentation**, rather than the application itself. The application is intentionally minimal to keep attention on DevSecOps practices such as pipeline design, secrets management, access control, and operational clarity.

The pipeline automatically triggers on code changes, builds a Docker image, pushes it to a container registry, and deploys it to an EC2 instance over a secure SSH connection.

---

## 2. Assumptions Made

The following assumptions were made while designing this solution:

- Developers work from an **on-premise environment** and push source code to GitHub.
- GitHub serves as the **single source of truth** for both code and CI/CD workflows.
- **GitHub Actions managed runners** are used for CI/CD execution.
- An **AWS EC2 instance is already provisioned** and reachable over the network.
- Docker is **pre-installed and running** on the EC2 instance.
- The EC2 instance allows **SSH access using key-based authentication**.
- A container registry (Docker Hub) is available and reachable from both GitHub Actions and EC2.
- All sensitive data (credentials, SSH keys) is stored securely using **GitHub Secrets**.
- This solution prioritizes **design correctness and documentation clarity** over live execution on AWS.

## 3. Tech Stack

| Layer | Technology |
|------|-----------|
| Source Control | GitHub |
| CI/CD | GitHub Actions |
| Containerization | Docker |
| Container Registry | Docker Hub |
| Compute | AWS EC2 |
| OS | Ubuntu Linux |
| Authentication | SSH + GitHub Secrets |

---

## 4. CI/CD Pipeline Flow

1. A developer working from an on-premise environment pushes code to the `main` branch.
2. GitHub Actions automatically triggers the CI/CD pipeline.
3. The pipeline checks out the source code from GitHub.
4. A Docker image is built using the provided Dockerfile.
5. The image is securely pushed to Docker Hub using stored secrets.
6. GitHub Actions connects to the AWS EC2 instance over SSH.
7. The latest Docker image is pulled and deployed as a running container.
8. The deployment is idempotent, ensuring safe re-runs without failure.
---

## 5. Network Diagram

```text
On-Prem Developer
        |
        v
   GitHub Repository
        |
        v
 GitHub Actions Runner
        |
        v
    Docker Hub
        |
        v
   AWS EC2 Instance
---

## 7. Cost Per Pipeline Run

| Component | Estimated Cost |
|---------|----------------|
| GitHub Actions | Free (public repository) |
| Docker Hub | Free tier |
| AWS EC2 (t2.micro) | ~$0.0116 per hour |
| Data Transfer | Negligible |

Note: The CI/CD pipeline itself incurs no cost under GitHub’s free tier for public repositories.

---

## 8. Security & DevSecOps Considerations

- No credentials are hardcoded in the repository.
- All secrets are stored securely using GitHub Secrets.
- SSH key-based authentication is used for EC2 access.
- Least privilege access is assumed for all credentials.
- Workflow permissions are explicitly controlled via token scopes.

---

## 9. Troubleshooting

- **Pipeline fails at Docker login:** Verify Docker Hub credentials in GitHub Secrets.
- **SSH deployment fails:** Check EC2 security group and SSH key validity.
- **Image pull fails on EC2:** Ensure outbound internet access is available.
