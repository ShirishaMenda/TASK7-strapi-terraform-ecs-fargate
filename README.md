# Strapi Deployment on AWS ECS Fargate using Terraform & GitHub Actions

# This project demonstrates deploying a Strapi application on:

Amazon ECS Fargate
Amazon RDS (PostgreSQL)
Amazon ECR
Application Load Balancer (ALB)
Automated using Terraform + GitHub Actions CI/CD


# Step 1: Create Strapi Application

Created a new project folder.

Inside that folder, created a Strapi application:

npx create-strapi-app strapi-app

Configured Strapi to use PostgreSQL instead of SQLite.

Verified locally that Strapi runs successfully.


# Step 2: Create Amazon ECR Using Terraform

Wrote Terraform code to create an ECR repository.

Files used:

ecr.tf
provider.tf

Ran Terraform commands:

terraform init
terraform plan
terraform apply

ECR repository was successfully created in AWS.


# Step 3: Build, Tag and Push Docker Image to ECR

Created a Dockerfile for Strapi.

Built Docker image:

docker build -t strapi-app .

Tagged the image:

docker tag strapi-app:latest <ecr-repo-url>:latest

Logged into ECR:

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <ecr-repo-url>

Pushed image to ECR:

docker push <ecr-repo-url>:latest


# Step 4: Deploy Infrastructure Using Terraform

Terraform creates:

VPC

Subnets

Internet Gateway

Security Groups

RDS PostgreSQL

ECS Cluster

ECS Task Definition

ECS Service

Application Load Balancer

Commands used:

terraform apply

After deployment:

Verified ECS service is running.

Checked that tasks are healthy.

Confirmed RDS is available.

Verified ALB target group is healthy.


# Step 5: Configure GitHub Actions (CI/CD)

Added workflow file inside:

.github/workflows/deploy.yml

Workflow performs:

Build Docker image

Tag image

Push image to ECR

Update ECS service

Added AWS credentials as GitHub Secrets:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_REGION

Pushed code to GitHub repository.

Once pushed:

GitHub Actions automatically triggered.

CI builds Docker image and pushes image to ecr.

CD initialize terrform and terraform apply to revision the task definition.

ECS service updates automatically.



# Step 6: Access the Application

After successful deployment:

Go to AWS Console.

Open EC2 → Load Balancers.

Copy the ALB DNS URL.

Example:

http://my-alb-123456.ap-south-1.elb.amazonaws.com

Open the DNS URL in browser.

Strapi application loads successfully.
