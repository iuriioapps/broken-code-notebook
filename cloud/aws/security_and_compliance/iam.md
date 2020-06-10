---
description: Identity and Access Management
---

# IAM

* Consists of the following:
  * Users
  * Groups
  * Roles
  * Policies
* IAM is universal, it does not apply to regions, works across all regions
* Least privileges  principle used: when new users, groups or roles are created, they don't get any permissions until explicitly granted

### IAM Policies

* **AWS-managed policy** - an IAM policy which is created and administered by AWS. These AWS-provided policies allow you to assign appropriate permissions to your users, groups and roles without having to write the policy yourself. A single managed policy can be attached to multiple users, groups or roles within the same AWS account and across different accounts. You can not change the permissions defined in an AWS-managed policy.
* **Customer-managed policy** - a standalone policy that you create and administer inside your own AWS account. You can attach this policy to multiple users, groups and roles - but only within your own account.
* **Inline policy** - an IAM policy which is embedded within a single user, group or role to which it applies. There is a strict 1:1 relationship between the entity and the policy. When you delete the user, group or role in which the inline policy is embedded, the policy is also be deleted.

#### MFA Reporting and IAM

* You can enable MFA using the CLI and by using Console.
* MFA can be enabled on both root account and user accounts.
* You can enforce the use of MFA with the CLI by using the STS token service.
* You can report on who's using the MFA on a per-user basis using the `Credentials Report`

[https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/](https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/)

`aws iam create-virtual-mfa-device --virtual-mfa-device-name EC2-User --outfile /home/ec2-user/QRCode.png --bootstrap-method QRCodePNG aws iam enable-mfa-device --user-name EC2-User --serial-number arn:aws:iam::"USERNUMBERHERE":mfa/EC2-User --authentication-code-1 "CODE1HERE" --authentication-code-2 "CODE2HERE"`



