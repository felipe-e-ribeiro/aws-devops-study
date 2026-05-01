---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Deployment]
---

# AWS CodeDeploy

[[AWS CodeDeploy]] is a fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers.

## Key Capabilities (From Update)
- **Deployment Automation:** An application deployment tool, utilized in conjunction with [[AWS CodePipeline]].
- **Platform Support:** It can be used to perform deployments across diverse platforms, such as [[Amazon EC2]], [[Amazon ECS]], [[AWS Lambda]], [[Amazon EKS]], and more.
- **CodeDeploy Agent:** An important characteristic of CodeDeploy is the use of agents that run on the target instances. These agents are responsible for receiving instructions from CodeDeploy and executing the necessary actions on the server, such as copying files, running scripts, and managing the application state.
- **Deployment Strategies:** A key differentiator of CodeDeploy is its flexibility in managing deployment strategies, supporting different approaches to update your applications safely and efficiently:
  - **All at once**
  - **Rolling**
  - **Rolling with additional configuration**
  - **Blue/green**
