# AWS Lambda

[[AWS Lambda]] is a serverless compute service that allows you to run code without provisioning or managing servers, and it can be executed directly from the AWS Management Console.

## Key Features & Exam Tips

- **Versioning and Aliases (Tags):** You can publish versions of your Lambda functions and assign aliases (effectively acting as pointers or tags to specific versions).
- **Deployment Strategies:** Using version aliases allows you to implement advanced deployment methods such as **blue/green** or **canary** deployments. You can route a specific percentage of traffic to version X and the remainder to version Y (e.g., using [[AWS CodeDeploy]]).
- **Environment Variables:** Lambda supports environment variables. For secure storage, it integrates seamlessly with [[AWS Systems Manager Parameter Store]] and [[AWS Secrets Manager]].
- **Concurrency Limits:** By default, there is a maximum concurrency limit of 1,000 concurrent executions per AWS region across all functions in an account (the note specifies 2,000, which may represent an account-specific or increased limit quota).
    - **Reserved Concurrency:** You can allocate a specific amount of reserved concurrency to a single function. However, this reservation is subtracted from the total available account concurrency limit, guaranteeing that capacity for the function while restricting other functions from using it.
- **Storage Options:** Lambda functions can utilize different storage mechanisms:
    - **Amazon EFS:** For persistent, shared file storage.
    - **Amazon S3:** For object storage.
    - **Lambda Layers:** For sharing code, libraries, or other dependencies across multiple Lambda functions.
    - **Ephemeral Storage (`/tmp`):** Up to 10 GB of temporary storage per execution environment.
