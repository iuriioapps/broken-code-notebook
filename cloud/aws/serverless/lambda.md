# Lambda

AWS Lambda is a compute service where you can upload your code and create a Lambda function. AWS Lambda takes care of the provisioning and managing the servers that you use to run the code. You don't have to worry about operating systems, patching, scaling, etc.

You can use Lambda in the following ways:

* as an event-driven compute service where Lambda runs your code in response to events. These events could be changes in S3 or DynamoDB.
* as a compute service that runs your code in response to HTTP requests using API Gateway or API calls made using AWS SDK.

AWS Lambda natively supports Java, Go, PowerShell, NodeJS, C\#, Python, Ruby and provides runtime API which allows you to use any additional language.

Using Lambda with other services:

* Services that Lambda reads from:
  * Kinesis
  * DynamoDB
  * SQS
* Services that invoke Lambda synchronously:
  * ELB
  * Cognito
  * Lex
  * Alexa
  * API Gateway
  * CloudFront \(Lambda@Edge\)
  * Kinesis Data Firehose
  * Step Functions
* Services that invoke Lambda asynchronously:
  * S3
  * SNS
  * SES
  * CloudFormation
  * CloudWatch Logs
  * CloudWatch Events
  * CodeCommit
  * Config
  * IoT events

To improve performance, Lambda may choose to retain an instance of your function and reuse it to serve a subsequent request, rather that creating a new copy. Your code should not assume that this will always happen. Lambda code must be stateless. Keeping function stateless enables Lambda to rapidly launch as many copies of the function as needed.

### Limits

Following limits apply per region and can be increased:

| Description | Limit |
| :--- | :--- |
| Concurrent executions | 1000 |
| Function and layer storage | 75 Gb |

Per function limits, can not be changed:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Limit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Memory allocation</td>
      <td style="text-align:left">128 Mb - 3008 Mb, 64 Mb increment</td>
    </tr>
    <tr>
      <td style="text-align:left">Timeout</td>
      <td style="text-align:left">900 seconds</td>
    </tr>
    <tr>
      <td style="text-align:left">Environment variables</td>
      <td style="text-align:left">4 Kb</td>
    </tr>
    <tr>
      <td style="text-align:left">Resource-based policy</td>
      <td style="text-align:left">20 Kb</td>
    </tr>
    <tr>
      <td style="text-align:left">Layers</td>
      <td style="text-align:left">5</td>
    </tr>
    <tr>
      <td style="text-align:left">Burst concurrency</td>
      <td style="text-align:left">500 - 3000</td>
    </tr>
    <tr>
      <td style="text-align:left">Invocation frequency</td>
      <td style="text-align:left">
        <ul>
          <li>10x concurrent limit, synchronous, all sources</li>
          <li>10x concurrent limit, asynchronous, non-AWS</li>
          <li>unlimited, AWS sources</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Payload</td>
      <td style="text-align:left">
        <ul>
          <li>6 Mb, synchronous</li>
          <li>256 Kb, asynchronous</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deployment package</td>
      <td style="text-align:left">
        <ul>
          <li>50 Mb, zipped, direct upload</li>
          <li>250 Mb, unzipped, including layers</li>
          <li>3 Mb, console editor</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ENIs per VPC</td>
      <td style="text-align:left">160</td>
    </tr>
    <tr>
      <td style="text-align:left">Test events</td>
      <td style="text-align:left">10</td>
    </tr>
    <tr>
      <td style="text-align:left">/tmp storage</td>
      <td style="text-align:left">512 Mb</td>
    </tr>
    <tr>
      <td style="text-align:left">File descriptors</td>
      <td style="text-align:left">1024</td>
    </tr>
    <tr>
      <td style="text-align:left">exec processes / threads</td>
      <td style="text-align:left">1024</td>
    </tr>
  </tbody>
</table>

### Versioning and aliases

