# MLflow Repository

This repository is exclusively responsible for **experiment tracking and model artifact management** using MLflow.

It provides a centralized platform for logging, monitoring, and comparing machine learning experiments across the entire pipeline.

The repository is maintained with **two distinct branches**, each targeting a different environment and deployment workflow.

---

# `main` Branch — Local Development

The `main` branch is intended for local development and testing.

In this setup:

* MLflow runs locally using Docker
* Experiment metadata is stored locally
* Model artifacts can be stored locally or in Amazon S3
* AWS credentials are configured through a local `.env` file when using S3 artifact storage

## AWS Credentials

If S3 is used as the artifact store, configure your AWS credentials in a `.env` file.

Example:

```env
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_DEFAULT_REGION=us-east-1
```

## Security Recommendations

Sensitive files should never be committed to the repository.

Ensure the following files are included in both `.gitignore` and `.dockerignore`:

```gitignore
.env
```

---

# `dev-prod` Branch — Production Environment

The `dev-prod` branch contains the production-ready configuration used for hosting MLflow in AWS.

This branch uses:

* Amazon ECS
* Amazon ECR
* GitHub Actions CI/CD
* AWS OIDC Authentication
* Amazon S3 for artifact storage

Unlike the `main` branch, production deployments do not rely on static AWS credentials.

MLflow is deployed as a containerized service running on ECS.

---

# CI/CD Pipelines

## `main` Branch CI/CD

The CI/CD pipeline for `main`:

1. Builds the MLflow image
2. Pushes the image to Docker Hub

### Required GitHub Secrets

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

---

## `dev-prod` Branch CI/CD

The CI/CD pipeline for `dev-prod`:

1. Builds the MLflow image
2. Pushes the image to Amazon ECR
3. Updates ECS task definitions
4. Deploys the latest MLflow service

This pipeline authenticates with AWS using OIDC.

### Required GitHub Secrets / Variables

```text
AWS_REGION
AWS_ARN_ROLE
```

* `AWS_REGION`: AWS region where the infrastructure is deployed.
* `AWS_ARN_ROLE`: IAM role assumed by GitHub Actions through OIDC.

---

# Runtime Environment Variables

MLflow containers receive their runtime configuration through ECS task definitions.

Common variables include:

```text
MLFLOW_BACKEND_STORE_URI
MLFLOW_ARTIFACT_ROOT
AWS_REGION
S3_BUCKET_NAME
```

Additional variables may be injected depending on the infrastructure configuration.

---

# Purpose

This repository is responsible for:

1. Tracking machine learning experiments
2. Logging training metrics
3. Storing model artifacts
4. Recording hyperparameters
5. Comparing experiment runs
6. Managing model versions

MLflow serves as the central experiment tracking platform used by the machine learning pipeline.

---

# Infrastructure as Code (IaC)

Infrastructure resources such as ECS clusters, ECR repositories, networking, IAM roles, load balancers, and task definitions are managed separately through the Infrastructure as Code repository.

For infrastructure configuration details, refer to the corresponding [IaC repository](https://github.com/Pipeout/IaC/blob/main/ecs_task_definitions.tf).

---

# Notes

* The `main` branch is intended for local development and testing.
* The `dev-prod` branch is intended for production deployment in AWS.
* This repository does not perform feature engineering, model training, or model serving; its sole responsibility is experiment tracking and artifact management through MLflow.
* Environment-specific configuration should be provided through GitHub Actions secrets, ECS task definitions, or AWS services rather than hardcoded values.
