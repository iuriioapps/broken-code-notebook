# Batch

* Enables you to run batch computing workloads on AWS
* It's a regional service that simplifies running batch jobs across multiple AZs within a region.

### Features

* Batch manages compute environments and job queues, allowing you to easily run thousands of jobs of any scale using EC2 and EC2 Spot.
* Batch chooses where to run the jobs, launching additional capacity if needed.
* Batch carefully monitors the progress of your jobs. When capacity no longer needed it will be removed.
* Batch provides the ability to submit jobs that are part of pipeline or workflow, enabling you to express any interdependency\(ies\) that exist between them as you submit jobs.

### Components

* Jobs
* Job definitions
* Job queues
* Compute environment

### Jobs

* A unit of work \(such as shell script, a Linux executable or a Docker container image\) that you submit to Batch.
* Jobs can reference other jobs by name of by ID, and can be dependent on the successful  completion of other jobs.
* Job types:
  * single
  * array \(between 2 and 10000\)
* An array job shares common job parameters, such as job definition, vCPUs, and memory. It runs as a collection of related, yet separate, basic jobs that can be distributed across multiple hosts and may run concurrently.
* Multi-node parallel jobs enable you to run single, large-scale, tightly-coupled and distributed GPU model training jobs that span multiple EC2 instances.
* Batch lets you specify up to 5 distinct node groups for each job. Each group can have its own container images, commands, environment variables, and so on.
* Each multi-node parallel jobs contain a main node, which is launched first. After the main node is up, the child nodes are launched. If the main node exists, the job is considered finished, and the child nodes are stopped.
* Not supported on environments that use Spot instances.

### Dependencies

* A job may have up to 20 dependencies. 
* For `job depends on` enter the job ID for any jobs that must finish before this job starts.
* \(array only\) For N-to-N job dependencies, specify one or more job IDs for any array jobs for which each child job index of this job should depend on the corresponding child index job of the dependency.
* \(array only\) Run children sequentially creates a sequential dependency for the current array job. This ensures that each child index job waits for its earlier sibling to finish.

### States

* **SUBMITTED** - a job that has been submitted to the queue, and has not been evaluated by the scheduler.
* **PENDING** - a job that resides in the queue and is not yet able to run due to a dependency on another job or resource.
* **RUNNABLE** - a job that resides in the queue, has no outstanding dependencies, and is therefore ready to be scheduled to a host. Jobs in this state are started as soon as sufficient resources are available in one of the compute environments that are mapped to it's jobs queue.
* **RUNNING** - the job is running as a container job on an ECS container instance within a compute environment. When the job's container exists, the process exit code determines whether the job succeeded or failed. An exit code of 0 indicates success, and any non-zero exit code indicates failure.
* **SUCCEEDED** - the job has successfully completed with an exit code of 0. The job state for succeeded jobs is persisted for 24 hours.
  * You can apply a retry strategy to your jobs and job definitions that allow failed jobs to be automatically retried.
  * You can configure a timeout duration for your jobs so that if a job runs longer than that, Batch terminates the job. If a job terminated for exceeding the timeout, it is not retried. If it fails on its own, then it can retry, if retries are enabled, and the timeout coundown is started over for the new attempt.

### Job definitions

* Specifies how jobs are to be run.
* The definition can contain:
  * An IAM role to provide programmatic access to other AWS resources.
  * Memory and CPU requirements for the job.
  * Controls for container properties, environment variables, and mount points for persistent storage.

### Job Queues

* This is where job resides until it is scheduled onto a compute environment.
* You can associate one or more compute environments with a job queue.
* You can assign priority values for the compute environments and even across the job queues themselves.
* The Batch Scheduler evaluates when, where and how to run jobs that have been submitted to a job queue. Jobs run in approximately the order in which they are submitted as long as all dependencies on other jobs have been met.

### Compute environment

* A set of managed or unmanaged \(such as EC2 instances\) that are used to run the jobs.
* Compute environments contain the ECS container instances that are used to run containerized batch jobs.
* A given compute environment can be mapped to one or more job queues.

#### Managed compute environment

* Batch manages the capacity and instance types of the compute resources within the environment, based on the compute resource specification that you define when you create the compute environment.
* You can choose to use EC2 on-demand or spot instances in your compute environment.
* ECS container instances are launched into the VPC and subnets that you specify when you create the compute environment.

#### Unmanaged compute environment

* You manage your own compute resources in this environment.
* After you have created your unmanaged compute environment, use `DescribeComputeEnvironments` API operation to view compute environment details and find the ECS cluster that is associated with the environment and manually launch container instances into the cluster.

### Security

* By default, IAM users don't have permissions to create or modify Batch resources, or perform tasks using Batch API.
* Take advantage of IAM policies, roles and permissions.

### Monitoring

* You can use AWS Batch event stream for CloudWatch to receive near real-time notifications regarding the current state of jobs that have been submitted to your job queue.
* Events from Batch event stream are ensured to be delivered at least once.
* CloudTrail captures all API calls for AWS Batch as events.

### Pricing

* There is no additional cost for Batch. You pay for resources you create to store and run your application. 

