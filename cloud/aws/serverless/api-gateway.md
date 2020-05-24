# API Gateway

API Gateway is fully managed service that makes it easy for developers to publish, maintain, monitor and secure API at any scale.

What API Gateway can do:

* expose an HTTPS endpoint to define a RESTful API.
* connect to services like DynamoDB and Lambda.
* Send each API endpoint to a different target.
* Scale and run efficiently with low cost.
* Track and control use by API key.
* Throttle requests to prevent attacks.
* Maintain multiple versions of the API.

### Concepts

* API deployment - a point-in-time snapshot of your API Gateway resources and methods. To be available to the clients to use, the deployment must be associated with one or more API stages.
* API endpoint - hostnames APIs in API Gateway, which are deployed to a specific region, and of the format: `{rest-api-id}.execute-api.{region}.amazonaws.com`
* API key - an alphanumeric string that API Gateway uses to identify app developer who uses your API.
* API stage - a logical reference to a lifecycle state of your API. API stages are identified by API ID and stage name.
* Model - data schema specifying the data structure of a request or response payload.
* Private API - an API that is exposed through interface VPC endpoints and isolated from the public internet.
* Private integration - an API Gateway integration type for a client to access resources inside a customer's VPC through a private VPC endpoint without exposing resources to the public internet.
* Proxy integration - you can setup a proxy integration as an HTTP proxy integration type or a Lambda proxy integration type:
  * For HTTP proxy integration, API Gateway passes the entire request and response between frontend and HTTP backend.
  * For Lambda proxy integration, API gateway sends the entire request as an input to a backend Lambda function.
* Usage plan - provides selected API clients with access to one or more deployed APIs. You can use usage plan to configure throttling and quota limits, which are enforced on individual client API keys.

### API endpoint types

* **Edge optimized endpoint**: the default hostname of an API Gateway API that is deployed to the specified region while using a CloudFront distribution to facilitate client access typically from across AWS regions. API requests are routed to the nearest CloudFront Point of Presence.
* **Regional endpoint:** the hostname of an API that is deployed to the specified region and intended to serve clients, such as EC2 instances, in the same AWS region. API requests are targeted directly to the region-specific API Gateway without going through the CloudFront distribution. You can apply latency-based routing on regional endpoints to deploy API to multiple regions using the same regional API endpoint configuration, set the same custom domain name for each deployed API and configure latency-based DNS records in Route53 to route client requests to the region that has the lowest latency.
* **Private endpoint:** allows the client to securely access private API resources inside the VPC. Private APIs are isolated from the public internet, and they can only be accessed using VPC endpoints for API Gateway that have been granted access.

### Features

* API Gateway can execute Lambda code in your account, start Step Function state machines, or make calls to Beanstalk, EC2 or web services outside AWS with publicly accessible HTTP endpoints.
* API Gateway helps you define plans that meter and restrict third-party developer access to your API.
* API Gateway helps you manage traffic to your backend systems by allowing you to set throttling rules based on the number of requests per second for each HTTP method in your APIs.
* You can setup a cache with customizable keys and time-to-live \(TTL\) in seconds for your API data to avoid hitting your backend services for each request.
* API Gateway lets you make multiple versions of the same API simultaneously with API Lifecycle.
* After you build, test and deploy your APIs, you can package them in an API Gateway usage plan and sell the plan as SaaS product through AWS Marketplace.
* API Gateway offers the ability to create, update and delete documentation associated with each portion of your API, such as methods and resources.
* All of the APIs created expose HTTPS endpoints only. API Gateway does not support unencrypted HTTP endpoints.

### Monitoring

* API Gateway is integrated with CloudWatch, so you get backend performance metrics, such as API calls, latency and error rates.
* You can setup a custom alarm on API Gateway APIs.
* API Gateway can also log API execution errors to CloudWatch Logs

### Security

* To authorize and verify API requests to AWS services, API Gateway can help you leverage sigv4. Using sigv4 authentication, you can use IAM and access policies to authorize access to your APIs and all your other AWS resources.
* You can enable WAF \(Web Application Firewall\) for your APIs in API Gateway, making it easier to protect your APIs against common web exploits.

### Pricing

* You pay only for the API calls you receive and the amount of data transferred out.
* API Gateway also provides optional data caching charged at an hourly rate that varies based on the cache size you select.

### Limits

| Description | Limit |
| :--- | :--- |
| Throttle limit, per account per region. With additional burst capacity provided by the token bucket algorithm, using a maximum bucket capacity of 5000 requests. Can be increased | 10000 requests/sec |
| Maximum number of regional APIs per account per region | 600 |
| Maximum number of private APIs per account per region | 600 |
| Maximum number of Edge-Optimized APIs per account per region | 120 |
| Maximum number of stages per API | 10 |
| Header value size | 10 Kb |
| Payload size | 10 Mb |

