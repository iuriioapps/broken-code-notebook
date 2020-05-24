# Systems Manager

Systems Manager is an AWS service that you can use to view and control your infrastructure on AWS. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources.

* Integrates with CloudWatch allowing you view your dashboards, view operational data and detect problems.
* Includes `Run Command` which automates operational tasks across resources - e.g. security patching, package installs, etc.
  * Allows you to run predefined commands on one or more EC2 instances.
  * Stop, restart, terminate, resize instances.
  * Attach / detach EBS volumes.
  * Create snapshots, backup DynamoDB.
  * Apply patches and updates
  * Run an Ansible playbook
  * Run a shell script
* Organize your inventory, grouping resources together by application or environment - including on-premises systems.

### Systems Manager Parameter Store

* Confidential information, such as passwords, database connection strings, and license codes can be stored in SSM Parameter Store.
* You can store values as plain text or you can encrypt the data.
* You can then reference these values by using their names.
* You can use this service with EC2, CloudFormation, Lambda, EC2 Run Command, etc.

