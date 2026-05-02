---
tags:
  - aws
  - devops
  - compute
---
# AWS Elastic Beanstalk

[[AWS Elastic Beanstalk]] allows you to assign automatic deployments where you can control resources such as [[Amazon EC2]], [[Application Load Balancer]], [[Amazon EC2 Auto Scaling]], [[Amazon RDS]], etc.

## Components
- **Environments**: A collection of AWS resources running the application. It is possible to have multiple environments (e.g., prod, stg, dev, etc.), but only one version can be active per environment at a time.
- **Versions**: The active version of the resource, which can be controlled by an [[Amazon S3]] bucket, for example.
- **Configurations**: A set of structural definitions that dictate the environment's setup, such as [[Amazon EC2 Auto Scaling]] configurations, instances, etc.

## Tiers
There are two distinct tier types:
- **Web Server Tier**: Used when an application is deployed and exposed to the internet. It typically utilizes an [[Application Load Balancer]] or a public endpoint to consume requests.
- **Worker Tier**: Used when the resource is controlled by a message queue, such as [[Amazon SQS]]. The workload is not publicly exposed; instead, it responds to the message broker.

## Deployment Modes
- **Single Instance**: A single deployment instance. Recommended only for development environments.
- **High Availability**: Utilizes multiple nodes, configuring an [[Amazon EC2 Auto Scaling]] group and an [[Application Load Balancer]] to manage the load. Used in production environments.

## Deployment Policies / Types of Updates

*Information can be found at: [Deploying existing versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html)*

**Table 1: Deployment Policies and Supported Environment Types**

| Deployment policy | Load-balanced environments | Single-instance environments | Legacy Windows Server environments† |
| --- | --- | --- | --- |
| **All at once** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Rolling** | ✅ Yes | ❌ No | ✅ Yes |
| **Rolling with an additional batch** | ✅ Yes | ❌ No | ❌ No |
| **Immutable** | ✅ Yes | ✅ Yes | ❌ No |
| **Traffic splitting** | ✅ Yes ([[Application Load Balancer]]) | ❌ No | ❌ No |

*† A Legacy Windows Server environment refers to an environment based on a Windows Server platform configuration utilizing an IIS version earlier than IIS 8.5.*

---

**Table 2: Comparison of Deployment Methods**

| **Method**                           | **Impact of failed deployment**                                                    | **Deploy time**      | **Zero downtime** | **No DNS change** | **Rollback process**                        | **Code deployed to**       |
| ------------------------------------ | ---------------------------------------------------------------------------------- | -------------------- | ----------------- | ----------------- | ------------------------------------------- | -------------------------- |
| **All at once**                      | Downtime                                                                           | ⏱️ (Fast)            | ❌ No              | ✅ Yes             | Manual redeploy                             | Existing instances         |
| **Rolling**                          | Single batch out of service; successful batches before failure run the new version | ⏱️⏱️ ⏱️(Slower)†     | ✅ Yes             | ✅ Yes             | Manual redeploy                             | Existing instances         |
| **Rolling with an additional batch** | Minimal if the 1st batch fails; otherwise, similar to Rolling                      | ⏱️⏱️⏱️ (Slower)†     | ✅ Yes             | ✅ Yes             | Manual redeploy                             | New and existing instances |
| **Immutable**                        | Minimal                                                                            | ⏱️⏱️⏱️⏱️ (Slowest)   | ✅ Yes             | ✅ Yes             | Terminate new instances                     | New instances              |
| **Traffic splitting**                | Percentage of traffic routed to the new version is temporarily impacted            | ⏱️⏱️⏱️⏱️ (Slowest)†† | ✅ Yes             | ✅ Yes             | Reroute traffic and terminate new instances | New instances              |
| **Blue/green**                       | Minimal                                                                            | ⏱️⏱️⏱️⏱️ (Slowest)   | ✅ Yes             | ❌ No              | Swap URL/CNAME ([[Amazon Route 53]])        | New instances              |

*† Deployment time varies depending on the configured batch size.*
*†† Deployment time varies depending on the configured traffic evaluation time.*

## Exam Tips
- For zero downtime deployments, prefer **Immutable** updates or **Blue/green** deployments. 
- Use **Traffic splitting** for canary testing where a small percentage of traffic is directed to the new version before fully committing.
- Understand that **All at once** deployments will always cause downtime, making it unsuitable for production environments.
