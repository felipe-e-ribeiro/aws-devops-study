# Content Domain 1: SDLC Automation
## Task Statement 1.1: Implement CI/CD pipelines.
### Knowledge of:
+ Software development lifecycle (SDLC) concepts, phases, and models
+ Pipeline deployment patterns for single- and multi-account environments

### Skills in:
+ Configuring code, image, and artifact repositories
+ Using version control to integrate pipelines with application environments
+ Setting up build processes (for example, [[AWS CodeBuild]])
+ Managing build and deployment secrets (for example, [[AWS Secrets Manager]], [[AWS Systems Manager]] Parameter Store)
+ Determining appropriate deployment strategies (for example, [[AWS CodeDeploy]])

## Task Statement 1.2: Integrate automated testing into CI/CD pipelines.
### Knowledge of:
+ Different types of tests (for example, unit tests, integration tests, acceptance tests, user interface tests, security scans)
+ Reasonable use of different types of tests at different stages of the CI/CD pipeline

### Skills in:
+ Running builds or tests when generating pull requests or code merges (for example, CodeBuild)
+ Running load/stress tests, performance benchmarking, and application testing at scale
+ Measuring application health based on application exit codes
+ Automating unit tests and code coverage
+ Invoking AWS services in a pipeline for testing

## Task Statement 1.3: Build and manage artifacts.
### Knowledge of:
+ Artifact use cases and secure management
+ Methods to create and generate artifacts
+ Artifact lifecycle considerations

### Skills in:
+ Creating and configuring artifact repositories (for example, [[AWS CodeArtifact]], [[Amazon S3]], [[Amazon Elastic Container Registry]] [[[Amazon ECR]]])
+ Configuring build tools for generating artifacts (for example, CodeBuild, [[AWS Lambda]])
+ Automating [[Amazon EC2]] instance and container image build processes (for example, [[EC2 Image Builder]])

## Task Statement 1.4: Implement deployment strategies for instance, container, and serverless environments.
### Knowledge of:
+ Deployment methodologies for various platforms (for example, [[Amazon EC2]], [[Amazon Elastic Container Service]] [[[Amazon ECS]]], [[Amazon Elastic Kubernetes Service]] [[[Amazon EKS]]], Lambda)
+ Application storage patterns (for example, [[Amazon Elastic File System]] [[[Amazon EFS]]], [[Amazon S3]], [[Amazon Elastic Block Store]] [[[Amazon EBS]]])
+ Mutable deployment patterns in contrast to immutable deployment patterns
+ Tools and services available for distributing code (for example, CodeDeploy, Image Builder)

### Skills in:
+ Configuring security permissions to allow access to artifact repositories (for example, [[AWS Identity and Access Management]] [[[IAM]]], CodeArtifact)
+ Configuring deployment agents (for example, [[CodeDeploy agent]])
+ Troubleshooting deployment issues
+ Using different deployment methods (for example, blue/green, canary)
