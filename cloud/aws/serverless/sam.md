---
description: Serverless Application Model
---

# SAM

* An open source framework for building serverless applications.
* It provides shorthand syntax to express functions, APIs, databases, and event source mappings.
* You create JSON or YAML configuration template to model your application.
* During deployment, SAM transforms and expands the SAM syntax into CloudFormation template. Any resource that you can declare in a CloudFormation template, you can also declare in a SAM template.
* The SAM CLI provides Lambda-like execution environment that lets you locally build, test and debug applications defined by SAM templates. You can also use SAM CLI to deploy your applications to AWS.
* You can use SAM to build serverless applications that use any runtime supported by AWS Lambda. You can also use SAM CLI to locally debug functions written in NodeJS, Java, Python and Go.

### Syntax

* `AWS::Serverless::Api`
  * this resource type describes an API Gateway resource. It's useful for advanced use cases where you want full control and flexibility when you configure your APIs.
* `AWS::Serverless::Application`
  * this resource type embeds a serverless application from AWS Serverless Application Repository or from an S3 bucket as a nested resource. Nested applications deployed as a nested stacks, which can contain multiple other resources.
* `AWS::Serverless::Function`
  * this resource type describes configuration information for creating a Lambda function. You can describe any event source that you want to attach to the Lambda function - such as S3, DynamoDB Streams, and Kinesis Data Streams.
* `AWS::Serverless::LayerVersion`
  * this resource type creates Lambda layer version that contain libraries or runtime code needed by a Lambda function. When a serverless layer version is transformed, SAM also transforms the logical ID of the resource so that old layer versions are not automatically deleted by CloudFormation when the resource is updated.
* `AWS::Serverless::SimpleTable`
  * this resource type provide simple syntax for describing how to create DynamoDB tables.
  * The optional `Transform` section of a CloudFormation template specifies one or more macros that CloudFormation uses to process your template. Aside from macros you create, CloudFormation is also supports `AWS::Serverless` transform, which is a macro hosted on CloudFormation. The `AWS::Transform` specifies the version of SAM to use. This model defines the SAM syntax that you can use and how CloudFormation process it.

### Common CLI commands

* `sam init` generates pre-configured SAM templates.
* `sam local` supports local invocation and testing of your Lambda functions and SAM-based serverless applications by executing your function code locally in a Lambda-like execution environment.
* `sam package` and `sam deploy` commands let you bundle your application code and dependencies into a deployment package and then deploy your serverless application to the AWS.
* `sam logs` command enables you to fetch, tail and filter logs for Lambda functions.
* The output of the `sam publish` command includes a link to the AWS Serverless Application Repository directly to your application.

### Controlling access to APIs

* You can use SAM to control who can access your API Gateway APIs by enabling authorization within your SAM template.
* **A Lambda authorizer** \(formerly known as custom authorizer\) is a Lambda function that you provide to control access to your API. When your API is called, this function is called with request context or an authorization token that are provided by the client application. The Lambda function returns a policy document that specify the operations that the caller is authorized to perform, if any. There are two types of authorizers:
  * Token based type receives the caller identity in a bearer token, such as JWT or OAuth.
  * Request parameter based type receives the caller identity in a combination of headers, query string parameters, stageVariables and $context variables.
* **Cognito user pools** or user directories in Cognito. A client of your application must first sign a user in to the user pool and obtain an identity or access token for the user. Then your API is called with one of the returned tokens. The API call succeeds only if the required token is valid.

## Installation

* Create account
* Configure IAM permissions - requires a user with admin credentials and programmatic access
* Install Docker
* Install Homebrew
* Install AWS SAM CLI

