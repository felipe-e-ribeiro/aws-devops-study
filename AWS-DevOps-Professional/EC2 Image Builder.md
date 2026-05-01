---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Compute, AMIs]
---

# EC2 Image Builder

[[EC2 Image Builder]] is a fully managed service that simplifies and automates the creation, management, testing, and distribution of server images (Amazon Machine Images - AMIs) and container images.

## How it Works
When executing a pipeline, EC2 Image Builder provisions a temporary [[Amazon EC2]] instance (the "Builder Instance"). It uses this instance to build a new AMI based on your defined recipes, tests the resulting image, and then distributes it to target regions or accounts.

## CI/CD Pipeline Example
Using [[AWS CodePipeline]] to orchestrate the build process:
1. **Source/Build Code:** Code in [[AWS CodeCommit]] triggers [[AWS CodeBuild]] to compile and validate the application code.
2. **Build AMI:** A stage invoking [[AWS CloudFormation]] is triggered, which in turn provisions/updates the [[EC2 Image Builder]] resources to create the new AMI.
3. **Deploy (Rollout):** The newly created AMI can be automatically integrated into an [[Amazon EC2 Auto Scaling]] group via CloudFormation or [[AWS CodeDeploy]], performing a rolling update or Blue/Green deployment with the new image.

## Cross-Account Sharing
You can share Images, Image Recipes, and Components across different AWS accounts within an [[AWS Organizations]] structure using [[AWS Resource Access Manager]] (RAM).

## Tracking the Latest AMI
To keep track of the latest validated AMI ID, it is a best practice to automatically store the output AMI ID in an [[AWS Systems Manager Parameter Store]] (SSM Parameter Store). Other services (like CloudFormation or Terraform) can then dynamically reference this parameter to always deploy using the latest image version.
