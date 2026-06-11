# GitHub Actions CI/CD Project

## Project Overview

This project demonstrates a complete CI/CD (Continuous Integration and Continuous Deployment) pipeline using:

* GitHub Actions
* Docker
* Docker Hub
* AWS EC2
* Nginx

The application is containerized using Docker and automatically deployed to an AWS EC2 instance whenever code is pushed to the `main` branch.

---

## Architecture

Developer → GitHub Repository → GitHub Actions → Docker Hub → AWS EC2 → Docker Container → Web Application

### Workflow

1. Developer pushes code to GitHub.
2. GitHub Actions workflow is triggered automatically.
3. Docker image is built from the source code.
4. The image is pushed to Docker Hub.
5. GitHub Actions connects to the EC2 instance via SSH.
6. The latest Docker image is pulled from Docker Hub.
7. Existing container is stopped and removed.
8. A new container is started with the updated image.
9. Application becomes available through the EC2 Public IP.

---

## Project Structure

```text
githubaction-project/
│
├── index.html
├── Dockerfile
│
└── .github/
    └── workflows/
        └── deploy.yml
```

---

## Prerequisites

Before running this project, ensure you have:

* GitHub Account
* Docker Hub Account
* AWS Account
* AWS EC2 Ubuntu Instance
* SSH Key Pair (.pem file)
* Git Installed
* Docker Installed on EC2

---

## Step 1: Create Application

Create an `index.html` file:

```html
<!DOCTYPE html>
<html>
<head>
    <title>CI/CD Project</title>
</head>
<body>
    <h1>Welcome to CI/CD Pipeline Project</h1>
    <p>Deployed using GitHub Actions, Docker Hub and AWS EC2.</p>
</body>
</html>
```

---

## Step 2: Create Dockerfile

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

---

## Step 3: Launch AWS EC2 Instance

Launch an Ubuntu EC2 instance and configure the following inbound rules:

| Type | Port |
| ---- | ---- |
| SSH  | 22   |
| HTTP | 80   |

---

## Step 4: Install Docker on EC2

Connect to EC2:

```bash
ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP
```

Update packages:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io -y
```

Start Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Verify installation:

```bash
docker --version
```

---

## Step 5: Create Docker Hub Repository

Create a repository in Docker Hub.

Example:

```text
githubaction-project
```

Generate a Docker Hub Access Token and save it for GitHub Secrets.

---

## Step 6: Configure GitHub Secrets

Navigate to:

```text
Repository → Settings → Secrets and Variables → Actions
```

Create the following secrets:

| Secret Name     | Description             |
| --------------- | ----------------------- |
| DOCKER_USERNAME | Docker Hub Username     |
| DOCKER_TOKEN    | Docker Hub Access Token |
| SERVER_HOST     | EC2 Public IP           |
| SERVER_USER     | ubuntu                  |
| SSH_PRIVATE_KEY | Content of PEM file     |

---

## Step 7: GitHub Actions Workflow

Create:

```text
.github/workflows/deploy.yml
```

The workflow performs:

* Source Code Checkout
* Docker Hub Authentication
* Docker Image Build
* Docker Image Push
* EC2 Deployment via SSH

---

## Step 8: Push Code to GitHub

```bash
git init

git add .

git commit -m "Initial Commit"

git branch -M main

git remote add origin <repository-url>

git push -u origin main
```

---

## Deployment Verification

Check running containers on EC2:

```bash
docker ps
```
<img width="1920" height="371" alt="image" src="https://github.com/user-attachments/assets/fae5799a-408b-494b-84de-1acb4ee15d69" />

Expected output:

```text
githubaction-project
```

Check application locally on EC2:

```bash
curl localhost
```

Access the application:

```text
http://<EC2_PUBLIC_IP>
```

---
<img width="1920" height="1020" alt="Screenshot 2026-06-11 094440" src="https://github.com/user-attachments/assets/82c77459-faa0-4005-9f91-47bf5de465c2" />

## CI/CD Benefits

* Automated Deployment
* Faster Release Cycle
* Reduced Manual Effort
* Consistent Deployment Process
* Improved Reliability
* Easy Rollback and Updates

---

## Technologies Used

* GitHub Actions
* Docker
* Docker Hub
* AWS EC2
* Nginx
* HTML
* Linux (Ubuntu)

---

## Testing the CI/CD Pipeline

The CI/CD pipeline can be tested by making changes to the application source code and pushing those changes to the GitHub repository.

### Test Procedure

#### Step 1: Modify the Application

Update the `index.html` file with new content.

Example:

```html
<h1>Testing GitHub Action Successful</h1>
<p>This change was deployed automatically using GitHub Actions.</p>
```
<img width="1153" height="436" alt="image" src="https://github.com/user-attachments/assets/45254451-8477-4061-96df-63f988b04576" />

#### Step 2: Commit the Changes

```bash
git add .
git commit -m "Updated application content"
```
<img width="1920" height="1020" alt="Screenshot 2026-06-11 102954" src="https://github.com/user-attachments/assets/be6d3ef4-2538-4762-86d1-ed407725ad64" />

#### Step 3: Push Changes to GitHub
<img width="1350" height="329" alt="image" src="https://github.com/user-attachments/assets/3c95fc34-95cf-4c47-8406-fb0f52064367" />

```bash
git push origin main
```

#### Step 4: GitHub Actions Trigger
<img width="1920" height="561" alt="image" src="https://github.com/user-attachments/assets/af87b02e-dc52-45d5-8244-05f81747d794" />


Once the code is pushed to the `main` branch, GitHub Actions automatically triggers the CI/CD workflow.

The workflow performs the following tasks:

1. Checks out the latest source code.
2. Authenticates with Docker Hub.
3. Builds a new Docker image.
4. Pushes the image to Docker Hub.
5. Connects to the AWS EC2 instance using SSH.
6. Stops and removes the existing container.
7. Pulls the latest Docker image from Docker Hub.
8. Starts a new container with the updated application.

#### Step 5: Verify Deployment

Navigate to the GitHub repository and open the **Actions** tab.

A successful workflow execution will display all steps with a green check mark.
<img width="1920" height="1020" alt="Screenshot 2026-06-10 121729" src="https://github.com/user-attachments/assets/7827195a-04f8-4a07-8f6b-2a5a57064ad3" />

#### Step 6: Validate Application Update

Open the application in a web browser using the EC2 Public IP address:

```text
http://<EC2_PUBLIC_IP>
```
<img width="1920" height="1020" alt="Screenshot 2026-06-11 102242" src="https://github.com/user-attachments/assets/cd40aff5-a090-4463-b9ca-fc43a572f20b" />

The updated content should be visible, confirming that the deployment was completed successfully.

### Expected Result

Any change pushed to the `main` branch is automatically built, packaged, and deployed to the AWS EC2 instance without requiring manual intervention.

This demonstrates a fully automated CI/CD pipeline using GitHub Actions, Docker Hub, and AWS EC2.

## Author

Achal Padol
GitHub - GitHub.com/achalpadol
LinkedIn - www.linkedin.com/in/achal-padol
DevOps Engineer | AWS | Docker | Linux | GitHub Actions
