---
tags: [AWS, DevOps, DeveloperTools, CI-CD, Deployment, EC2]
---

# AWS CodeDeploy EC2

For deployments to an EC2 or On-Premises environment, it is **mandatory** to install and run the **CodeDeploy Agent** on the instance. The instance (via an IAM Role) needs permissions to read artifacts from [[Amazon S3]] and communicate with the [[AWS CodeDeploy]] service.

The deployment is driven by an `appspec.yml` file (always located at the root of the revision package).

Example `appspec.yml` for EC2:
```yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root
```

**Hook Execution Order (Without Load Balancer):**
1. ApplicationStop
2. DownloadBundle (System action, cannot have a custom hook)
3. BeforeInstall
4. Install (System action)
5. AfterInstall
6. ApplicationStart
7. ValidateService

**With Load Balancer (In-Place or Blue/Green):**
Additional traffic blocking and allowing events are added: `BeforeBlockTraffic`, `BlockTraffic`, `AfterBlockTraffic`, `BeforeAllowTraffic`, `AllowTraffic`, and `AfterAllowTraffic`.
