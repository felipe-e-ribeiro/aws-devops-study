---
tags: [AWS, DevOps, DeveloperTools, CI-CD]
---

# AWS CodePipeline

[[AWS CodePipeline]] is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

## Key Capabilities (From Update)
- **Pipeline Stages:** It typically consists of three main stages:
  - **Source:** Supports multiple version control repositories as sources, such as [[AWS CodeCommit]], GitHub, GitLab, etc.
  - **Build:** Facilitates artifact creation using services like [[AWS CodeBuild]], Jenkins, or [[AWS CloudFormation]].
  - **Test:** Integrates with tools like [[AWS CodeBuild]], [[AWS Device Farm]], or other testing frameworks to apply environment tests and validations.
