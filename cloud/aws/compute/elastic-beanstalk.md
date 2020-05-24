# Elastic Beanstalk

Allows you to quickly deploy and manage applications in AWS Cloud without worrying about the infrastructure that runs these applications.

* Automatically handles the details of capacity provisioning, load balancing, scaling and health monitoring.
* It is platform-as-a-service \(PaaS\).
* Support Go, Java, .NET, NodeJS, PHP, Python, Ruby.
* Supports following web containers: Tomcat, Passenger, Puma.
* Supports Docker containers.
* Your application domain: `{subdomain}.{region}.elasticbeanstalk.com`

### Environment pages

* **Configuration page** shows the resources provisioned for this environment. This page also lets you configure some of the provisioned resources.
* **Health page** shows the status and detailed health information about the EC2 instances running your application.
* **Monitoring page** shows the statistics for the environment, such as the average latency and CPU utilization. You also use this page to create alarms for the metrics that you're monitoring.
* **Event page** shows any informational or error messages from services that this environment is using.
* **Tags page** shows tags.

### Concepts

* **Application** - a logical collection of Beanstalk components, including environments, versions, and environment configurations. It's conceptually similar to a folder.
* **Version** - refers to a specific, labeled iteration of deployable code for a web application. An application version points to an S3 object that contains the deployable code. Applications can have many versions and each application version is unique. There is a limit to the number of application versions you can have.You can avoid hitting the limit by applying the application version policy to your applications to tell Beanstalk to delete application versions that are old, or to delete versions when the total number of versions for an application exceeds a specific number.
* **Environment** - a version that is deployed to AWS resources. Each environment runs only a single application version at a time, however you can run the same version or different version in many environments at the same time.
* **Environment Tier** - determines whether Beanstalk provisions resources to support an application that handles HTTP requests or an application that pulls tasks from a queue. An application that serves HTTP requests runs in a web server environment. An application that pulls events from an SQS queue runs in a worker environment.
* **Environment Configuration** - identifies the collection of parameters and settings that define how an environment and its associated resources behave.
* **Configuration Template** - a starting point for creating unique environment configuration.

### Environment Types

* **Load balancing, auto-scaling environment** - automatically starts additional instances to accommodate increasing load on your application.
* **Single instance environments** - contains one EC2 instance with Elastic IP address.

### Environment Configurations

* Your EC2 VMs configured to run web applications on the platform that you choose.
* An auto-scaling group that ensures that there is always one instance running in a single-instance environment, and allows configuration of the group with a range of instances to run in a load-balanced environment.
* When you enable load-balancing, Beanstalk will create ELB to distribute traffic among your environment instances.
* Beanstalk provides integration with RDS to help you add a database instance to your environment. When you add a database instance to your environment, Beanstalk provides connection information to your application by setting environment properties for the database: hostname, port, user name, password, and a database name.
* You can use environment variables to pass secrets, endpoints, debug settings and other information to your application. Environment properties help you run your application in multiple environments for different purposes, such as dev, qa, prod, etc.
* You can configure your environment to use SNS to notify you of important events that affect your application.
* Your environment is available to users at a subdomain of `elasticbeanstalk.com`. When you create an environment, you can choose a unique subdomain that represent your application.

### Monitoring

* Beanstalk monitoring console displays you environment status and application health at a glance.
* Beanstalk reports a health of a web server environment depending of how the application running in it responds to a health check.
* Enhanced health reporting is a feature that you can enable on your environment to allow Beanstalk to gather additional information about resources in your environment.Beanstalk analyses the information gathered to provide a better picture of overall environment health and aid in the identification of issues that can cause your application to become unavailable.
* You can create alarms for metrics to help you monitor changes to your environment so you can easily identify and mitigate problems before they occur.
* EC2 instances in your environment generate logs that you can view to troubleshoot issues with your application or configuration files.

### Security

* When you create an environment, Beanstalk prompts you to provide two IAM roles: service role and an instance profile role:
  * **service role** - assumed by Beanstalk to use other AWS services on your behalf.
  * **instance profile** - applied to the instances in your environment and allows them to retrieve application versions from S3, upload logs to S3 and perform other tasks that vary depending on the environment type and platform.
