Final Project Phases (clean + professional)
Here’s the exact flow we’ll follow:
✅ Phase 1 — Application Setup
Clone repo 
Install dependencies 
Run React app on port 3000 
Verify in browser 
Create Dockerfile 
Build image locally 
Run container (port 3000) 
Validate 
Create ECR repo 
Tag + push Docker image 
☸️ Phase 4 — EKS Setup
 Create cluster 
Configure kubectl 
IAM roles & permissions 
Write deployment.yaml 
Write service.yaml (LoadBalancer) 
Deploy app 
Create project 
Write buildspec.yml 
Build + push image 
GitHub → CodeBuild → EKS deploy 
CloudWatch logs 
README (we build it alongside, not at the end)
## Dockerization

The application is containerized using Nginx to serve static files from the `dist/` directory.

### Steps:

1. Created a Dockerfile using `nginx:alpine` as base image
2. Configured custom `nginx.conf` to run on port 3000
3. Copied `dist/` files into `/usr/share/nginx/html`
4. Built Docker image
5. Ran container and verified application

### Commands:

docker build -t brain-tasks-app .
docker run -d -p 3000:3000 brain-tasks-app

## Amazon ECR (Elastic Container Registry)

The Docker image is pushed to AWS ECR for storage and deployment.

### Steps:

1. Created ECR repository
2. Authenticated Docker with AWS
3. Tagged Docker image
4. Pushed image to ECR

### Commands:

aws ecr create-repository --repository-name brain-tasks-app --region <region>

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com

docker tag brain-tasks-app:latest <account-id>.dkr.ecr.<region>.amazonaws.com/brain-tasks-app:latest

docker push <account-id>.dkr.ecr.<region>.amazonaws.com/brain-tasks-app:latest

## Amazon EKS (Elastic Kubernetes Service)

An EKS cluster is created to deploy the containerized application.

### Steps:

1. Installed eksctl, kubectl, AWS CLI
2. Created EKS cluster using eksctl
3. Verified worker nodes
4. Confirmed cluster connectivity

### Command:

eksctl create cluster \
  --name brain-tasks-cluster \
  --region ap-south-1 \
  --node-type t3.medium \
  --nodes 2

kubectl get nodes
## Kubernetes Deployment

The application is deployed to AWS EKS using Kubernetes.

### Steps:

1. Created Deployment YAML (2 replicas)
2. Created Service YAML (LoadBalancer)
3. Applied configurations using kubectl
4. Accessed application via external IP

### Commands:

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl get pods
kubectl get svc

## AWS CodeBuild

CodeBuild is used to automate Docker image build and push to ECR.

### Steps:

1. Created buildspec.yml
2. Configured CodeBuild project
3. Enabled privileged mode for Docker
4. Built and pushed image automatically

### Outcome:

Docker image is built and stored in ECR via CI pipeline.

### Issue Faced: ECR Access Denied in CodeBuild

During the build process, CodeBuild failed with an AccessDeniedException for ECR authentication.

**Root Cause:**
The IAM role used by CodeBuild did not have sufficient permissions to access ECR.

**Solution:**
Attached the following policies to the CodeBuild IAM Role:

- AmazonEC2ContainerRegistryFullAccess
- CloudWatchLogsFullAccess

After attaching the policies, the build succeeded successfully.

## CodeBuild Success

The build process was successfully executed using AWS CodeBuild.

### Key Outcomes:

- Successfully authenticated with Amazon ECR
- Docker image built using Nginx base image
- Image tagged and pushed to ECR
- Build completed without errors

### Logs Insight:

All phases (PRE_BUILD, BUILD, POST_BUILD) completed successfully.

### Issue: CodePipeline Connection Permission Error

Error:
"Provided role does not have sufficient permissions"

**Root Cause:**
CodePipeline IAM role lacked permission to use GitHub connection.

**Solution:**
Attached policy:
AWSCodeStarConnectionsFullAccess

**Result:**
Pipeline successfully accessed GitHub repository.

## CI/CD Pipeline (Final Implementation)

The project implements a full CI/CD pipeline using AWS services.

### Flow:
GitHub → CodePipeline → CodeBuild → ECR → EKS

### Features:
- Automatic trigger on code push
- Docker image build and push to ECR
- Automatic deployment to EKS
- Application updates without manual intervention

### Outcome:
The application is continuously integrated and deployed with every code change.
To automate the complete CI/CD workflow for the Brain Tasks application using AWS CodePipeline, integrating GitHub, CodeBuild, Amazon ECR, and Amazon EKS.

GitHub → CodePipeline → CodeBuild → Amazon ECR → Amazon EKS

Created a pipeline using Build Custom Pipeline option
Configured pipeline stages manually for better control
Faced issues due to multiple GitHub connections and miss-configuration
❌ Issues Faced:
GitHub repository not visible
Invalid repository format errors
Connection not available errors
✅ Resolution:
Deleted incorrect connections
Recreated GitHub connection properly
Authorized AWS Connector in GitHub with correct repository access
Ensured connection status showed Available

Connected GitHub repository to CodePipeline
Selected:
Repository: brain-tasks-devops
Branch: main
AWS uses GitHub App-based authentication, not personal tokens. Proper repository access must be granted during authorization.
✅ Outcome:
Pipeline successfully fetched source code
Automatic trigger enabled on every Git push

❌ Issue:
Pipeline failed with error:
"Provided role does not have sufficient permissions"
CodePipeline IAM role lacked permission to use GitHub connection
✅ Solution:
Attached the following policy to CodePipeline role:
AWSCodeStarConnectionsFullAccess
Pipeline successfully accessed GitHub repository

Integrated existing CodeBuild project into pipeline
CodeBuild configuration included:
Docker image build
Image tagging
Push to Amazon ECR
ECR login
Docker build
Docker tag
Docker push
❌ Issue Faced:
ECR Access Denied error
✅ Solution:
Attached required IAM policies to CodeBuild role:
AmazonEC2ContainerRegistryFullAccess
CloudWatchLogsFullAccess
Triggered initial pipeline execution manually
Verified:
Source stage → Success
Build stage → Success
Confirmed Docker image pushed to ECR
Made test changes in application files
Pushed changes to GitHub
✅ Observed Behavior:
Pipeline triggered automatically
CodeBuild executed without manual intervention
New Docker image built and pushed
Application updated in EKS
Fully automated CI/CD pipeline established
Zero manual intervention required after code push
Continuous deployment to Kubernetes cluster achieved
Importance of IAM roles in AWS services
Difference between CodeBuild role and CodePipeline role
GitHub connection authorization using AWS Connector
Handling real-world pipeline errors and debugging
End-to-end automation of containerized application

Successfully implemented a production-grade CI/CD pipeline using AWS services. The pipeline ensures seamless integration, build, and deployment of application updates, demonstrating real-world DevOps practices.
## CI/CD Proof (Before & After)

### Before Change
[Attach Screenshot]

### After Change
[Attach Screenshot]

### Observation
After pushing code changes to GitHub, the pipeline triggered automatically and updated the application without manual intervention.

Amazon CloudWatch is used to monitor and track logs generated during the CI/CD pipeline execution.
CodeBuild Logs :
All build phases (PRE_BUILD, BUILD, POST_BUILD) are logged
Logs include Docker build, image push, and deployment steps
Helps in debugging build failures
Log Groups
Logs are stored under:
/codebuild/
Each build creates a new log stream
Benefits
Real-time monitoring of pipeline execution
Easy debugging of errors
Centralized logging for CI/CD pipeline
CodeBuild logs (successful execution)
CloudWatch log group and log stream
