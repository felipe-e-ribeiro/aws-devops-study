---
tags: [AWS, DevOps, DeveloperTools, CI-CD]
---

# AWS CodePipeline

[[AWS CodePipeline]] is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

## Key Capabilities
- **Orchestration:** It is the CI/CD orchestration service that integrates all Developer Tools. You can configure pipelines with multiple stages.
- **Pipeline Stages:** It typically consists of these main stages:
  - **Source:** Supports multiple version control repositories as sources, such as [[AWS CodeCommit]], GitHub, Bitbucket, and [[Amazon S3]].
  - **Build:** Facilitates artifact creation using services like [[AWS CodeBuild]] or Jenkins.
  - **Test:** Integrates with tools like [[AWS CodeBuild]], [[AWS Device Farm]], or other testing frameworks to apply environment tests and validations.
  - **Deploy:** Automates deployments using [[AWS CodeDeploy]], [[Amazon ECS]], [[AWS CloudFormation]], [[AWS Elastic Beanstalk]], etc.
- **Artifact Management:** CodePipeline uses an [[Amazon S3]] artifact bucket to pass the output of one stage (output artifact) to the next stage (input artifact). It encrypts the data at rest using [[AWS KMS]].
