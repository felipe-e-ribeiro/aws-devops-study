---
tags: [AWS, DevOps, DeveloperTools, MachineLearning, CodeQuality]
---

# AWS CodeGuru

[[AWS CodeGuru]] is a machine learning-based recommendation service that helps developers identify cost optimizations, improve code quality, and enhance application performance based on workloads.

## Key Capabilities

AWS CodeGuru consists of two primary components:

1. **CodeGuru Reviewer:** Analyzes static source code to identify critical issues, security vulnerabilities, and hard-to-find bugs. It integrates directly into the CI/CD pipeline or pull requests.
   - **Secrets Detection:** As part of the Reviewer, it automatically scans for hardcoded secrets, passwords, or keys. It can optionally point to or facilitate storing these credentials securely using [[AWS Secrets Manager]].
2. **CodeGuru Profiler:** Analyzes application performance in runtime (production or pre-production environments) and provides actionable recommendations to optimize CPU utilization, reduce latency, and lower compute costs.

## Exam Tips
- **CodeGuru Reviewer** is focused on *static* code analysis (e.g., during pull requests in [[AWS CodeCommit]] or GitHub).
- **CodeGuru Profiler** requires an agent installed in the application (like a JVM agent or Python agent) to profile *dynamic* runtime behavior.
- Use **Secrets Detection** to automatically find and remediate embedded credentials, leveraging [[AWS Secrets Manager]] for secure storage.
