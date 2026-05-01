---
tags: [AWS, DevOps, IaC, Provisioning, Management]
---

# AWS CloudFormation

[[AWS CloudFormation]] is a service that allows you to use Infrastructure as Code (IaC) to model, provision, and manage AWS and third-party resources. 

To configure and deploy infrastructure, you use a template (written in YAML or JSON) that declares the AWS resources you want to create and configure. When you submit this template, CloudFormation provisions the defined resources together as a single unit called a **Stack**. Behind the scenes, the template is uploaded to an [[Amazon S3]] bucket which CloudFormation uses as a reference during the stack creation.

## CloudFormation Template Anatomy

A standard CloudFormation template consists of several key components:

- **AWSTemplateFormatVersion:** (Required) Specifies the AWS CloudFormation template version (e.g., `2010-09-09`).
- **Description:** (Optional) A text string that describes the template.
- **Parameters:** (Optional) Values passed to your template at execution time (when creating or updating a stack) to customize your resources.
- **Mappings:** (Optional) A mapping of keys and associated values that you can use to specify conditional parameter values, similar to a lookup table.
- **Conditions:** (Optional) Conditions that control whether certain resources are created or whether certain resource properties are assigned a value during stack creation or update.
- **Resources:** (Required) Specifies the stack resources and their properties, such as an [[Amazon EC2]] instance or an [[Amazon RDS]] database.
- **Outputs:** (Optional) Describes the values that are returned whenever you view your stack's properties.

## Example Template (YAML)

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple template to provision an EC2 instance
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
  InstanceName:
    Type: String
    Default: MyInstance
  Env:
    Type: String
    Default: dev
    AllowedValues:
      - Production
      - dev
      - staging
      
Mappings:
  RegionMap:
    us-east-1:
      AMI: "ami-0c55b159cbfafe1f0"
    us-west-2:
      AMI: "ami-0c55b159cbfafe1f0"

Conditions:
  "IsProduction": { "Fn::Equals": [ !Ref Env, "Production" ] }

Resources:
  MyInstance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
      InstanceType: !If [IsProduction, "t3.medium", "t3.nano"]
      Tags:
        - Key: Name
          Value: !Ref InstanceName
        - Key: Env
          Value: !Ref Env

Outputs:
  InstanceId:
    Description: EC2 Instance ID
    Value: !Ref MyInstance
  PublicIp:
    Description: Public IP address of the EC2 instance
    Value: !GetAtt MyInstance.PublicIp
```

## Exam Tips
- **StackUpdates:** When you update a stack, understand the interruption behavior of the resources (e.g., `No Interruption`, `Some Interruption`, or `Replacement`). 
- **Change Sets:** Always use Change Sets to preview how proposed changes to a stack might impact your running resources before applying them.
- **Drift Detection:** CloudFormation Drift Detection lets you detect whether a stack's actual configuration differs, or has drifted, from its expected configuration defined in the template.
- **Nested Stacks:** Use nested stacks (via the `AWS::CloudFormation::Stack` resource) to overcome template size limits and to reuse common templates across multiple environments or stacks.
