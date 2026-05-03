# AWS CDK (Cloud Development Kit)

The [[AWS CDK]] is an open-source software development framework that allows you to define cloud infrastructure as code (IaC) using familiar programming languages (e.g., TypeScript, Python, Java).

## Key Comparisons

### CDK vs. SDK
- **[[AWS CDK]]**: Used to define and provision infrastructure (IaC).
- **[[AWS SDK]]**: Used for point-in-time programmatic interaction with AWS cloud resources (e.g., getting an object from S3 or invoking a Lambda function).

### CDK vs. SAM
- **[[AWS CDK]]**: Can be used to provision any AWS resource, not just serverless components.
- **[[AWS SAM]]**: Specifically optimized and tailored for building serverless applications.

Both CDK and SAM ultimately synthesize down to [[AWS CloudFormation]] templates for deployment.
