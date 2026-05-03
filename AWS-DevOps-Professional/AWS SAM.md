# AWS SAM (Serverless Application Model)

[[AWS SAM]] is an open-source framework designed to help build and deploy serverless applications on AWS. 

It provides a simplified way of defining serverless resources such as [[AWS Lambda]] functions, [[Amazon API Gateway]] APIs, and [[Amazon DynamoDB]] tables.

## How it Works
To use SAM, you create a SAM template. Behind the scenes, SAM transforms this template into a standard [[AWS CloudFormation]] template. During the deployment process, the packaged artifacts are stored in an [[Amazon S3]] bucket, and [[AWS CloudFormation]] is then used to provision the resources.

### SAM Accelerate (`sam sync`)
SAM Accelerate is a feature used to significantly reduce deployment latency, enabling faster feedback loops during serverless development.

## Example SAM Template
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: An example of a Serverless API

Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.handler
      Runtime: nodejs18.x
      CodeUri: ./hello-world/
      MemorySize: 128
      Timeout: 10
      Events:
        HelloWorld:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```
