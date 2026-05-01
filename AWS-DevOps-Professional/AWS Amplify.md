---
tags: [AWS, DevOps, Frontend, Mobile, Web]
---

# AWS Amplify

[[AWS Amplify]] is a set of purpose-built tools and features that lets frontend web and mobile developers quickly and easily build full-stack applications on AWS. It automates the creation, management, and distribution of applications by integrating various underlying AWS services.

## CI/CD and Branch Environments
Amplify offers native hosting with a built-in CI/CD workflow. You can integrate Amplify directly with version control repositories like [[AWS CodeCommit]], GitHub, or GitLab.
- **Environment per Branch:** You can map different repository branches to distinct web environments. For example, you can connect the `main` or `master` branch to your Production environment, and a `develop` or `stage` branch to your Staging environment. Amplify automatically builds and deploys the site whenever a new commit is pushed to the respective branch.
