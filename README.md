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
<img width="1897" height="1131" alt="Screenshot 2026-02-24 175044" src="https://github.com/user-attachments/assets/acdc4222-e737-4f51-8426-16c6003f881a" />
<img width="1916" height="1126" alt="Screenshot 2026-02-24 175100" src="https://github.com/user-attachments/assets/6cbf218d-c005-475f-95b2-cb5e6d6c4660" />
<img width="1919" height="1076" alt="Screenshot 2026-02-24 180558" src="https://github.com/user-attachments/assets/e4dce5b8-9ad5-487c-b973-31b1124751b0" />
<img width="1909" height="1078" alt="Screenshot 2026-02-24 180621" src="https://github.com/user-attachments/assets/ccb591d1-eb92-4bf8-90be-b1a3ad904e74" />
<img width="1901" height="1084" alt="Screenshot 2026-02-24 180636" src="https://github.com/user-attachments/assets/e3535467-c4c4-431f-aba3-dc6b1c01f104" />





