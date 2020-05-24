# Web Identity Federation

* Federation allows users to authenticate with **Web Identity Provider** \(Google, Facebook, etc\).
* The user authenticates first with Web ID Provider and receives a token, which is exchanged for temporary AWS credentials allowing them to assume IAM role 
* **Cognito** is an **Identity Broker** which handles interaction between your applications and the Web ID Provider:
  * Provides sign up, sign in, and guest user access
  * Syncs user data for a seamless experience across your devices
  * Cognito is the AWS recommended approach for Web Identity Federation, particularly for mobile apps.
* Cognito uses **User Pools** to manage user sign up and sign in directly or via Web ID Provider
* Cognito acts as an Identity Broker, handling all interactions with Web ID Provider
* Cognito uses push synchronization to send a silent push notification of user data updates to multiple device types associated with a user ID.

**STS AssumeRoleWithWebIdentity**

* Part of STS \(Security Token Service\)
* Allows users who have authenticated with Web ID Provider to access AWS resources
* Once the user has authenticated, the application makes the '`assume-role-with-web-identity`' API call.
* If successful, STS will return temporary credentials enabling access to AWS resources
* AssumedRoleUser ARN and AssumedRoleID are used to programmatically reference the temporary credentials - not an IAM role or user.
* Mobile apps should use Cognito for federation instead of STS AssumeRoleWithWebIdentity

