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
* All keys in the template file are CASE SENSITIVE, if there is a mistake in the template stack creation/update will fail.

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

### [Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)

* Resources are key components of the stack
* Resources section is a required section that need to be defined in CloudFormation template

```yaml
Resources:
  LogicalID:
    Type: Resource type
    Properties:
      Set of properties
```

### [Intrinsic Functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)

**Important:** You can't nest short form functions consecutively.

Invalid:

```yaml
AvailabilityZone: !Select 
  - 0
  - !GetAZs !Ref 'AWS::Region'
```

Valid:

```yaml
AvailabilityZone: !Select 
  - 0
  - Fn::GetAZs: !Ref 'AWS::Region'
```

* [**Ref**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html) - The intrinsic function Ref returns the value of the specified parameter or resource.
* [**Fn::FindInMap**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-findinmap.html) - returns the value corresponding to keys in a two-level map that is declared in the Mappings section.

```yaml
Mappings: 
  RegionMap: 
    us-east-1: 
      HVM64: "ami-0ff8a91507f77f867"
      HVMG2: "ami-0a584ac55a7631c0c"
    us-west-1: 
      HVM64: "ami-0bdb828fd58c52235"
      HVMG2: "ami-066ee5fd4a9ef77f1"
    eu-west-1: 
      HVM64: "ami-047bb4163c506cd98"
      HVMG2: "ami-31c2f645"
    ap-southeast-1: 
      HVM64: "ami-08569b978cc4dfa10"
      HVMG2: "ami-0be9df32ae9f92309"
    ap-northeast-1: 
      HVM64: "ami-06cd52961ce9f0d85"
      HVMG2: "ami-053cdd503598e4a9d"
Resources: 
  myEC2Instance: 
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: !FindInMap
        - RegionMap
        - !Ref 'AWS::Region'
        - HVM64
      InstanceType: m1.small
```

* [**Fn::Select**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-select.html) - returns a single object from a list of objects by index.

```yaml
Parameters: 
  DbSubnetIpBlocks: 
    Description: "Comma-delimited list of three CIDR blocks"
    Type: CommaDelimitedList
    Default: "10.0.48.0/24, 10.0.112.0/24, 10.0.176.0/24"

Subnet0: 
  Type: "AWS::EC2::Subnet"
  Properties: 
    VpcId: !Ref VPC
    CidrBlock: !Select [ 0, !Ref DbSubnetIpBlocks ]
```

* [**Fn::GetAtt**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html) - returns the value of an attribute from a resource in the template. To find what properties a specific resource can return with `Fn::GetAtt`, refer to that Resource documentation \(for instance, [EC2 Instance Return Values](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html)\)

```yaml
!GetAtt myELB.DNSName
```

* [**Fn::ImportValue**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html) - The intrinsic function Fn::ImportValue returns the value of an output exported by another stack. You typically use this function to create cross-stack references. 

```yaml
Fn::ImportValue:
  !Sub "${NetworkStackName}-SecurityGroupID"
```

* [**Fn::Sub**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-sub.html) - substitutes variables in an input string with values that you specify. In your templates, you can use this function to construct commands or outputs that include values that aren't available until you create or update a stack.

```yaml
!Sub 'arn:aws:ec2:${AWS::Region}:${AWS::AccountId}:vpc/${vpc}'
```

* [**Fn::Join**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-join.html) - appends a set of values into a single value, separated by the specified delimiter. If a delimiter is the empty string, the set of values are concatenated with no delimiter.

```yaml
!Join
  - ''
  - - 'arn:'
    - !Ref AWS::Partition
    - ':s3:::elasticbeanstalk-*-'
    - !Ref 'AWS::AccountId'
```

* [**Fn::Split**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-split.html) - To split a string into a list of string values so that you can select an element from the resulting string list, use the Fn::Split intrinsic function. Specify the location of splits with a delimiter, such as , \(a comma\). After you split a string, use the Fn::Select function to pick a specific element.

```yaml
!Select [2, !Split [",", !ImportValue AccountSubnetIDs]]
```

* [**Fn::Base64**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-base64.html) - returns the Base64 representation of the input string. This function is typically used to pass encoded data to Amazon EC2 instances by way of the UserData property.

```yaml
Resources:
  SimpleEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: ami-0d6621c01e8c2de2c
      InstanceType: t3.nano
      KeyName: test-cfn-kp
      SecurityGroups:
        - default
      UserData: 
        !Base64 |
          #!/bin/bash
          yum update -y
          amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
          yum install -y httpd mariadb-server
          systemctl start httpd
          systemctl enable httpd
          usermod -a -G apache ec2-user
          chown -R ec2-user:apache /var/www
          chmod 2775 /var/www
          find /var/www -type d -exec chmod 2775 {} \;
          find /var/www -type f -exec chmod 0664 {} \;
          echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
      Tags:
        - Key: Name 
          Value: SimpleEC2
```