AWS Jenkins plugin [https://plugins.jenkins.io/aws-sam/](https://plugins.jenkins.io/aws-sam/)

AWS Toolkit for JetBrains [https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)

## Setting up AWS Credentials

* With AWS CLI `aws configure` command
* Manually by editing `~/.aws/credentials file`

  `[default]`

  `aws_access_key_id = your_access_key_id`

  `aws_secret_access_key = your_secret_access_key`

* Through environment variables

  `export AWS_ACCESS_KEY_ID=your_access_key_id`

  `export AWS_SECRET_ACCESS_KEY=your_secret_access_key`

### Getting started commands:

Step 1 - Initializing application

`sam init`

Step 2 - Build your application

`sam build`

Step 3 - Deploy your application

`sam deploy --guided`

### samconfig.toml

This file contains all the information about where and how to deploy the app. Example:

```bash
version = 0.1
[default]
[default.deploy]
[default.deploy.parameters]
stack_name = "my-serverless-app" <-- Stack name in CloudFormation
s3_bucket = "mybucket-serverless-deploy" <-- Target bucket, preferably with versioning enabled
s3_prefix = "my-serverless-app" <-- Subfolder for deployed application
region = "us-west-2"
profile = "<< IAM profile with programmatic access and necessary permissions >>"
confirm_changeset = true
capabilities = "CAPABILITY_IAM"

```

### Running SAM application locally

```bash
sam local start-api -p 9500 --skip-pull-image --region=us-west-2
```

This will start a service locally on port 9500, and will not check for latest Docker image \(this will speed up invocations, but Lambda Docker image must be present on the machine it's running on\). Service supports hot reload. Additional configuration can be found by running:

`sam local start-api --help`

### Making One-off Invocations

`sam local invoke "HelloWorldFunction" -e events/event.json`

where "HelloWorldFunction" is the name of the function as it is defined in template.yaml "Resources" section.

### Generating sample events

`sam local generate-event apigateway aws-proxy --body "" --path "hello" --method GET > api-event.json`

You can use this command to generate sample payloads from different event sources such as S3, API Gateway, and SNS. These payloads contain the information that the event sources send to your Lambda functions. 

Generate the event that S3 sends to your Lambda function when a new object is uploaded

`sam local generate-event s3 [put/delete]`

You can even customize the event by adding parameter flags. To find which flags apply to your command, run:

`sam local generate-event s3 [put/delete] --help`

Then you can add in those flags that you wish to customize using

 `sam local generate-event s3 [put/delete] --bucket <bucket> --key <key>`

 After you generate a sample event, you can use it to test your Lambda function locally

`sam local generate-event s3 [put/delete] --bucket <bucket> --key <key> | sam local invoke <function logical id>`

Supported services:

* alexa-skills-kit
* alexa-smart-home
* apigateway
* batch
* cloudformation
* cloudfront
* cloudwatch
* codecommit
* codepipeline
* cognito
* config
* connect
* dynamodb
* kinesis
* lex
* rekognition
* s3
* sagemaker
* ses
* sns
* sqs
* stepfunctions

### Deleting the deployment

* from Console, delete the stack
* using "`aws cloudformation delete-stack`" command

## AWS SAM Specification

You use the AWS SAM specification to define your serverless application. AWS SAM templates are an extension of AWS CloudFormation templates, with some additional components that make them easier to work with.

The primary differences between AWS SAM templates and AWS CloudFormation templates are the following:

* **Transform declaration**. The declaration Transform: AWS::Serverless-2016-10-31 is required for AWS SAM templates. This declaration identifies an AWS CloudFormation template as an AWS SAM template.
* **Globals section**. The Globals section is unique to AWS SAM. It defines properties that are common to all your serverless functions and APIs. All the AWS::Serverless::Function, AWS::Serverless::Api, and AWS::Serverless::SimpleTable resources inherit the properties that are defined in the Globals section
* **Resources section**. In AWS SAM templates the Resources section can contain a combination of AWS CloudFormation resources and AWS SAM resources.

Following are not specific to SAM:

* **Description \(optional\)**. A text string that describes the template.
* **Metadata \(optional\)**. Objects that provide additional information about the template.
* **Parameters \(optional\)**. Values to pass to your template at runtime \(when you create or update a stack\). You can refer to parameters from the Resources and Outputs sections of the template.
* **Mappings \(optional\)**. A mapping of keys and associated values that you can use to specify conditional parameter values, similar to a lookup table. You can match a key to a corresponding value by using the Fn::FindInMap intrinsic function in the Resources and Outputs sections.
* **Conditions \(optional\)**. Conditions that control whether certain resources are created or whether certain resource properties are assigned a value during stack creation or update. For example, you could conditionally create a resource that depends on whether the stack is for a production or test environment.
* **Outputs \(optional\)**. Describes the values that are returned whenever you view your stack's properties. For example, you can declare an output for an S3 bucket name, and then call the aws cloudformation describe-stacks AWS CLI command to view the name.

### Globals

[Reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-template-anatomy-globals.html)

Resources in an AWS SAM template tend to have shared configuration, such as Runtime, Memory, VPCConfig, Environment, and Cors. Instead of duplicating this information in every resource, you can write them once in the Globals section and let your resources inherit them.

```yaml
Globals:
  Function:
    Runtime: nodejs6.10
    Timeout: 180
    Handler: index.handler
    Environment:
      Variables:
        TABLE_NAME: data-table
```

Supported resources and props:

```yaml
Globals:
  Function:
    # Properties of AWS::Serverless::Function
    Handler:
    Runtime:
    CodeUri:
    DeadLetterQueue:
    Description:
    MemorySize:
    Timeout:
    VpcConfig:
    Environment:
    Tags:
    Tracing:
    KmsKeyArn:
    Layers:
    AutoPublishAlias:
    DeploymentPreference:
    PermissionsBoundary:
    ReservedConcurrentExecutions:

  Api:
    # Properties of AWS::Serverless::Api
    # Also works with Implicit APIs
    Auth:
    Name:
    DefinitionUri:
    CacheClusterEnabled:
    CacheClusterSize:
    Variables:
    EndpointConfiguration:
    MethodSettings:
    BinaryMediaTypes:
    MinimumCompressionSize:
    Cors:
    GatewayResponses:
    AccessLogSetting:
    CanarySetting:
    TracingEnabled:
    OpenApiVersion:

  SimpleTable:
    # Properties of AWS::Serverless::SimpleTable
    SSESpecification:
```

### AWS::Serverless::Api

[Reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-api.html)

```yaml
Type: AWS::Serverless::Api
Properties:
  AccessLogSetting: AccessLogSetting
  Auth: ApiAuth	<-- SAM specific
  BinaryMediaTypes: List
  CacheClusterEnabled: Boolean
  CacheClusterSize: String
  CanarySetting: CanarySetting
  Cors: String | CorsConfiguration <-- SAM specific
  DefinitionBody: String
  DefinitionUri: String | ApiDefinition
  Domain: DomainConfiguration
  EndpointConfiguration: String
  GatewayResponses: Map
  MethodSettings: MethodSettings
  MinimumCompressionSize: Integer
  Models: Map
  Name: String
  OpenApiVersion: String
  StageName: String <-- Required in SAM
  Tags: Map
  TracingEnabled: Boolean
  Variables: Map
```

example

```yaml
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: 'HelloWorldFunc'
      Description: 'This is a description for this function'
      CodeUri: hello-world/
      Handler: app.lambdaHandler
      Runtime: nodejs12.x
      Events:
        HelloWorld:
          Type: Api
          Properties:
            Path: /hello
            Method: get
            RestApiId: !Ref ApiGateway
  ApiGateway:
    Type: AWS::Serverless::Api
    Description: 'gateway desc here'
    Properties:
      Name: demo-rest-api
      StageName: prod
      Cors: "'*'"
```

### AWS::Serverless::Application

[Reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-application.html)

Embeds a serverless application from the AWS Serverless Application Repository or from an Amazon S3 bucket as a nested application. Nested applications are deployed as nested AWS::CloudFormation::Stack resources, which can contain multiple other resources including other AWS::Serverless::Application resources.

```yaml
Type: AWS::Serverless::Application
Properties:
  Location: String | ApplicationLocationObject
  NotificationARNs: List
  Parameters: Map
  Tags: Map
  TimeoutInMinutes: Integer
```



