# Amazon ECS

You can deploy applications using two compute options:
- [[Amazon EC2]]
- [[AWS Fargate]]

## IAM Roles for ECS

### EC2 Instance Profile / EC2 Launch Type
- Used by the ECS Agent.
- Makes API calls to ECS services.
- Used to pull images from [[Amazon ECR]].
- References information from [[AWS Secrets Manager]] or [[AWS Systems Manager Parameter Store]].

### ECS Task Role
- A specific role can be created per task.
- This allows each role to be attached to different services securely.

## Load Balancer Integration

Load Balancer integration is supported, which is especially useful when there are multiple endpoints within a given service.
Supported types:
- [[Application Load Balancer]] (ALB)
- Network Load Balancer (NLB)

## Data Persistence

For data persistence, [[Amazon EFS]] is commonly used to provide network-level storage so multiple endpoints can read and write data concurrently.

## Auto Scaling

ECS supports auto-scaling to increase or decrease the number of nodes in the environment. Supported metrics include:
- ECS Service Average CPU utilization
- ECS Service Average Memory utilization
- ALB Request Count Per Target

You can also configure:
- **Target Tracking**: Uses the metrics above.
- **Step Scaling**: Scales based on [[Amazon CloudWatch]] alarms.
- **Schedule Scaling**: Scales based on date/time.

## ECS for Background Processing

ECS can be used for processing steps within a workflow, similar to [[AWS Lambda]], but is typically used for longer-running or larger processing tasks.

*Example:* 
When an object is created in an [[Amazon S3]] bucket, an event can be routed to [[Amazon EventBridge]], which then triggers a "Run ECS Task" request. This task spins up a container, processes the data, saves the results to [[Amazon DynamoDB]] or another appropriate destination, and finishes the process.

## Logs with awslogs Driver

### AWS Fargate
If using Fargate, you simply need to configure `logConfiguration` within the Launch Type definitions.

### EC2 Launch Type
- Prevents logs from taking up space inside the container.
- Uses CloudWatch Unified Agents and ECS Container Agents.
- Enabled by setting `ECS_AVAILABLE_LOGGING_DRIVERS` in `/etc/ecs/ecs.config`.
- The EC2 instance must have IAM permissions to forward logs to [[Amazon CloudWatch]].

Alternatively, a sidecar container can be used to handle logs and forward them to [[Amazon CloudWatch]] Logs.
