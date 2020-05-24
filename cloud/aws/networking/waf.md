---
description: Web Application Firewall
---

# WAF

* AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to CloudWatch or an ALB to to API Gateway. WAF also lets you control access to your content.
* You can configure conditions such as what IP addresses are allowed to make this request or what query string parameters need to be passed for the request to be allowed, and then the ALB or CloudFront will either allow this content to be received or to give HTTP 403.
* You can define conditions by using characteristics of web requests such as:
  * IP addresses that requests originate from
  * Country that requests originate from
  * Values in request headers
  * Strings that appear in requests, either specific strings or strings that match regular expressions
  * Length or requests
  * Presence of SQL code that is likely to be malicious
  * Presence of a script that is likely to be malicious \(XSS attack\)
* At its most basic level, WAF allows 3 different behaviors:
  * Allow all requests except the ones that you specify
  * Block all requests except the ones that you specify
  * Count the requests that match the properties that you specify

