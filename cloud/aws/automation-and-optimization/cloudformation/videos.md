# Videos

## AWS re:Invent 2019: \[REPEAT 1\] Best practices for authoring AWS CloudFormation \(DOP302-R1\)

{% embed url="https://www.youtube.com/watch?v=bJHHQM7GGro" %}

### Authoring Best Practices

* Parameters: Avoid hardcoding values, can add validation to users and improve UX with console grouping, labels, descriptions; keep secrets in SSM Parameter Store and Secrets Manager
* Mappings: As a case statement, helps maintain a set of information for various environments
* Conditions: Simple if/then statements \(e.g. if dev do this, if prod do that\)
* Imports and exports, leverage cross-stack references or export/import values through SSM
* Use !Sub over !Join
* Leverage SSM Parameter Store for latest AMI instance IDs

### Testing & Deployment Best Practices

* Run Lint in headless mode, prevent promoting templates with errors
* Use [TaskCat](https://github.com/aws-quickstart/taskcat/tree/master/taskcat) to live-test infrastructure
* Use ChangeSets to know what effect the change will have on the underlying resources before actually deploying it
* Use StackSets to deploy across multiple accounts and regions
* Use automated deployment pipelines to deploy infrastructure changes
* Refactor large stacks with resource import
* Detect and remediate drift

### StackSets best practices

* Partially deploy your CloudFormation StackSet updates to reduce blast radius; great for sanity testing / releasing incrementally
* Depending on your speed needs, consider setting a higher concurrent account limit
* Use parameter overrides to define specific parameters in account-region pairs
* Separate stacks by function and frequency of changes needed



