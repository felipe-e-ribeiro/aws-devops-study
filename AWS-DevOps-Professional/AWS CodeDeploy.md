---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Deployment]
---

# AWS CodeDeploy

[[AWS CodeDeploy]] is a fully managed deployment service that automates software deployments to compute services such as [[Amazon EC2]], [[Amazon ECS]], [[AWS Lambda]], and your on-premises servers.

## Key Capabilities
- **Deployment Automation:** An application deployment tool, utilized in conjunction with [[AWS CodePipeline]].
- **Platform Support:** It can be used to perform deployments exclusively across [[Amazon EC2]], [[Amazon ECS]], and [[AWS Lambda]] (along with on-premises servers).
- **CodeDeploy Agent:** An important characteristic of CodeDeploy is the use of agents that run on the target instances (for EC2/On-Premises). These agents receive instructions from CodeDeploy and execute necessary actions on the server.
- **Deployment Strategies:** CodeDeploy supports different approaches to update your applications safely and efficiently:
  - **In-Place (Rolling)**: Gradually updates existing instances (Only for EC2/On-Premises).
  - **Blue/Green**: Creates a new environment (Green) and routes traffic from the old environment (Blue) to the new one.
  - **Canary / Linear**: Gradual traffic shifting, very common for Lambda and ECS.

## Deployment Targets
For detailed information regarding how CodeDeploy integrates with specific compute targets, see the following dedicated topics:
- [[AWS CodeDeploy EC2]]
- [[AWS CodeDeploy ECS]]
- [[AWS CodeDeploy Lambda]]
