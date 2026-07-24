🚀 FreshShop Master - CI/CD Pipeline with Jenkins, SonarQube, Docker & AWS ALB

A complete End-to-End DevOps CI/CD Project demonstrating how to automate the deployment of a static HTML/CSS website using Jenkins (Freestyle & Pipeline Jobs), SonarQube, Docker, GitHub Webhooks, and AWS EC2 with an Application Load Balancer (ALB).

📖 Project Overview

This project automates the complete software delivery lifecycle.

Whenever code is pushed to GitHub:

✅ GitHub Webhook triggers Jenkins
✅ Jenkins checks out the latest code
✅ SonarQube performs static code analysis
✅ Jenkins deploys the application to Docker servers
✅ Docker hosts serve the application
✅ AWS Application Load Balancer distributes traffic across Docker instances
🏗️ Architecture
                    Developer
                        │
                        │ Git Push
                        ▼
                  GitHub Repository
                        │
                        │ Webhook
                        ▼
                  Jenkins Server
             (Freestyle / Pipeline)
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼
 SonarQube Analysis              Deploy Application
        │                               │
        └───────────────┬───────────────┘
                        ▼
                Docker Server 1
                        │
                Docker Server 2
                        │
                        ▼
         AWS Application Load Balancer
                        │
                        ▼
                    End Users
☁️ AWS Infrastructure
Component	Count	Purpose	Recommended Instance
Jenkins	1	CI/CD Orchestration	t2.medium
SonarQube	1	Static Code Analysis	t2.medium (4 GiB RAM)
Docker	2	Application Deployment	t2.medium
ALB	1	Load Balancing	AWS Application Load Balancer

Note: SonarQube requires at least 4 GiB RAM.
A t2.medium instance is recommended. A c7i-flex.large instance also works because it provides 4 GiB memory.

🛠️ Technology Stack
Category	Technology
Source Control	Git & GitHub
CI/CD	Jenkins (Freestyle & Pipeline)
Code Quality	SonarQube
Containerization	Docker
Cloud	AWS EC2
Load Balancing	AWS Application Load Balancer
Operating System	Ubuntu Linux
Frontend	HTML5, CSS3
📁 Project Structure
freshshop-master/
│
├── css/
├── js/
├── images/
├── fonts/
├── index.html
├── Dockerfile
├── Jenkinsfile
└── README.md
📋 Prerequisites

Before starting, make sure you have:

AWS Account
GitHub Account
GitHub Personal Access Token
Ubuntu EC2 Instances
HTML/CSS Website Template
SSH Key Pair (.pem)
Docker Knowledge (Basic)
Jenkins Knowledge (Basic)
🚀 Step 1 – Prepare the Website Source
Download an HTML/CSS template.
Extract the project.
Verify index.html exists.
Create a GitHub repository.
Push the project.
git init

git add .

git commit -m "Initial commit"

git branch -M main

git remote add origin https://github.com/<username>/<repository>.git

git push -u origin main
🚀 Step 2 – Launch AWS Infrastructure

Create the following Ubuntu EC2 instances:

Server	Purpose
Jenkins	CI/CD Server
SonarQube	Code Quality Analysis
Docker Server 1	Application Deployment
Docker Server 2	Application Deployment

Create an Application Load Balancer and attach both Docker servers to the Target Group.

🔐 Required Security Group Ports
Port	Purpose
22	SSH
80	HTTP
443	HTTPS
8080	Jenkins
9000	SonarQube
🔗 Configure GitHub Webhook

Navigate to:

GitHub Repository
    ↓
Settings
    ↓
Webhooks
    ↓
Add Webhook

Payload URL

http://<JENKINS_PUBLIC_IP>:8080/github-webhook/

Content Type

application/json

Events

Push Events

Save the webhook.

⚙️ Create Jenkins Freestyle Job
New Item
      ↓
Freestyle Project

Configure:

Repository URL
Git Credentials
Branch: main
GitHub Hook Trigger for GITScm Polling

Save.

Push code to GitHub to verify automatic triggering.

🔍 Configure SonarQube in Jenkins
Install Plugins

Manage Jenkins → Plugins

Install:

SonarQube Scanner
SSH2 Easy
Configure Sonar Scanner

Manage Jenkins

↓

Tools

↓

SonarQube Scanner

Name:
freshshop-master-sonarscanner

Enable:

Install Automatically
Configure SonarQube Server

Manage Jenkins

↓

System

↓

SonarQube Servers

Name:
freshshop-master-sonar-srv

Server URL:
http://<SONARQUBE_PUBLIC_IP>:9000

Add the SonarQube Token using Secret Text Credentials.

Configure Build Step

Inside the Jenkins project

Build Steps

↓

Execute SonarQube Scanner

Analysis Properties

sonar.projectKey=freshshop-master
🐳 Docker Server Configuration

Install Docker on both Docker servers.

Verify Docker:

docker --version

Enable Docker:

sudo systemctl enable docker

sudo systemctl start docker
🔑 Enable Password Authentication

Edit:

/etc/ssh/sshd_config.d/50-cloud-init.conf

Change:

PasswordAuthentication no

to

PasswordAuthentication yes

Restart SSH:

sudo systemctl restart ssh

Verify:

sshd -T | grep passwordauthentication

Output:

passwordauthentication yes
🔐 Configure Passwordless SSH

On Jenkins Server

sudo su jenkins

ssh-keygen

Copy key to Docker Hosts

ssh-copy-id ubuntu@<DOCKER_SERVER_IP>

Verify

ssh ubuntu@<DOCKER_SERVER_IP>

Ensure the deployment directory belongs to the ubuntu user.

sudo chown -R ubuntu:ubuntu ~/website
📤 Configure Deployment Servers

Manage Jenkins

↓

System

↓

Server Groups Center

Create:

Docker-Servers

Add:

Docker Server 1
Docker Server 2

Configure:

SSH Port: 22
Username: ubuntu
Authentication Credentials
🚀 Build Workflow
Git Push
    │
    ▼
GitHub Webhook
    │
    ▼
Jenkins Build
    │
    ├────────► Checkout Source Code
    │
    ├────────► SonarQube Analysis
    │
    ├────────► SCP Deployment
    │
    ▼
Docker Servers
    │
    ▼
Application Load Balancer
    │
    ▼
Live Website
✅ Verification

After pushing code to GitHub:

Jenkins build starts automatically.
SonarQube analysis completes successfully.
Files are copied to Docker servers.
Docker containers start successfully.
Verify:
docker ps

Access the application using the AWS Application Load Balancer DNS Name.

🔒 Security Best Practices
❌ Never commit .pem files.
❌ Never expose GitHub Tokens.
❌ Never expose SonarQube Tokens.
❌ Never store passwords inside Jenkins jobs.
✅ Use Jenkins Credentials Manager.
✅ Restrict Security Groups to trusted IP addresses.
✅ Use SSH Keys instead of passwords wherever possible.
📚 Learning Outcomes

By completing this project, you will gain hands-on experience with:

Git & GitHub
GitHub Webhooks
Jenkins Freestyle Jobs
Jenkins Declarative Pipelines
SonarQube Integration
Docker Deployment
Passwordless SSH
AWS EC2
AWS Application Load Balancer
End-to-End CI/CD Pipeline
🚀 Future Enhancements
Docker Compose
Amazon ECR
Amazon ECS
Kubernetes (EKS)
Terraform
Ansible
Nginx Reverse Proxy
SSL using Let's Encrypt
Monitoring with Prometheus & Grafana
👩‍💻 Author

Sayali Kadwade

AWS Cloud | DevOps Engineer
