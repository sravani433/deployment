# Hello World Python Application on Amazon EKS

This repository contains a simple "Hello World" Python application packaged into a Docker image and deployed to an Amazon EKS (Elastic Kubernetes Service) cluster. The process utilizes AWS CLI commands to manage resources on the AWS Free Tier, ensuring cost-effectiveness.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Application Setup](#application-setup)
3. [Docker Setup](#docker-setup)
4. [AWS ECR Setup](#aws-ecr-setup)
5. [EKS Cluster Setup](#eks-cluster-setup)
6. [Deployment](#deployment)
7. [Accessing the Application](#accessing-the-application)

## Prerequisites

- An AWS account
- AWS CLI installed and configured
- Docker installed and running
- eksctl installed
- kubectl installed

## Application Setup

1. **Create the Hello World Python Application**

   ```bash
   mkdir hello-world-app
   cd hello-world-app
   echo 'print("Hello, World!")' > hello.py

## Docker Setup

### Create a Dockerfile

First, create a `Dockerfile` to build the Docker image:

```bash
FROM python:3.9-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    libffi-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . .

# Upgrade pip
RUN pip install --upgrade pip

# Install Python packages
RUN pip install --no-cache-dir -r requirements.txt

# Expose a port if needed
EXPOSE 80

# Command to run your application
CMD ["python", "app.py"]  > Dockerfile
```

### Build the Docker Image

Build the Docker image using the following command:

```bash
docker build -t hello-world-app .
```

## AWS ECR Setup

### Authenticate Docker with ECR

Log in to your AWS account and run the following command to authenticate Docker with ECR:

```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 676206922543.dkr.ecr.us-west-2.amazonaws.com
```

### Create ECR Repository

Create a repository to hold your Docker image:

```bash
aws ecr create-repository --repository-name hello-world-app --region us-west-2
```

### Tag and Push the Docker Image to ECR

Tag your Docker image and push it to ECR:

```bash
docker tag hello-world-app:latest 676206922543.dkr.ecr.us-west-2.amazonaws.com/hello-world-app:latest
docker push 676206922543.dkr.ecr.us-west-2.amazonaws.com/hello-world-app:latest
```

## EKS Cluster Setup

### Create an EKS Cluster

Make sure you have `eksctl` installed. Then, create an EKS cluster:

```bash
eksctl create cluster --name hello-world-cluster --region us-west-2 --nodes 2 --node-type t2.micro --managed
```

## Deployment

### Deploy the Application on EKS

Create a Kubernetes deployment configuration file called `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: 676206922543.dkr.ecr.us-west-2.amazonaws.com/hello-world-app:latest
        ports:
        - containerPort: 80
```

Deploy the application using `kubectl`:

```bash
kubectl apply -f deployment.yaml
```

## Accessing the Application

### Expose the Application

Expose the deployment to access it externally:

```bash
kubectl expose deployment hello-world --type=LoadBalancer --port=80
```

### Get the External IP Address

Get the external IP to access your application:

```bash
kubectl get services
```

After a few moments, you should see an external IP address that you can use to access your "Hello, World!" application.
