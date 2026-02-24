# MEAN Stack DevOps Deployment Task

This repository contains the completed DevOps task for containerizing and deploying a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js).

## 🚀 Deliverables Completed
1. **Dockerized Application**: `Dockerfile`s created for both the frontend (multi-stage Nginx build) and backend (Node.js).
2. **Reverse Proxy**: Nginx configuration (`nginx.conf`) created to route traffic on port `80` to the frontend and backend `/api/` endpoints.
3. **Docker Compose Orchestration**: `docker-compose.yml` set up for running the entire stack, including a MongoDB container.
4. **CI/CD Pipeline**: GitHub Actions workflow (`.github/workflows/deploy.yml`) created to automatically build and push images to Docker Hub, and ssh into a server to deploy.
5. **Code Adjustments**: 
   - Backend DB config points to the `mongo` Docker Compose service instead of `localhost`.
   - Frontend API Service points to the relative `/api/` path to utilize the Nginx reverse proxy.

---

## 🛠️ Step-by-Step Setup and Deployment Instructions

### 1. Repository Setup
1. Create a new repository on your GitHub account.
2. Add the remote origin to your local project:
   ```bash
   git init
   git add .
   git commit -m "Initial commit for MEAN app with Docker and CI/CD"
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   ```
3. Do **not** push it just yet until you have configured GitHub secrets in Step 3.

### 2. Local Testing (Optional but Recommended)
If you have Docker installed locally, you can verify the stack is functioning correctly:
```bash
docker-compose up --build -d
```
Visit `http://localhost:80` in your browser. The app should load and interact with the database successfully.

### 3. CI/CD Configuration (GitHub Actions to Docker Hub)
To make the automated deployment work, you must add secrets to your GitHub repository settings.
1. Go to repository **Settings** -> **Secrets and variables** -> **Actions**.
2. Add the following **New repository secrets**:
   - `DOCKER_USERNAME`: Your Docker Hub username.
   - `DOCKER_PASSWORD`: Your Docker Hub password or access token.
   - `VM_HOST`: The IP address of your Ubuntu VM.
   - `VM_USERNAME`: The SSH username (usually `ubuntu` or `root`).
   - `VM_SSH_KEY`: The private SSH key (`.pem` file contents) used to connect to your VM.

### 4. VM Setup & Deployment
1. Provision a new Ubuntu Virtual Machine on AWS (EC2), Azure, or similar.
2. Ensure Port `80` (HTTP) and `22` (SSH) are open in the VM's security groups/firewall rules.
3. SSH into the server and install Docker and Docker Compose:
   ```bash
   sudo apt update
   sudo apt install docker.io docker-compose -y
   sudo usermod -aG docker $USER
   ```
4. Create the deployment directory on the VM matching the CI/CD script:
   ```bash
   sudo mkdir -p /opt/mean-app
   sudo chown -R $USER:$USER /opt/mean-app
   ```
5. Copy the `docker-compose.yml` and `nginx.conf` files to the `/opt/mean-app` directory on the VM manually for the very first time. You can use tools like `scp` or `nano`. Ensure the `image` names in the VM's `docker-compose.yml` point to the images built on Docker Hub (e.g. `image: YOUR_USERNAME/mean-frontend:latest` instead of `build: ./frontend`).

### 5. Final Execution
1. Push your code to the `main` or `master` branch:
   ```bash
   git push -u origin main
   ```
2. Navigate to the **Actions** tab on GitHub. You will see the workflow running, building the images, pushing them to Docker Hub, and triggering a deployment on your VM.
3. Once completed, visit your VM's public IP address in a web browser `http://<VM_PUBLIC_IP>` to see the working MEAN application!

---

### Additional Screenshots Required for Final Submission
As requested in the task overview, please take screenshots of the following after you execute these steps:
- CI/CD configuration and successful execution (from GitHub Actions tab).
- Docker image build and push process (from GitHub Actions output or Docker Hub dashboard).
- Application deployment and working UI (screenshot of the app loaded via the VM's IP address).
- Nginx setup / Infrastructure details running on the server.
