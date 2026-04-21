🚀 End-to-End CI/CD Pipeline on AWS using EKS, ECR & CodePipeline
📌 Project Overview

This project demonstrates a complete end-to-end CI/CD pipeline built on AWS to automate the deployment of a containerized web application.

The application is a production-ready static React build, deployed using modern DevOps practices including Docker, Kubernetes (EKS), and AWS CI/CD services.

🧠 Architecture
GitHub → CodePipeline → CodeBuild → Amazon ECR → Amazon EKS → LoadBalancer (Live App)
---------------------------------------------------------------------------------------------------
⚙️ Tech Stack
🐳 Docker (Containerization)
☸️ Kubernetes (Amazon EKS)
📦 Amazon ECR (Container Registry)
🔄 AWS CodePipeline (CI/CD Orchestration)
🛠 AWS CodeBuild (Build Automation)
📊 Amazon CloudWatch (Monitoring & Logs)
🌐 AWS LoadBalancer (Public Access)
____________________________________________________________________________
🚀 Key Features
✅ Fully automated CI/CD pipeline
✅ Automatic trigger on GitHub push
✅ Docker image build & push to ECR
✅ Kubernetes deployment on EKS
✅ Public access via AWS LoadBalancer
✅ Centralized logging with CloudWatch
✅ Zero manual intervention after setup
____________________________________________________________________________
🔧 Implementation Steps
1️⃣ Application Setup
Used a pre-built React app (dist/ folder)
Served using Nginx
____________________________________________________________________________
2️⃣ Dockerization
Created Dockerfile using Nginx
Built and tested container locally
____________________________________________________________________________
3️⃣ Amazon ECR
Created ECR repository
Tagged and pushed Docker image
____________________________________________________________________________
4️⃣ Kubernetes Deployment (EKS)
Created EKS cluster using eksctl
Deployed application using:
Deployment.yaml
Service.yaml (LoadBalancer)
Exposed application via public URL
____________________________________________________________________________
5️⃣ CI/CD with CodeBuild
Created buildspec.yml
Automated:
Docker build
Image tagging
Push to ECR
____________________________________________________________________________
6️⃣ CI/CD with CodePipeline
Connected GitHub repository
Integrated CodeBuild
Enabled automatic pipeline trigger
____________________________________________________________________________
7️⃣ Auto Deployment to EKS
Integrated kubectl commands in buildspec
Enabled automatic deployment after each build
____________________________________________________________________________
8️⃣ Monitoring with CloudWatch
Tracked build logs
Debugged pipeline issues
Verified successful execution
____________________________________________________________________________
📸 Project Proof
🔹 Before & After CI/CD Deployment
Demonstrates automatic application update after Git push
🔹 Pipeline Execution
CodePipeline success stages
🔹 Build Logs
CodeBuild logs from CloudWatch
🔹 Kubernetes
Running pods and services
____________________________________________________________________________
🧠 Key Learnings
Deep understanding of CI/CD pipelines
Hands-on experience with AWS EKS, ECR, CodeBuild, CodePipeline
Debugging real-world issues:
IAM permissions
GitHub connection errors
ECR authentication issues
Kubernetes deployment & service exposure
Monitoring and logging using CloudWatch
🎯 Outcome

Successfully built a production-grade CI/CD pipeline where:

Code changes automatically trigger pipeline
Docker images are built and pushed
Application is deployed to Kubernetes
Updates reflect live without manual intervention
🔗 Live Application

🌐 Available via AWS LoadBalancer (dynamic URL)

💡 Future Enhancements
🔹 Custom domain with Route 53
🔹 HTTPS using AWS ACM
🔹 Kubernetes Ingress
🔹 Helm charts
🔹 Monitoring with Prometheus & Grafana
🤝 Connect with Me

If you found this project interesting, feel free to connect or reach out!
