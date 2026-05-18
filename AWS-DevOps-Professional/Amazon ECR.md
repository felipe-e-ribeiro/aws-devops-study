# Amazon ECR

Amazon Elastic Container Registry (ECR) is a container image repository service in AWS.

- Supports storing both private and public repositories.
- Fully integrated with [[Amazon ECS]].
- Supports vulnerability scanning, image tagging, lifecycle policies, etc.

## Lifecycle Policies

Lifecycle rules can be applied to manage old images, images that haven't been used for a certain number of days, or based on image count.

*Example of a lifecycle policy:*

```json
{
    "rules": [
        {
            "rulePriority": 1,
            "description": "Expire untagged images older than 14 days",
            "selection": {
                "tagStatus": "untagged",
                "countType": "sinceImagePushed",
                "countUnit": "days",
                "countNumber": 14
            },
            "action": {
                "type": "expire"
            }
        },
        {
            "rulePriority": 2,
            "description": "Keep only the 30 most recent images tagged with 'latest' or 'release'",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["latest", "release"],
                "countType": "imageCountMoreThan",
                "countNumber": 30
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```