* **User policies** - allow users to create and manage Beanstalk applications and environments.

### Pricing

* There is no additional charge for using Beanstalk. You pay only for underlying resources that you consume.

### Worker Environments

* If your applications perform operations or workflows that take a long time to complete, you can offload these tasks to a dedicated worker environment. Decoupling your application frontend from a process that performs blocking operations is a common way to ensure that your application stays responsive under load.
* One option is to spawn worker processes locally, return success and process the task asynchronously. This works if your instance can keep up with all the tasks sent to it. Under high load, however, an instance can become overwhelmed with background tasks and become unresponsive to higher priority requests. If individual users can generate multiple tasks, the increase in load might not correspond to an increase in users, making it hard to scale out the web tier.
* Beanstalk worker environments simplify this process by managing the SQS queue and running the daemon process on each instance that reads from the queue for you. When daemon pull an item from the queue, it sends the HTTP POST request locally to http://localhost/ on port 80 with contents of the queue message in the body. All that your application needs to do is perform a long-running task in response to POST.

### Periodic Tasks

* You can define periodic tasks in a file named `cron.yaml` in your source bundle to add jobs to your worker environments queue automatically at a regular interval.
* When the task runs, a daemon posts a message to the environment's SQS queue with the header indicating the job  that needs to be performed. Any instance in the environment can pick up the message and run the job.
* If you configure your worker environment with an existing SQS queue and choose FIFO queue, periodic tasks are not supported.
* Beanstalk uses leader election to determine which instance in your worker environment queues the periodic task. Each instance attempts to become a leader by writing to the DynamoDB table. The first instance that succeeds is the leader, and must continue to write to the table to maintain leader status. If the leader goes out of service, another instance quickly takes its place.

### X-Ray

* You can use console or configuration file to run X-Ray daemon on the instances in your environment.
* You can use `XRayEnabled` option in the `aws:elasticbeanstalk:xray` namespace to enable debugging. Add this configuration to `debugging.config` or `xray-daemon.config`:

```yaml
option_settings:
    aws:elasticbeanstalk:xray:
        XRayEnabled: true
```

### Deployment options

* **All at once** - deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs.
* **Rolling** - deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment capacity by the number of instances in a batch.
* **Rolling with additional batch** - deploys the new version in batches, but first launch the additional batch of instances to ensure full capacity during the deployment process.
* **Immutable**  - deploy a new version to a fresh group of instances by performing the immutable update.
* **Blue/green** - deploy a new version to a separate environment, and then swap CNAMES of the two environments to redirect traffic to the new version instantly.

### Platform updates

* Beanstalk regularly releases new platform versions to update all Linux-based and Windows-based platforms. New platform version provide update to existing software components and support for new features and configuration options.
* You can use Beanstalk console of CLI to update your environment platform version. Depending of platform version you'd like to update to, Beanstalk recommends one of two methods for performing platform updates:
  * **Update your environment platform version** - this is the recommended method when you're updating to the latest platform version, without a change in the runtime, web server or application server versions, and without a change in major platform version.
  * **Perform a blue/green deployment** - this is the recommended method when you're updating to a different runtime, web server or application server version, or to a different major platform version. This is a good approach when you want to take advantage of new runtime capabilities or the latest Beanstalk functionality.

### Deploying with CLI

* CLI provides interactive commands that simplify creating, updating and monitoring environments from a local repository. It is recommended that you use CLI as part of your everyday development and testing cycle as an alternative to AWS Console.
* You can tell CLI to deploy ZIP or WAR file that you generate as part of a separate build process by adding the following lines to `.elasticbeanstalk/config.yaml` in your project folder:

```yaml
deploy:
    artifact: path/to/your/build/artifact.zip
```

* If you configure the CLI in your Git repository, and you don't commit the artifact to source, use the `--staged` option to deploy the latest build:

`eb deploy --staged`



