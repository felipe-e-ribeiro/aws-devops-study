---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Build]
---

# AWS CodeBuild

[[AWS CodeBuild]] is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.

## Key Capabilities
- **Application Build Tool:** Used to build applications across various programming languages, including Java, Node.js, Python, Ruby, Go, .NET, and more.
- **Docker Support:** Can be used to build Docker applications by utilizing a `Dockerfile` to create container images.
- **Local Execution:** Can be executed locally via Docker using the official image provided by AWS: `public.ecr.aws/codebuild/local-build-runner:latest`.
- **VPC Integration:** CodeBuild can be configured to run within an [[Amazon VPC]]. This is crucial for builds that require access to internal private resources, such as databases.
- **Webhook Triggers:** Supports triggering builds automatically using various repository events, such as `push`, `pull request`, and others.

## Configuration and buildspec.yml

To configure CodeBuild, you need to create a `buildspec.yml` file, which must be placed at the root of your project. This file is responsible for defining the phases that will be executed during the build process.

The main phases are:
1. `install`: Installation of dependencies or runtimes.
2. `pre_build`: Commands executed before the build (e.g., linting, unit tests, registry login).
3. `build`: Compiling and building the application.
4. `post_build`: Commands executed after the build (e.g., packaging, pushing to [[Amazon S3]] or [[Amazon ECR]]).

Example `buildspec.yml`:
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - echo "Installing dependencies..."
      - npm ci
  pre_build:
    commands:
      - echo "Running tests..."
      - npm test
  build:
    commands:
      - echo "Building the application..."
      - npm run build
  post_build:
    commands:
      - echo "Pushing artifacts to S3..."
      - aws s3 sync ./build s3://<your-bucket-name>/ --delete
artifacts:
  files:
    - '**/*'
  base-directory: build
```
