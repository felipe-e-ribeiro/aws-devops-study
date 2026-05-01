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

## Advanced Features

### Rollbacks
CloudFormation automatically handles errors during stack operations. There are three main rollback behaviors:
- **Stack Creation Failure:** If a stack fails to create, CloudFormation automatically rolls back and deletes the resources created up to that point.
- **Stack Update Failure:** If a stack update fails, CloudFormation rolls back the stack to its previous known-good state.
- **Manual Rollback:** In cases where an update fails and automatic rollback is paused or fails, you can trigger a rollback manually.

### Service Role
By default, CloudFormation uses the permissions of the IAM user executing the stack operations. You can specify an **IAM Service Role** that CloudFormation assumes to create, update, or delete resources, allowing you to decouple user permissions from stack permissions.

### Capabilities
When a template contains resources that affect permissions or advanced capabilities, you must explicitly acknowledge them during stack creation/update:
- `CAPABILITY_IAM`: Acknowledges that the template will create or modify IAM resources (with auto-generated names).
- `CAPABILITY_NAMED_IAM`: Acknowledges that the template will create IAM resources with custom, specific names.
- `CAPABILITY_AUTO_EXPAND`: Required when the template uses macros (e.g., AWS SAM) or nested stacks that require expansion before processing.

### Deletion Policies
The `DeletionPolicy` attribute dictates what happens to a resource when the stack is deleted or when the resource is removed from the template:
- **Delete:** (Default) The resource is deleted. *Note: For Amazon S3 buckets, deletion will fail if the bucket is not empty.*
- **Retain:** The resource is kept in AWS, even though it's removed from CloudFormation's management.
- **Snapshot:** Creates a backup snapshot before deleting the resource (supported by resources like Amazon RDS, EBS, and ElastiCache).

### Stack Policies
A Stack Policy is a JSON document that defines what update actions can be performed on designated resources. This is crucial for protecting critical resources (like production databases) from unintentional updates or replacements during a stack update.

### Termination Protection
Termination Protection prevents a stack from being accidentally deleted. When enabled, any attempt to delete the stack via the console, CLI, or API will fail until the protection is explicitly disabled.

### Custom Resources
Custom Resources (`Custom::MyResource` or `AWS::CloudFormation::CustomResource`) allow you to write custom provisioning logic in templates that CloudFormation runs anytime you create, update, or delete stacks. They are commonly backed by [[AWS Lambda]] functions or [[Amazon SNS]] topics. 

*Example Use Case:* A Custom Resource backed by a Lambda function triggered during stack deletion to automatically empty an S3 bucket, ensuring the bucket can be deleted successfully.

```yaml
Parameters:
  S3BucketName:
    Type: String
    AllowedPattern: "[a-zA-Z][a-zA-Z0-9_-]*"

Resources:
  SampleS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref S3BucketName
    DeletionPolicy: Delete

  # The Custom Resource invoking the Lambda Function
  S3CustomResource:
    Type: Custom::S3CustomResource
    Properties:
      ServiceToken: !GetAtt AWSLambdaFunction.Arn
      bucket_name: !Ref SampleS3Bucket

  AWSLambdaFunction:
     Type: AWS::Lambda::Function
     Properties:
       FunctionName: !Sub '${AWS::StackName}-${AWS::Region}-lambda'
       Handler: index.handler
       Role: !GetAtt AWSLambdaExecutionRole.Arn
       Timeout: 360
       Runtime: python3.12
       Code:
         ZipFile: |
          import boto3
          import cfnresponse

          def handler(event, context):
              the_event = event['RequestType']        
              response_data = {}
              bucket_name = event['ResourceProperties']['bucket_name']
              
              try:
                  if the_event == 'Delete':
                      b_operator = boto3.resource('s3')
                      b_operator.Bucket(str(bucket_name)).objects.all().delete()
                  
                  cfnresponse.send(event, context, cfnresponse.SUCCESS, response_data)
              except Exception as e:
                  response_data['Data'] = str(e)
                  cfnresponse.send(event, context, cfnresponse.FAILED, response_data)

  AWSLambdaExecutionRole:
     Type: AWS::IAM::Role
     Properties:
       AssumeRolePolicyDocument:
         Statement:
         - Action: sts:AssumeRole
           Effect: Allow
           Principal:
             Service: lambda.amazonaws.com
       Policies:
       - PolicyName: !Sub ${AWS::StackName}-S3-CW
         PolicyDocument:
           Statement:
           - Action:
             - logs:CreateLogGroup
             - logs:CreateLogStream
             - logs:PutLogEvents
             - s3:PutObject
             - s3:DeleteObject
             - s3:List*
             Effect: Allow
             Resource: "*"
```

### Dynamic References
Dynamic references provide a compact, powerful way for you to reference external values that are supplied at runtime. They are critical for securely injecting secrets or fetching configuration data without hardcoding them in the template.

Common dynamic reference patterns:
- **[[AWS Systems Manager Parameter Store]] (SSM):** Fetch configuration values securely.
  `'{{resolve:ssm:parameter-name:version}}'`
- **SSM Secure String:** Fetch encrypted configuration values (supported only for specific resources).
  `'{{resolve:ssm-secure:parameter-name:version}}'`
- **[[AWS Secrets Manager]]:** Fetch passwords, API keys, or dynamically generated secrets.
  `'{{resolve:secretsmanager:secret-id:secret-string:json-key:version-stage:version-id}}'`

*Example Use Case:* Using AWS Secrets Manager to dynamically inject a database password into an RDS instance, while simultaneously configuring the RDS instance to automatically generate and manage that password:

```yaml
Resources:
  MyDatabaseSecret:
    Type: AWS::SecretsManager::Secret
    Properties:
      Name: "MyDatabaseSecret"
      GenerateSecretString:
        SecretStringTemplate: '{"username": "admin"}'
        PasswordLength: 16
        GenerateStringKey: "password"
        ExcludeCharacters: "\"@/\\\""

  MyDBInstance:
    Type: AWS::RDS::DBInstance
    Properties:
      Engine: mysql
      AllocatedStorage: 20
      DBInstanceClass: db.t3.micro
      MasterUsername: "{{resolve:secretsmanager:MyDatabaseSecret:SecretString:username}}" 
      MasterUserPassword: "{{resolve:secretsmanager:MyDatabaseSecret:SecretString:password}}"      

  # Attaches the DB details back to the secret (useful for rotation)
  SecretRDSAttachment:
    Type: AWS::SecretsManager::SecretTargetAttachment
    Properties: 
      SecretId: !Ref MyDatabaseSecret  
      TargetId: !Ref MyDBInstance   
      TargetType: AWS::RDS::DBInstance
```
*(Note: As an alternative to `resolve:secretsmanager`, you can set `ManageMasterUserPassword: true` directly on `AWS::RDS::DBCluster` or `AWS::RDS::DBInstance` to have RDS auto-create and manage the secret for you.)*

## Exam Tips
- **StackUpdates:** When you update a stack, understand the interruption behavior of the resources (e.g., `No Interruption`, `Some Interruption`, or `Replacement`). 
- **Change Sets:** Always use Change Sets to preview how proposed changes to a stack might impact your running resources before applying them.
- **Drift Detection:** CloudFormation Drift Detection lets you detect whether a stack's actual configuration differs, or has drifted, from its expected configuration defined in the template.
- **Nested Stacks:** Use nested stacks (via the `AWS::CloudFormation::Stack` resource) to overcome template size limits and to reuse common templates across multiple environments or stacks.
