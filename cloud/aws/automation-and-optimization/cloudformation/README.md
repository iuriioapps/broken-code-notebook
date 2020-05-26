# CloudFormation

* CloudFormation is a service that allows you to manage, configure and provision your AWS infrastructure as a code \(IaaC\).
* Resources are defined using a CloudFormation template.
* CloudFormation interpretes and makes the appropriate API calls to create the resources you have defined.
* Supports YAML or JSON.

### Benefits

* Infrastructure provisioned consistently, with fewer mistakes
* Less time and effort than configuring things manually
* You can version control and peer review your templates
* Free to use \(charged only for what you create\)
* Can be used to manage updates and dependencies
* Can be used to rollback and delete the entire stack

### Template

* YAML or JSON used to describe the end state of the infrastructure you are either provisioning or changing
* After creating the template, you upload it to CloudFormation using S3
* CloudFormation reads the template and makes the API calls on your behalf
* The resulting resources are called a Stack
* `Resources` is the only mandatory section of the CloudFormation template
* `Transform` section is used to reference additional code stored in S3, allowing for code reuse

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: |
  
Parameters:
  
Metadata:
  
Mappings:
  
Conditions:
  
Resources: # required
  
Transform:
  
Outputs:

```



