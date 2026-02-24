# MEAN Stack Deployment Assignment

**Internship Submission for DevOps Task**

This repository contains my solution for the full-stack MEAN (MongoDB, Express, Angular, Node.js) application deployment assignment. The objective of this project was to containerize the application, orchestrate the services, and establish an automated CI/CD pipeline for cloud deployment.

##  Key Achievements
- **Containerization**: Wrote robust `Dockerfile`s for both the Node.js backend and the Angular frontend. The frontend utilizes a multi-stage build pattern using Nginx for serving optimized production files.
- **Service Orchestration**: Implemented a comprehensive `docker-compose.yml` to run the frontend, backend, MongoDB database, and Nginx reverse proxy together seamlessly within an isolated network.
- **Reverse Proxy**: Configured an Nginx block (`nginx.conf`) to proxy traffic internally. It listens on port 80 and elegantly routes client requests between the compiled static Angular files and the Node.js `/api/` endpoints.
- **Automated CI/CD**: Established a complete CI/CD pipeline using **GitHub Actions**. Upon every push to the `main` branch, the workflow automatically:
  1. Builds the latest Docker images.
  2. Authenticates and pushes the updated images to Docker Hub.
  3. Establishes a secure SSH connection to an Ubuntu Virtual Machine.
  4. Pulls the latest images and deploys the stack using Docker Compose with zero downtime.

## Architecture & Technology Stack
- **Frontend**: Angular 15, served via Nginx.
- **Backend**: Node.js & Express API routing.
- **Database**: MongoDB containerized within the compose network (via `mongo` image).
- **Orchestration**: Docker & Docker Compose.
- **CI/CD**: GitHub Actions.
- **Deployment OS**: Ubuntu Linux VM (AWS EC2 / Azure).

---

## Instructions to Replicate the Deployment

If you are an evaluator checking this repository, here is how the CI/CD pipeline is executed in my environment:

### 1. Prerequisites
- Target an Ubuntu VM with Docker and Docker Compose installed.
- Ensure ports `80` (HTTP) and `22` (SSH) are open.
- On the VM, the `/opt/mean-app` directory contains the stack's `docker-compose.yml` and `nginx.conf`.

### 2. GitHub Secrets Configuration
The automated CI/CD pipeline requires the following secrets to be configured in the repository settings (**Settings > Secrets and variables > Actions**):
- `DOCKER_USERNAME`: Personal Docker Hub credentials.
- `DOCKER_PASSWORD`: Personal Docker Hub access token.
- `VM_HOST`: The public IP address of the target deployment VM.
- `VM_USERNAME`: The SSH user (e.g., `ubuntu`).
- `VM_SSH_KEY`: The contents of the private `.pem` SSH key file.

### 3. Execution Flow
1. Any new code committed and pushed to the `main` branch triggers the `.github/workflows/deploy.yml` workflow.
2. The pipeline builds both the `mean-frontend` and `mean-backend` images and pushes them to the connected Docker Hub registry.
3. The pipeline securely connects to the VM, executes a `docker-compose pull`, and restarts the containers using `docker-compose up -d`.
4. The live application is immediately accessible via `http://<VM_HOST>`.

---

##  Screenshots & Deliverables


1. **Working Application UI**: (Insert screenshot of the app running in the browser on the VM's IP address)
2. **CI/CD Configuration & Success**: (Insert screenshot of the passing GitHub Actions workflow)
3. **Docker Hub Images**: (Insert screenshot of the pushed images in your Docker Hub account)
4. **Nginx Setup / Infrastructure**: (Insert screenshot of the VM terminal running `docker ps` showing the Nginx, Node, Angular, and Mongo containers running)
