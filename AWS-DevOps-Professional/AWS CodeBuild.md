---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Build]
---

# AWS CodeBuild

[[AWS CodeBuild]] is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.

## Key Capabilities (From Update)
- **Application Build Tool:** Used to build applications across various programming languages, including Java, Node.js, Python, Ruby, Go, .NET, and more.
- **Docker Support:** Can be used to build Docker applications by utilizing a `Dockerfile` to create container images.
- **Local Execution:** Can be executed locally via Docker using the official image provided by AWS: `public.ecr.aws/codebuild/local-build-runner:latest`.
- **VPC Integration:** CodeBuild can be configured to run within an [[Amazon VPC]]. This is crucial for builds that require access to internal private resources, such as databases.
- **Webhook Triggers:** Supports triggering builds automatically using various repository events, such as `push`, `pull request`, and others.
