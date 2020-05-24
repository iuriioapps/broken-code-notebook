---
description: Security Token Service
---

# STS

* Grants users limited and temporary access to AWS resources. Users can come from these sources:
  * Federation \(typically Active Directory\)
    * Uses Security Assertion Markup Language \(SAML\)
    * Grants temporary access based off the user Active Directory credentials
    * Does not need to be a user in IAM
    * Single sign on allows users to login to AWS console without assigning IAM credentials
  * Federation with mobile apps
    * Use Facebook / Google / Amazon or other OpenID providers to login.
  * Cross account access
    * Let's users from one AWS account access resources in another AWS account

