# Amazon API Gateway

[[Amazon API Gateway]] is a fully managed serverless service that makes it easy for developers to create, publish, maintain, monitor, and secure REST, HTTP, and WebSocket APIs at any scale.

## Integrations
API Gateway can route requests to various backends:
- **[[AWS Lambda]] Functions:** Invokes a Lambda function to process the request.
- **HTTP Endpoints:** Routes requests to any HTTP endpoint, including on-premises services.
- **AWS Services:** Directly integrates with other AWS services (e.g., [[AWS Step Functions]], [[Amazon SQS]], [[Amazon DynamoDB]]).

## Endpoint Types
- **Edge-Optimized:**
    - Requests are routed through [[Amazon CloudFront]] Edge Locations to provide low latency for globally distributed clients.
    - The API Gateway itself still resides in a specific AWS region.
- **Regional:**
    - Designed for clients in the same region as the API.
    - Can be manually combined with your own [[Amazon CloudFront]] distribution to provide more granular control over caching and routing.
- **Private:**
    - Accessible only from within an [[Amazon VPC]] using an interface VPC endpoint (ENI).

## Security & Authentication
- **IAM Roles:** Typically used for internal authentication (e.g., cross-service or cross-account access within AWS).
- **[[Amazon Cognito]]:** Used for authenticating external users (User Pools).
- **Custom Authorizers (Lambda Authorizers):** Uses an [[AWS Lambda]] function to implement custom authentication logic (e.g., verifying OAuth/SAML tokens).

## Custom Domain Names and ACM
When using custom HTTP domain names alongside [[AWS Certificate Manager]] (ACM):
- If using an **Edge-Optimized** endpoint, the ACM certificate MUST be located in the `us-east-1` (N. Virginia) region.
- If using a **Regional** endpoint, the ACM certificate can be in the specific region where the API is deployed.
- You must configure a CNAME or an Alias (A) record in [[Amazon Route 53]] pointing to the API Gateway domain.

## Exam Tips
- **Timeout:** The maximum integration timeout for an API Gateway request is **29 seconds**. If the backend (e.g., Lambda) takes longer, API Gateway will return an HTTP 504 Gateway Timeout error.
