# Amazon EKS

There are three different types of Amazon EKS nodes:

## Node Types

- **Managed Nodes**:
  - Nodes created by AWS ([[Amazon EC2]] instances) but managed by you.
  - These nodes are part of an Auto Scaling Group (ASG) managed by EKS.
  - Supports On-Demand or Spot instances.

- **Self-Managed Nodes**:
  - Nodes created by you, registered to the EKS cluster, and managed by your own ASG.
  - You can use the Amazon EKS Optimized AMI.
  - Supports On-Demand or Spot instances.

- **[[AWS Fargate]]**:
  - Serverless compute for containers.
  - No node management required; the node layer is fully managed by AWS.

## Supported Storage

Amazon EKS supports the following storage solutions:
- [[Amazon EBS]]
- [[Amazon EFS]]
- [[Amazon FSx for Lustre]]
- [[Amazon FSx for NetApp ONTAP]]

## Logging and Monitoring

The EKS control plane has built-in integration with [[Amazon CloudWatch]], allowing for a steady flow of logs.

For Worker nodes:
- If you enable the CloudWatch Agent, it will only forward standard CloudWatch metrics.
- You must enable **Fluent Bit** or **Fluentd** to forward container logs to [[Amazon CloudWatch]].
- Once configured, you can use [[CloudWatch Container Insights]] to access detailed dashboards and monitoring solutions for nodes, pods, tasks, and services.
