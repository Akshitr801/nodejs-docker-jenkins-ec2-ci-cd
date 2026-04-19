**Node.js Docker Jenkins EC2 CI/CD Pipeline**


**📌 Overview**

This project implements a multi-server CI/CD pipeline where Jenkins automates the build, containerization, and deployment of a Node.js application.

The pipeline builds a Docker image, pushes it to Docker Hub, and deploys it to a remote EC2 instance via SSH.

🏗️ **_Architecture_**
GitHub → Jenkins Server (EC2-1) → Docker Hub → App Server (EC2-2)
GitHub → Source code repository
Jenkins Server (EC2-1) → CI/CD automation
Docker Hub → Image registry
App Server (EC2-2) → Runs deployed container


**🛠️ Tech Stack**Node.js (Express)
Docker
Jenkins
Docker Hub
AWS EC2
SSH


**🎯 Project Objective**
Automate application build process
Containerize application using Docker
Push versioned images to Docker Hub
Deploy application to remote EC2 instance
Implement end-to-end CI/CD pipeline
⚙️ Infrastructure Setup
🔹 Jenkins Server (EC2-1)
Jenkins installed (port 8080)
Docker installed
Git installed
Handles build + push + deployment
🔹 App Server (EC2-2)
Docker installed
Port 3000 open
Receives deployment via SSH


**📂 Repository Structure**
├── app.js              # Node.js application
├── package.json        # Dependencies and scripts
├── Dockerfile          # Containerization setup
├── Jenkinsfile         # CI/CD pipeline definition
├── .dockerignore       # Docker build optimization
├── README.md           # Project documentation
└── screenshots/        # Proof of execution


**⚙️ Jenkins Pipeline Stages**
-Checkout
Pulls code from GitHub repository
-Build Image
Builds Docker image using Dockerfile
-Docker Hub Login
Authenticates using Jenkins credentials
-Push Image
Pushes versioned image to Docker Hub
-Deploy to App Server
Connects via SSH
-Pulls latest image
Stops existing container
-Runs new container on port 3000


**🔄 Deployment Flow**
Code Push → Jenkins Trigger → Build Image → Push to Docker Hub → SSH to App Server → Deploy Container → Application Live


**🧪 Issues Faced & Resolutions**
1. Jenkinsfile Not Found
Fixed script path and ensured file exists in root
2. Docker Credential Not Found
Added docker-creds in Jenkins
3. Docker Login Failed
Replaced password with Docker Hub access token
4. Token Permission Issue
Created token with Read & Write access
5. Docker Auth Cache Error
Cleared Docker config:
sudo -u jenkins docker logout
sudo rm -rf /var/lib/jenkins/.docker
6. SSH Permission Denied
Configured SSH key properly between Jenkins and app server
7. Wrong Application File Name
Renamed app.json → app.js

**✅ Final Outcome**
Fully functional CI/CD pipeline
Docker image versioning using Jenkins build number
Automated deployment to remote EC2 server
Application accessible via:
http://<APP_SERVER_IP>:3000

**🧠 Key Learnings**
CI/CD pipeline design and debugging
Docker authentication and image management
SSH-based remote deployment
Handling real-world DevOps failures
Multi-server architecture setup

**📢 Conclusion**

This project demonstrates a complete CI/CD workflow from code commit to deployment using industry-relevant tools and practices.
