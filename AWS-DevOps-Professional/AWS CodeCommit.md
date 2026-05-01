---
tags: [AWS, DevOps, DeveloperTools, SourceControl]
---

# AWS CodeCommit

[[AWS CodeCommit]] is a fully managed source control service by AWS that hosts secure, Git-based repositories for storing and managing source code and artifacts.

## Key Capabilities (From Update)
- **Code Repository:** Acts as a highly scalable and secure managed version control service for hosting private Git repositories.
- **Notifications:** You can configure notification processes by natively integrating with [[Amazon SNS]] (or [[Amazon EventBridge]]) to send alerts for repository events (like commits, pull requests, etc).
- **Deployment Integration:** **Note on CodeDeploy:** You cannot directly "attach" [[AWS CodeDeploy]] to CodeCommit. Instead, you integrate them by using [[AWS CodePipeline]]. CodePipeline acts as the orchestrator that automatically fetches source code revisions from [[AWS CodeCommit]] and provides them as an artifact to [[AWS CodeDeploy]].

- **Triggers:** Repository triggers can be used to invoke an [[AWS Lambda]] function or publish an [[Amazon SNS]] topic whenever specific repository events occur without needing a full pipeline.
- **Security:** Access control is entirely governed by [[AWS Identity and Access Management]] ([[IAM]]) using git credentials or SSH keys. Data is encrypted at rest automatically using [[AWS KMS]].