* [**Fn::GetAZs**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getavailabilityzones.html) - returns an array that lists Availability Zones for a specified region. Because customers have access to different Availability Zones, the intrinsic function Fn::GetAZs enables template authors to write templates that adapt to the calling user's access. That way you don't have to hard-code a full list of Availability Zones for a specified region.

```yaml
AvailabilityZone: !Select 
  - 0
  - Fn::GetAZs: !Ref 'AWS::Region'
```

* [**Fn::Cidr**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-cidr.html) - returns an array of CIDR address blocks. The number of CIDR blocks returned is dependent on the count parameter.

```yaml
!Cidr [ "192.168.0.0/24", 6, 5 ]
```

* [**Fn::Transform**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-transform.html) - specifies a macro to perform custom processing on part of a stack template. Macros enable you to perform custom processing on templates, from simple actions like find-and-replace operations to extensive transformations of entire templates.

#### [Condition Intrinsic Functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html)

* [**Fn::Equals**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-equals) - Compares if two values are equal. Returns true if the two values are equal or false if they aren't.
* [**Fn::If**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-if) - Returns one value if the specified condition evaluates to true and another value if the specified condition evaluates to false.
* [**Fn::And**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-and) - Returns true if all the specified conditions evaluate to true, or returns false if any one of the conditions evaluates to false. Fn::And acts as an AND operator. The minimum number of conditions that you can include is 2, and the maximum is 10.
* [**Fn::Not**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-not) - Returns true for a condition that evaluates to false or returns false for a condition that evaluates to true. Fn::Not acts as a NOT operator.
* [**Fn::Or**](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-or) - Returns true if any one of the specified conditions evaluate to true, or returns false if all of the conditions evaluates to false. Fn::Or acts as an OR operator. The minimum number of conditions that you can include is 2, and the maximum is 10.

### [Pseudo Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)

Pseudo parameters are parameters that are predefined by AWS CloudFormation. You do not declare them in your template. Use them the same way as you would a parameter, as the argument for the Ref function.

* **AWS::AccountId** - Current account ID
* **AWS::NotificationARNs** - List of notification ARNs for current stack
* **AWS::NoValue** - Removes resource property. Used with Fn::If
* **AWS::Partition** - Partition that the resource is in
* **AWS::Region** - Current region
* **AWS::StackId** - Current stack ID
* **AWS::StackName** - Current name of the stack
* **AWS::URLSuffix** - Suffix for a domain, typically `amazonaws.com`

### [Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html)

* Enable us to input custom values to our template each time when we create or update the stack
* There can be maximum of 60 parameters in the template
* Each parameter must be given a logical name \(logical ID\), which must be alphanumeric and unique among all logical names within the template
* Each parameter must be assigned a parameter type that is supported by AWS CloudFormation
* Each parameter must be assigned a value at runtime for AWS CloudFormation to successfully provision the stack. We can optionally specify a default value to use unless another value is provided
* Parameters must be declared and referenced from within the same template. You can reference parameters from the Resources and Outputs sections of the template
* Parameter type can one of the supported built-in types, like String, List, etc, or AWS-specific parameters, like `AWS::EC2::KeyPair::KeyName` or parameters that are linked to SSM Parameter Store \(`AWS::SSM::Parameter::Value`\)

```yaml
Parameters:
  ParameterLogicalID:
    Type: DataType
    ParameterProperty: value
```

### [Mappings](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html)

* The optional Mappings section matches a key to a corresponding set of named values. For example, if you want to set values based on a region, you can create a mapping that uses the region name as a key and contains the values you want to specify for each specific region. You use the `Fn::FindInMap` intrinsic function to retrieve values in a map.

```yaml
Mappings: 
  Mapping01: 
    Key01: 
      Name: Value01
    Key02: 
      Name: Value02
    Key03: 
      Name: Value03
```

### [Conditions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html)

* The optional Conditions section contains statements that define the circumstances under which entities are created or configured. For example, you can create a condition and then associate it with a resource or output so that AWS CloudFormation only creates the resource or output if the condition is true. Similarly, you can associate the condition with a property so that AWS CloudFormation only sets the property to a specific value if the condition is true. If the condition is false, AWS CloudFormation sets the property to a different value that you specify.

```yaml
Conditions:
  Logical ID:
    Intrinsic function
```

* Conditions are evaluated based on predefined pseudo parameters or input parameter values that we specify when we create or update stack
* Within each condition we can reference other condition
* We can associate these conditions in three places
  * Resources
  * Resource properties
  * Outputs
* At stack creation or stack update, CloudFormation evaluates all condifitions in our template. During stack update, Resources that are now associated with a false condition are deleted.
* **Note:** During stack update we can't update conditions by themselves. We can update conditions only when we include changes that add, modify or delete resources.



