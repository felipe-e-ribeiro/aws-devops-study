---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Artifacts]
---

# AWS CodeArtifact

[[AWS CodeArtifact]] is a fully managed artifact repository service that makes it easy for organizations to securely store, publish, and share software packages used in their software development process. It supports common package managers and build tools such as Maven, Gradle, npm, yarn, twine, and pip.

## Integration with CodeBuild
[[AWS CodeBuild]] can use CodeArtifact to fetch artifacts and dependencies. Under the hood, CodeArtifact securely stores the packages in an [[Amazon S3]] bucket and serves them to CodeBuild.

## Upstream Repositories
You can configure CodeArtifact to fetch artifacts from public repositories such as Maven Central, npm registry, or PyPI. 
- You can configure up to **10 upstream repositories**.
- You can have an external connection (to fetch from public registries), but it is limited to **only one external connection per CodeArtifact repository**.

## Package Retention and Caching Strategy
When chaining repositories (e.g., Repositories A, B, and C):
- Assume the developer is connected to **Repository A**.
- **Repository A** has an upstream connection to **Repository B**.
- **Repository B** has an upstream connection to **Repository C**.
- **Repository C** has an external connection to a public registry (e.g., npm public).

If a developer requests a package from Repository A, CodeArtifact will fetch it from the public registry through C. The package will be **cached in Repository C** (because it has the external connection) and **cached in Repository A** (because it is the requested repository). **Repository B will NOT cache** the package.

By default, CodeArtifact retains dependencies for **90 days**, but this retention period is configurable.

## Exam Tips
- **Cross-Account Access:** You can share a CodeArtifact repository across different AWS accounts using Resource-Based Policies (IAM).
- **EventBridge Integration:** CodeArtifact integrates with [[Amazon EventBridge]] to trigger events when packages are published, modified, or deleted, which can invoke an [[AWS Lambda]] function or an [[AWS CodePipeline]] execution.
- **VPC Endpoints:** For enhanced security, use an interface VPC endpoint (AWS PrivateLink) to connect to CodeArtifact from an [[Amazon VPC]] without going over the public internet.
