---
description: Elastic Container Service & Elastic Container Registry
---

# ECS & ECR

## ECS - Amazon Elastic Container Service

* A container service to run and and manage Docker container on a cluster.
* ECS can be used to create a consistent deployment and build experience, manage and scale batch and ELT workloads, and build sophisticated application architectures on a microservice model.
* ECS is a regional service.

### Features

* You can create ECS clusters within a new or existing VPC.
* After the cluster is up and running, you can define task definitions and services that specify which Docker container images to run across your clusters.
* AWS SLA guarantees a monthly uptime percentage of at least 99.99% for ECS.

### Components

* **Containers and images**
  * Your application components must be architected to run in containers - containing everything that your application needs to run: code, runtime, system tools, libraries, etc.
  * Containers are created from a read-only template called an image.
  * Images are typically build from a Dockerfile, a plaintext file that specifies all of the components that are included in the container. These images are then stored in the registry, from which they can be downloaded and run on your cluster.
  * When you launch a container instance, you have the option of passing the user data to the instance. The data can be used to perform common automated configuration tasks and even run scripts when the instance boots.
* **Task definitions**
  * Task definitions specify various parameters for your application. It's a text file, in JSON format, that describes one or more containers, up to a maximum of 10, that form your application.
  * Task definitions are split into separate parts:
    * **Task family** - the name of the task and each family can have multiple revisions.
    * **IAM task role** - specifies the permissions that containers in tasks should have.
    * **Network mode** - determines how the networking is configured for your containers.
    * **Container definitions** - specify which image to use, how much CPU and memory the containers are allocated, and many more options.
    * **Volumes** - allow you to share data between containers and even persist the data on the container instance when the containers are no longer running.
    * **Task placement constraints** - lets you customize how your tasks are placed within the infrastructure.
    * **Launch types** - determines which infrastructure your tasks use.

### Task definitions

* **Fargate launch type**
  * Fargate task definitions require that network mode is set to `awsvpc`. The `awsvpc` provides each task with its own ENI \(Elastic Network Interface\).
  * Fargate task definitions require that you specify CPU and memory at the task level.
  * Fargate task definitions only support `awslogs` log driver for the log configuration. This configures your Fargate tasks to send log information to CloudWatch Logs.
  * Task storage is ephemeral. After Fargate task stops, the storage is deleted.
  * Put multiple containers in the same task definition if:
    * Containers share a common lifecycle.
    * Containers are required to be run on the same underlying host.
    * You want your containers to share resources.
    * Your containers share data volumes.
  * Otherwise, define your containers in separate task definitions so that you can scale, provision and deprovision them separately.
* **EC2 launch type**
  * Create task definitions that group the containers that are used for a common purpose, and separate the different components into multiple type definitions.
  * After you have your task definitions, you can create services from them to maintain the availability of your desired tasks.
  * For EC2 tasks, the following are the types of data volumes that can be used:
    * Docker volumes
    * Bind mounts
  * Private repositories are only supported by the EC2 launch type.

### Tasks and scheduling

* A tasks is the instantiation of a task definition within a cluster. After you have created a task definition for your application, you can specify the number of tasks that will run on your cluster.
* Each task that uses the Fargate launch type has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources or ENI with another task.
* The task scheduler is responsible for placing tasks within your cluster. There are several scheduling options available:
  * **REPLICA** - places and maintains the desired number of tasks across your cluster. By default, the service scheduler spreads tasks across the Availability Zones.You can use task placement strategies and constraints to customize task placement decision.
  * **DAEMON** - deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster. When using this strategy, there's no need to specify a desired number of tasks, a task placement strategy or use Service Auto Scaling policy.
* You can upload a new version of your application task definition, and the ECS scheduler automatically starts new containers using the updated image and stop containers running the previous version.

### Clusters

