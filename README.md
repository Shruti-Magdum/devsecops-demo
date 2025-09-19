DevSecOps CI/CD Pipeline Demo
📌 Overview

This project demonstrates a DevSecOps pipeline using Jenkins, Docker, and Trivy. The pipeline:

Pulls code from GitHub.

Builds a Docker image.

Scans the image with Trivy for vulnerabilities (fails on HIGH/CRITICAL).

Optionally pushes the image to Docker Hub.

This setup is lightweight and suitable for local machines and cloud environments.

🏗 Architecture
GitHub (repo + Jenkinsfile)
        |
        v
Jenkins (Pipeline)
        |
   +----+-------------------+
   |                        |
   v                        v
Docker Build             Trivy Scan
   |                        |
   +-----> [Fail if HIGH/CRITICAL]
   |
   v
Docker Hub (optional push)

🗂 Project Structure
devsecops-demo/
├─ server.js        # Node.js app
├─ package.json     # Node dependencies
├─ Dockerfile       # Docker build instructions
├─ Jenkinsfile      # CI/CD pipeline stages
└─ README.md        # Project documentation

⚙️ Local Setup

Clone the repo:

git clone <your-repo-url>
cd devsecops-demo


Test Docker build locally:

docker build -t devsecops-demo:local .
docker run --rm -p 3000:3000 devsecops-demo:local
curl http://localhost:3000


Test Trivy scan locally:

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  ghcr.io/aquasecurity/trivy:latest image --severity HIGH,CRITICAL devsecops-demo:local

🖥 Jenkins Setup

Start Jenkins (local Docker recommended):

docker volume create jenkins_home
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts


Open Jenkins at http://localhost:8080 and install suggested plugins.

Create a Pipeline Job using your GitHub repo.

Jenkins will run pipeline stages:

Checkout code

Build Docker image

Trivy scan

Optional: Push Docker image to Docker Hub

✅ Demo / Usage

Push a commit to GitHub → Jenkins automatically triggers.

Jenkins builds the Docker image.

Trivy scans for vulnerabilities.

If the scan passes, the Docker image can be pushed to Docker Hub.

Screenshot suggestion:

Jenkins pipeline run

Trivy scan output

Docker image in Docker Hub (optional)

📌 Resume Bullet Example

“Implemented CI/CD pipeline using Jenkins and Docker; integrated container vulnerability scanning with Trivy to automatically fail builds on HIGH/CRITICAL vulnerabilities.”

⚠ Troubleshooting

docker: command not found → Install Docker CLI inside Jenkins container.

Pipeline fails on low RAM → reduce concurrency or add swap.

Trivy permission errors → ensure /var/run/docker.sock is mounted.