* By default, there is only one version of the code, referred to as `$LATEST`
* You can use versions to manage the deployment of your Lambda code. You can change the function code and settings only of the unpublished version of a function. When you publish a version, the code and most of the settings are locked.
* You can create one or more aliases for your Lambda function. An alias is like a pointer to a specific Lambda function version. You can access the function version using the alias ARN. Each alias has unique ARN. An alias can only point to a function version, not to another alias. You can update the alias to point to a new version of the function.
* An alias can be used to split traffic between two published versions of the function.

### Scalability and Availability

* Lambda is designed to use replication and redundancy for both the service itself and for the Lambda functions it operates.
* When you update the function, there will be a brief window of time, when requests could be served by either the old or the new version of your function.
* Lambda is designed to run many instances of your functions in parallel. However, it has a default safety throttle for number of concurrent executions per account per region.
* On exceeding the throttle limit, Lambda functions being invoked synchronously will return a throttling error \(HTTP 429 error code\). Lambda functions being invoked asynchronously can absorb reasonable bursts of traffic for approximately 15-30 minutes after which incoming events will be rejected and throttled.

### Error handling

* On failure, Lambda functions being invoked synchronously will respond with an exception. Lambda functions being invoked asynchronously are retried at least 3 times.
* On exceeding the retry policy for asynchronous invocations, you can configure a "dead letter queue"  \(DLQ\) into which the event will be placed; in absence of a configured DLQ the event may be rejected.

### VPC access

* You can configure a function to connect to private subnets in a VPC. Lambda creates an ENI \(Elastic Network Interface\) for each combination of security group and subnet in your function's VPC configuration in a process that can take some time. 
* Multiple functions connected to the same subnets share ENIs, so connecting additional functions is much quicker.
* By default, Lambda runs in a secure VPC with access to AWS services and the internet. When you connect a function to VPC in your account, it does not have access to the internet, unless the VPC provides access.

Monitoring

* Lambda monitors functions on your behalf and sends metrics to CloudWatch.
* The `AWS/Lambda` namespace includes the following metrics:
  * **Invocations** - measures the number of times a function is invoked in response to an event or API call.
  * **Errors** - measures the number of invocations that failed due to errors in the function.
  * **DeadLetterErrors** - incremented when Lambda is unable to write the failed event payload to your configured DLQ.
  * **Duration** - measures function execution time.
  * **Throttles** - measures the number of invocation attempts that were throttled due to invocation rates exceeding the concurrent limit.
  * **ConcurrentExecutions** - emitted as an aggregate for all functions in the account. Measures sum of concurrent executions for a given function at a given point in time.
  * **UnreservedConcurrentExecutions** - emitted as an aggregate metric for all functions in the account only. Represents the sum of the concurrency of the functions that don't have a custom concurrency limit specified.

### Compliance

* Lambda is SOC, HIPAA, PCI and ISO compliant.

### Lambda@Edge

* Lets you run Lambda functions to customize content that CloudFront delivers, executing the functions in AWS locations closer to the viewer. The functions run in response to CloudFront events, without provisioning or managing servers.
* You can use Lambda functions to change CloudFront responses at the following points:
  * After CloudFront receives a request from a viewer.
  * Before CloudFront forwards request to the origin.
  * After CloudFront receives the response from the origin.
  * Before CloudFront forwards the response to the viewer.

### Lambda with DynamoDB streams

* DynamoDB is integrated with Lambda so that you can create triggers. With triggers you can react to data modifications in DynamoDB tables.
* After you enable DynamoDB streams on a table, associate the DynamoDB table with Lambda function. Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records.
* Configure the StreamSpecification you want for your DynamoDB streams:
  * StreamEnabled - whether or not stream is enabled.
  * StreamViewType - when an item in the table is modified, StreamViewType is determines what information is written to the stream for this table. Valid values for the StreamViewType are:
    * KEYS\_ONLY - only the key attributes of the modified items are written to the stream.
    * NEW\_IMAGE - the entire item, as it appears after it was modified, is written to the stream.
    * OLD\_IMAGE - the entire item, as it appears before it was modified, is written to the stream.
    * NEW\_AND\_OLD\_IMAGE - both the new and the old item images of the item are written to the stream. 



