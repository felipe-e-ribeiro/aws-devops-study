---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Deployment, ECS]
---

# AWS CodeDeploy ECS

In [[Amazon ECS]], [[AWS CodeDeploy]] does not use an agent because the ECS platform natively manages the containers. CodeDeploy orchestrates the deployment by creating a new Task Set (new environment) and adjusting the Application Load Balancer.

**ECS deployment only supports the Blue/Green strategy.**

The `appspec.yml` for ECS is different from EC2, focusing on referencing the Task Definition and Load Balancer information.

Example `appspec.yml` for ECS:
```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:us-east-1:123456789012:task-definition/my-task:2"
        LoadBalancerInfo:
          ContainerName: "my-container"
          ContainerPort: 80
```

**ECS Blue/Green Traffic Strategies:**
1. **Canary**: Shifts X% of traffic to the new version. If successful (after a specified wait time), it shifts the rest (e.g., `Canary10Percent5Minutes`).
2. **Linear**: Shifts X% of traffic every Y minutes until it reaches 100% (e.g., `Linear10PercentEvery1Minute`).
3. **AllAtOnce**: Shifts 100% of traffic instantly.

**Hooks in ECS:**
ECS hooks are executed by invoking **[[AWS Lambda]] Functions**, not by running scripts on a machine.
The available lifecycle events to trigger Lambda functions are:
1. `BeforeInstall`
2. `AfterInstall`
3. `AfterAllowTestTraffic`
4. `BeforeAllowTraffic`
5. `AfterAllowTraffic`
