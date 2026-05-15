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

## Deployments and Stages
- **Stage Deployment:** You can deploy multiple versions of your API to different stages (e.g., `dev`, `test`, `prod`) to safely manage releases without causing access issues.
- **Stage Variables:** Act like environment variables for your API stages. They can be used to dynamically route requests. For instance, using `${stageVariables.variableName}`.
- **Routing to Lambda Aliases:** You can combine API Gateway Stage Variables with [[AWS Lambda]] Aliases to dynamically route traffic to specific function versions based on the API stage (e.g., `prod` stage points to `PROD` alias, while `dev` points to `$LATEST`).
- **Canary Deployments:** API Gateway allows you to configure canary settings for a stage. When deploying a new version, you can route a percentage of traffic to the canary release. Once verified, you can **Promote Canary** to make it the primary version.

## API Caching
- Caching is defined at the **Stage** level and can significantly improve latency.
- The default Time to Live (TTL) is 300 seconds (configurable from 0 to 3600 seconds).
- **Overrides:** You can override cache settings on a per-method basis.
- **Capacity:** Cache capacity ranges from 0.5 GB to 237 GB.
- **Cache Invalidation:**
    - You can flush the cache manually via the AWS Management Console.
    - Clients can invalidate the cache by sending the header `Cache-Control: max-age=0` (requires the client to have the correct IAM permissions).

## Monitoring & CloudWatch Metrics
- [[Amazon CloudWatch]] metrics and logging can be enabled globally or overridden on a per-method basis.
- **Key Metrics:**
    - `CacheHitCount` & `CacheMissCount`: Useful for measuring the effectiveness of your API cache.
    - `Count`: The total number of API requests received in a given period.
    - `IntegrationLatency`: The time taken by the backend (e.g., Lambda) to process the request and return the response to API Gateway.
    - `Latency`: The total time between API Gateway receiving the client's request and returning the response to the client.

## Throttling and Quotas
- API Gateway enforces a soft limit throttle of **10,000 requests per second (rps)** across all APIs within an AWS account.
- **Important:** Since this is an account-level limit, if one API receives a massive spike and hits the limit, it can negatively impact other APIs in the same account (similar to Lambda concurrency limits).
- If the limit is exceeded, clients will receive an HTTP `429 Too Many Requests` error.
- **Mitigation:**
    - You can configure Stage limits and Method limits to protect your backend and ensure fair usage.
    - Implement **Usage Plans** to define request limits and quotas on a per-client basis.

## Common HTTP Error Codes
- **4xx (Client Errors):**
    - `400 Bad Request`: Generic client-side error.
    - `403 Access Denied`: Can occur due to IAM permission issues or if blocked by [[AWS WAF]].
    - `429 Too Many Requests`: API quota or throttling limit exceeded.
- **5xx (Server Errors):**
    - `502 Bad Gateway`: Bad response from the backend integration.
    - `503 Service Unavailable`: Backend service is down.
    - `504 Gateway Timeout`: Integration failure. Occurs if the backend takes longer than the **29-second maximum timeout** limit.
