# 🚀 End-to-End CI/CD Pipeline with Jenkins, SonarQube, Docker & AWS

![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?logo=docker&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-Code%20Quality-4E9BCD?logo=sonarqube&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20ALB-FF9900?logo=amazonaws&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-Web%20Server-009639?logo=nginx&logoColor=white)

## 📌 Project Overview

This project demonstrates an end-to-end CI/CD pipeline using *GitHub, Jenkins, SonarQube, Docker, and AWS EC2*.

Whenever code is pushed to GitHub, Jenkins automatically triggers the build process. The project supports two Jenkins configuration styles and two pipeline variants:

| Jenkins Configuration | Description |
|---|---|
| *Freestyle Project* | Configured entirely through the Jenkins GUI |
| *Pipeline Project* | Configured using a Jenkinsfile |

| Jenkinsfile | Stages |
|---|---|
| *Without SonarQube* | Checkout → Transfer files → Docker build → Docker deploy |
| *With SonarQube* | Checkout → SonarQube analysis → Quality check → Transfer files → Docker build → Docker deploy |

---

## 🏗️ Architecture


Developer
   │
   ▼
GitHub Repository
   │
   ▼
GitHub Webhook
   │
   ▼
Jenkins ───────────────► SonarQube (Code Analysis)
   │
   ▼
Copy Application Files
   │
   ├──────────────► Docker Server 1 ──► Docker Container
   │
   └──────────────► Docker Server 2 ──► Docker Container
                              │
                              ▼
               AWS Application Load Balancer
                              │
                              ▼
                          End Users


---

## 🛠️ Technologies Used

- AWS EC2
- Jenkins (Freestyle & Pipeline Projects)
- GitHub & GitHub Webhooks
- SonarQube Community Edition
- Docker
- Ubuntu Linux
- Git
- Nginx

---

## ⚙️ Jenkins Project Configurations

### 1. Jenkins Freestyle Project

Configured using the Jenkins GUI, including:

- GitHub repository integration
- GitHub webhook trigger
- Git checkout
- SonarQube build step (optional)
- SSH file transfer to Docker server
- Remote shell execution
- Docker image build
- Docker container deployment

*Freestyle Workflow*


GitHub Push
   │
   ▼
GitHub Webhook
   │
   ▼
Jenkins Freestyle Job
   │
   ▼
Checkout Code
   │
   ▼
SonarQube Analysis (Optional)
   │
   ▼
Copy Files to Docker Server 1 & Docker Server 2
   │
   ▼
Docker Build
   │
   ▼
Docker Run
   │
   ▼
AWS Application Load Balancer
   │
   ▼
Application Live


### 2. Jenkins Pipeline Project

Uses a Jenkinsfile for automation. Two pipeline versions are available.

*Jenkinsfile Without SonarQube*


Checkout Code
   │
   ▼
Transfer Files to Docker Server 1 & Docker Server 2
   │
   ▼
Docker Build
   │
   ▼
Docker Deploy
   │
   ▼
AWS Application Load Balancer


*Jenkinsfile With SonarQube*


Checkout Code
   │
   ▼
SonarQube Analysis
   │
   ▼
Quality Check
   │
   ▼
Transfer Files to Docker Server 1 & Docker Server 2
   │
   ▼
Docker Build
   │
   ▼
Docker Deploy
   │
   ▼
AWS Application Load Balancer


---

## 📂 Repository Structure


.
├── Jenkinsfile              # Pipeline without SonarQube
├── Jenkinsfile-sonarqube    # Pipeline with SonarQube static code analysis
├── Dockerfile
├── README.md
├── fruitkha-1.0.0/
├── assets/
└── images/


---

## 🧭 Project Workflow

### Step 1 — Launch AWS EC2 Instances

| Instance | Purpose |
|---|---|
| Jenkins | CI/CD automation |
| SonarQube | Code quality analysis |
| Docker Server 1 | Application deployment |
| Docker Server 2 | Application deployment |

### Step 2 — Configure Jenkins

- Install Jenkins
- Create Freestyle project
- Create Pipeline project
- Connect GitHub repository
- Configure GitHub credentials
- Enable GitHub webhook trigger

### Step 3 — Configure SonarQube

- Install Java
- Download SonarQube Community Edition
- Start SonarQube
- Create project
- Generate analysis token
- Configure SonarScanner

### Step 4 — Integrate SonarQube with Jenkins

- Install SonarQube Scanner plugin
- Configure SonarQube server
- Add Sonar token
- Configure SonarScanner tool
- Add Sonar build step

### Step 5 — Configure Docker Server

- Install Docker Engine
- Enable Docker service
- Configure SSH access
- Generate Jenkins SSH key
- Copy public key to Docker server

### Step 6 — Configure Remote Deployment

- Install Jenkins SSH2 Easy plugin
- Configure server group, remote server, and SSH authentication

### Step 7 — Create Dockerfile

dockerfile
FROM nginx
COPY . /usr/share/nginx/html/


### Step 8 — Transfer Project Files

Jenkins copies application files to the Docker server using SCP:

bash
scp -r ./fruitkha-1.0.0 ubuntu@<DOCKER_SERVER_IP>:~/website/


### Step 9 — Build Docker Image

bash
docker build -t mywebsite .


### Step 10 — Run Docker Container

bash
docker run -d -p 8085:80 --name fruitkha-website mywebsite


### Step 11 — Configure AWS Application Load Balancer (ALB)

Create and configure an Application Load Balancer to distribute incoming traffic between both Docker servers:

- Create an Application Load Balancer in AWS
- Select Internet-facing load balancer
- Configure availability zones and subnets
- Create target group for Docker servers
- Register Docker Server 1 and Docker Server 2 as targets
- Configure health check settings
- Add listener (HTTP/HTTPS)
- Forward traffic from load balancer to target group
- Verify target health status

*Traffic Flow*


User Request
   │
   ▼
AWS Application Load Balancer
   │
   ├──────────────► Docker Server 1 ──► Docker Container
   │
   └──────────────► Docker Server 2 ──► Docker Container


The Application Load Balancer distributes user traffic across both Docker servers, improving application availability and scalability.

---

## 🔍 SonarQube Analysis

The SonarQube-enabled pipeline performs:

- Static code analysis
- Bug detection
- Vulnerability detection
- Code smell detection
- Security hotspot analysis
- Code quality reporting

---

## ✅ Features

- GitHub webhook integration
- Jenkins Freestyle automation
- Jenkins Pipeline automation
- Two Jenkinsfiles (with and without SonarQube)
- SonarQube static analysis
- Remote deployment using SSH
- Docker image build
- Docker container deployment
- Continuous Integration
- Continuous Delivery

---

## 📸 Screenshots

> Add screenshots for the following:

- AWS EC2 instances
- Jenkins Freestyle job
- Jenkins Pipeline job
- Jenkins build success
- SonarQube dashboard
- Docker container
- Running website
- GitHub repository
- Pipeline build logs

---

## 🔮 Future Improvements

- Docker Hub integration
- Kubernetes deployment
- ArgoCD
- Terraform
- Prometheus monitoring
- Grafana dashboard
- Email notifications
- Slack notifications

---

## 🎓 Learning Outcomes

- Jenkins CI/CD
- Jenkins Freestyle and Pipeline projects
- SonarQube integration
- Docker deployment
- AWS EC2
- GitHub webhooks
- SSH automation
- Linux administration
- DevOps best practices

---

## 👤 Author

*Harshvardhan Sutar*
DevOps Engineer
