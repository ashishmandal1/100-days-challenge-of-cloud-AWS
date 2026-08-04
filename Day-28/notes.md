# Day 28 – Amazon Elastic Container Registry (ECR)

## Objective
The objective of this task was to create a private Amazon Elastic Container Registry (ECR) repository, build a Docker image from a Dockerfile, authenticate Docker with ECR, push the image to the repository, and verify that the image was successfully uploaded.

---

# What is Amazon ECR?

Amazon Elastic Container Registry (ECR) is a fully managed container image registry service provided by AWS. It allows developers to securely store, manage, and deploy Docker and OCI-compatible container images. Amazon ECR integrates seamlessly with AWS services such as Amazon ECS, Amazon EKS, AWS Lambda, and CI/CD pipelines.

---

# Why Use Amazon ECR?

- Securely stores Docker container images.
- Fully managed by AWS.
- Supports private and public repositories.
- Integrates with IAM for access control.
- Supports image versioning through tags.
- Can scan images for security vulnerabilities.
- Works seamlessly with Amazon ECS and Amazon EKS.

---

# Services Used

- Amazon Elastic Container Registry (ECR)
- AWS Identity and Access Management (IAM)
- Docker
- AWS CLI

---

# Task Performed

- Created a private ECR repository named **devops-ecr**.
- Verified AWS CLI credentials.
- Retrieved the repository URI.
- Authenticated Docker with Amazon ECR.
- Built a Docker image from the Dockerfile located in `/root/pyapp`.
- Tagged the Docker image with the ECR repository URI.
- Pushed the Docker image to Amazon ECR with the `latest` tag.
- Verified that the image was successfully uploaded.

---

# Commands Used

## Verify AWS Credentials

```bash
aws sts get-caller-identity
```

---

## Create ECR Repository

```bash
aws ecr create-repository \
--repository-name devops-ecr \
--region us-east-1
```

---

## Get Repository URI

```bash
REPO_URI=$(aws ecr describe-repositories \
--repository-names devops-ecr \
--query "repositories[0].repositoryUri" \
--output text)

echo $REPO_URI
```

---

## Login Docker to Amazon ECR

```bash
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin $(echo $REPO_URI | cut -d/ -f1)
```

---

## Navigate to Docker Project

```bash
cd /root/pyapp
```

---

## Build Docker Image

```bash
docker build -t devops-ecr:latest .
```

---

## Tag Docker Image

```bash
docker tag devops-ecr:latest $REPO_URI:latest
```

---

## Push Docker Image to ECR

```bash
docker push $REPO_URI:latest
```

---

## Verify Uploaded Image

```bash
aws ecr list-images --repository-name devops-ecr
```

---

# Docker Commands Learned

Build Docker image

```bash
docker build -t image-name:tag .
```

List Docker images

```bash
docker images
```

Tag Docker image

```bash
docker tag source-image target-image
```

Push Docker image

```bash
docker push image-name
```

---

# AWS CLI Commands Learned

Create repository

```bash
aws ecr create-repository --repository-name <repository-name>
```

Describe repositories

```bash
aws ecr describe-repositories
```

List images

```bash
aws ecr list-images --repository-name <repository-name>
```

Describe images

```bash
aws ecr describe-images --repository-name <repository-name>
```

Delete repository

```bash
aws ecr delete-repository --repository-name <repository-name> --force
```

---

# Key Concepts Learned

- Amazon ECR is a secure container image registry service.
- Docker images must be tagged with the ECR repository URI before they can be pushed.
- Docker must be authenticated using `aws ecr get-login-password` before interacting with ECR.
- The `latest` tag represents the most recent version of an image by convention.
- AWS CLI simplifies ECR management through command-line operations.

---

# Common Errors

### Authentication Error

**Error**

```
no basic auth credentials
```

**Solution**

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <registry-url>
```

---

### Repository Already Exists

```
RepositoryAlreadyExistsException
```

Continue using the existing repository.

---

### Invalid AWS Credentials

Verify credentials using:

```bash
aws sts get-caller-identity
```

---

### Image Not Found

Verify local images:

```bash
docker images
```

Ensure the image is correctly tagged before pushing.

---

# Best Practices

- Use meaningful image tags such as `v1.0.0` instead of always using `latest`.
- Enable image scanning for security vulnerabilities.
- Remove unused images and repositories to reduce storage costs.
- Use IAM policies with the principle of least privilege.
- Store container images in private repositories unless public access is required.

---

# Outcome

Successfully created a private Amazon ECR repository named **devops-ecr**, built a Docker image from the provided Dockerfile, authenticated Docker with Amazon ECR, tagged and pushed the image using the **latest** tag, and verified that the image was successfully stored in the repository.