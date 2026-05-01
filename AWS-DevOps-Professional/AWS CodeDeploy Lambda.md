---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Deployment, Lambda]
---

# AWS CodeDeploy Lambda

[[AWS CodeDeploy]] can also be used to update [[AWS Lambda]] function aliases gradually. The `appspec.yml` for Lambda specifies the current version, the target version, and Lambda functions for validation (hooks). It utilizes the same deployment strategies as ECS (Canary, Linear, AllAtOnce).
