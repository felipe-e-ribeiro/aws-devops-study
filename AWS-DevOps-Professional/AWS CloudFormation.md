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
- **Resources:** (Required) Specifies the stack resources and their properties, such as an [[Amazon EC2]] instance or an [[Amazon RDS]] database. All AWS resource types can be found in the [official documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html).
- **Parameters:** (Optional) Values passed to your template at execution time to customize your resources.
  - **Supported Types:** `String`, `Number`, `CommaDelimitedList`, `List`, `AWS-Specific Parameters` (e.g., `AWS::EC2::KeyPair::KeyName`), and `SSM Parameters` (e.g., `AWS::SSM::Parameter::Value<String>`).
  - **Common Properties:** `Description`, `ConstraintDescription`, `MinLength`/`MaxLength`, `MinValue`/`MaxValue`, `Default`, `AllowedValues`, `AllowedPattern`, and `NoEcho` (crucial for masking sensitive inputs like passwords).
- **Mappings:** (Optional) A mapping of keys and associated values that you can use to specify conditional parameter values (like an AMI ID per Region) using the `!FindInMap` intrinsic function.
- **Conditions:** (Optional) Statements that define when a resource is created or configured. Conditions are evaluated based on input parameters using intrinsic functions like `!Equals`, `!If`, `!Not`, `!And`, or `!Or`.
- **Outputs:** (Optional) Describes the values that are returned whenever you view your stack's properties. You can use the `Export` field to create a **Cross-Stack Reference**, allowing other stacks to use the output value via the `!ImportValue` intrinsic function.

### Pseudo Parameters
AWS provides built-in parameters that can be used anywhere in the template without declaring them:
- `AWS::AccountId`, `AWS::Region`, `AWS::StackName`, `AWS::StackId`, `AWS::NotificationARNs`, `AWS::NoValue`.

## Example Template (YAML)

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Template demonstrating Parameters, Mappings, Conditions, and Outputs
Parameters:
  EnvType:
    Type: String
    Default: development
    AllowedValues:
      - development
      - production
    Description: Defines the deployment environment
  DBPassword:
    Type: String
    NoEcho: true
    Description: Database master password

Mappings:
  RegionMap:
    us-east-1:
      AMI: "ami-0c55b159cbfafe1f0"
    us-west-2:
      AMI: "ami-0c55b159cbfafe1f0"

Conditions:
  CreateProdResources: !Equals [ !Ref EnvType, "production" ]

Resources:
  MyInstance:
    Type: 'AWS::EC2::Instance'
    Condition: CreateProdResources
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
      InstanceType: t2.micro
      Tags:
        - Key: Environment
          Value: !Ref EnvType

Outputs:
  InstanceId:
    Description: EC2 Instance ID
    Value: !Ref MyInstance
    Export:
      Name: !Sub "${AWS::StackName}-InstanceId"
```

## Intrinsic Functions

Intrinsic functions in CloudFormation help you assign values to properties that are not available until runtime. [Full reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)

### Common Functions
- **`!Ref` (Fn::Ref):** Returns the value of the specified parameter or resource (e.g., the ID of an EC2 instance).
- **`!GetAtt` (Fn::GetAtt):** Returns the value of an attribute from a resource in the template (e.g., fetching a `PrivateIp` or `PublicDnsName` from an EC2 instance).
- **`!Sub` (Fn::Sub):** Substitutes variables in an input string with values that you specify. Highly useful for constructing strings (e.g., `!Sub "${MyInstance.PublicDnsName}.example.com"`).
- **`!ImportValue` (Fn::ImportValue):** Returns the value of an output exported by another stack.
- **`!FindInMap` (Fn::FindInMap):** Returns the value corresponding to keys in a two-level map declared in the `Mappings` section.
- **`!Join` (Fn::Join):** Appends a set of values into a single value, separated by the specified delimiter.
- **`!Split` (Fn::Split):** Splits a string into a list of string values based on a specified delimiter.
- **`!Base64` (Fn::Base64):** Encodes a string in Base64 representation. Extremely common for encoding `UserData` scripts in EC2.
- **`!Cidr` (Fn::Cidr):** Returns an array of CIDR address blocks.
- **`!GetAZs` (Fn::GetAZs):** Returns an array that lists Availability Zones for a specified region.
- **`!Transform` (Fn::Transform):** Specifies a macro to perform custom processing on part of a stack template.

### Condition & List Functions
- **Conditionals:** `!If`, `!Not`, `!Equals`, `!And`, `!Or`. Used to evaluate logic inside the `Conditions` block or resource properties.
- **`!Length` (Fn::Length):** Returns the number of elements within an array/list (replaces the incorrect assumption of `!Cout`).
- **`!Contains` (Fn::Contains):** Evaluates if a list contains a specific value.

## Exam Tips
- **StackUpdates:** When you update a stack, understand the interruption behavior of the resources (e.g., `No Interruption`, `Some Interruption`, or `Replacement`). 
- **Change Sets:** Always use Change Sets to preview how proposed changes to a stack might impact your running resources before applying them.
- **Drift Detection:** CloudFormation Drift Detection lets you detect whether a stack's actual configuration differs, or has drifted, from its expected configuration defined in the template.
- **Nested Stacks:** Use nested stacks (via the `AWS::CloudFormation::Stack` resource) to overcome template size limits and to reuse common templates across multiple environments or stacks.