* When you run tasks using ECS, you place them in a cluster, which is a logical grouping of resources.
* Clusters are region-specific.
* Clusters can contain tasks using both the Fargate and EC2 launch types.
* When using the Fargate launch type with tasks within your cluster, ECS manages your resources.
* When using EC2 launch type, then your clusters are a group of container instances you manage. These clusters can contain multiple container instance types, but each container instance may only be part of one cluster at a time.
* Before you can delete a cluster, you must delete all services and deregister the container instances inside that cluster.

### Services

* ECS allows you to run and maintain a specified number of a task definitions simultaneously in a cluster.
* In addition to maintaining the desired count of tasks in your service, you can optionally run your service behind a load balancer.
* There are two deployment strategies in ECS:
  * **Rolling update**
    * This involves the service scheduler replacing the current running version of the container with the latest version.
    * The number of tasks ECS adds or removes from the service during the rolling update is controlled by the deployment configuration, which consists of minimum and maximum number of tasks allowed during a service deployment.
  * **Blue/green deployment**
    * This deployment type allows you to verify a new deployment of a service before sending production traffic to it.
    * The service must be configured to use either Application Load Balancer or Network Load Balancer.

### Container Agent

* The Container Agent runs on each infrastructure resource within an ECS cluster.
* It sends information about the resource's current running tasks and resource utilization to ECS, and starts and stops tasks whenever it receives the request from ECS.
* Container agent is only supported on EC2 instances.

### Task Placement Strategies

* A task placement strategy is an algorithm for selecting instances for task placement or tasks for termination. When the tasks that uses the EC2 launch type is launched, ECS must determine where to place the task based on the requirements specified in the task definition, such as CPU and memory. Similarly, when you scale down the task count, ECS must determine which tasks to terminate.
* A task placement constraint is a rule that is considered during task placement.
* You can use constraints to place tasks based on AZ or instance type.
* You can also associate attributes, which are name-value pairs, and then use a constraint to place tasks based on attribute.
* Task placement strategy types:
  * **Binpack** - place tasks based on the least available CPU and memory. This minimizes the number of instances in use and allow you to be cost-efficient.
  * **Random** - place tasks randomly. You use this strategy when task placement or termination does not matter.
  * **Spread** - place tasks evenly based on the specified value. Accepted values are attribute key-value pairs, instance ID or host. Spread is typically used to achieve high availability by making sure that multiple copies of task are scheduled across multiple AZs in the default placement strategy used for services.
* You can combine different strategy types to suit your application needs.
* Task placement strategies are best effort.
* By default, Fargate tasks are spread across availability zones.
* By default ECS uses following placement strategies:
  * When you run tasks with the `RunTask` action, tasks are placed randomly in the cluster.
  * When you launch and terminate tasks with the `CreateService` API action, the service scheduler spreads the tasks across the AZs \(and the instances within the zones\) in the cluster. 

### Cluster queries

Cluster queries are expressions that enable you to group objects. For example, you can group container instances by attributes such as AZs, instance types or custom metadata. You can add custom metadata to your container instances, known as attributes. Each attribute has a name and optional string value. You can use the built-in attributes provided by ECS or define custom attributes.

After you have defined a group of container instances, you can customize ECS to place tasks on container instances based on group. Running tasks manually is ideal in certain situations. For example, suppose that you're developing a task but you're not ready to deploy this task with the service scheduler. Perhaps your task is a one-time or periodic batch job that does not make sense to keep running or restart when finishes.

### Secrets

ECS enables you to inject sensitive data into your containers by storing your sensitive data in either Secrets Manager or Systems Manager Parameter Store parameters and then referencing them in your container definition. This feature is supported by tasks using both the EC2 and Fargate launch types.

Secrets can be exposed to containers in the following way:

* as environment variables, use the `secrets` container definition parameter.
* in the log configuration of a container, use the `secretOptions` container definition parameter.

### Fargate

* You can use Fargate with ECS to run containers without having to manage servers or clusters of EC2 instances.
* You no longer have to provision, configure or scale clusters of virtual machines to run containers.
* Fargate only supports images hosted on ECR or Docker Hub.

### Monitoring

