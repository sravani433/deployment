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