* You can configure your container instances to send log information to the CloudWatch Logs from your container instances in one convenient location.
* With CloudWatch Alarms, watch a single metric over time period that you specify, and perform one or more actions based on the value of the metric relative to a give threshold over the number of time periods.
* Share a log files between accounts, monitor CloudTrail log files in real time by sending them to CloudWatch Logs.

### ECS and X-Ray

* The AWS X-Ray SDK does not trace data directly to the AWS X-Ray. To avoid calling the service every time your application serves the request, the SDK sends the trace data to a daemon, which collects segments for a multiple requests and uploads them in batches.
* Use a script to run a daemon alongside your application.
* To properly instrument your applications in ECS, you have to create a Docker image that runs X-Ray daemon, upload it to an image repository, and then deploy it to your ECS cluster. You can use port mappings and network mode settings in your task definition file to allow your application to communicate with the daemon container.
* The X-Ray daemon is an application that listens for the traffic on UDP port 2000, gathers raw segment data and relays it to the X-Ray API. The daemon works in conjunction  with X-Ray SDK and must be running so that data sent by the SDK can reach the X-Ray service.

### Tagging

* ECS resources, including task definitions, clusters, tasks, services and container instances, are assigned an ARN and a unique resource ID. These resources can be tagged with values that you define, to help you organize and identify them.

### Pricing

* With Fargate, you pay for the amount of vCPU and memory resources that your containerized application requests. vCPU and memory resources are calculated from the time your container images are pulled until the ECS task terminates.
* There is no additional charge for EC2 launch type. You pay for the AWS resources you create to store and run your application.

### Limits

| Description | Value |
| :--- | :--- |
| Number of clusters, per region, per account | 1000 |
| Number of container instances per cluster | 1000 |
| Number of services per cluster | 500 |
| Number of tasks using EC2 launch type per service \(the desired count\) | 1000 |
| Number of tasks using Fargate launch type, per region | 50 |
| Number of load balancers per service | 1 |
| Number of tasks launched \(count\) per run-task | 10 |
| Number of container instances per start-task | 10 |
| Task definition max containers | 10 |
| Maximum layer size of an image used by a task, Fargate launch type | 4GB |
| Maximum size of a shared volume used by multiple containers within a task using the Fargate launch type | 4GB |
| Maximum container storage for tasks using the Fargate launch type | 10 GB |

## ECR - Elastic Container Registry

### Features

* ECR supports Docker registry API v2 allowing you to use Docker CLI commands or your preferred Docker tools in maintaining your existing development workflow.
* ECR stores both the containers you create and any container software you buy through AWS marketplace.
* ECR stores your images in S3.
* ECR support the ability to define and organize repositories in your registry using namespaces.
* You can transfer your container images to and from ECR via HTTPS.

### Components

* **Registry**
  * A registry is provided to each AWS account; you can create image repositories in your registry and store the images in them.
  * The URL for your default registry is `https://aws.{account_id}.dkr.ecr.{region}.amazonaws.com`
  * You must be authenticated before you can use your registry.
* **Authorization Token**
  * Your Docker client needs to authenticate to ECR repositories as an AWS user before it can push and pull images. The AWS CLI `get-login` command provides you with authentication credentials to pass to Docker.
* **Repository**
  * An image repository contains your Docker images
  * ECR uses resource-based permissions to let you specify who has access to a repository and what actions they can perform on it.
  * ECR lifecycle policies enable enable you to specify the lifecycle management in a repository.
* **Repository policy**
  * You can control access to your repositories and the images within them with repository policy.
* **Image**
  * You can push and pull images to your repositories. You can use these images locally on your development system, or you can use them in your task definitions.

### Security

* By default, IAM users don't have permissions to create or modify ECR resources, or perform tasks using the ECR API.
* Use IAM policies to grant or deny permissions to use ECR resources and operations.
* ECR partially supports resource-level permissions.

### Pricing

* You pay only for the amount of data you store in your repositories and data transferred to the internet.

### Limits

| Description | Limit |
| :--- | :--- |
| Maximum number of repositories per account | 1000 |
| Maximum number of images per repository | 1000 |



