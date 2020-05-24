---
description: ELB
---

# Elastic Load Balancers

AWS supports 3 types of load balancers:

* Application load balancer \(ALB\)
* Network load balancer \(NLB\)
* Classic load balancer \(CLB\)

Tips:

* Instances monitored by ELB are reported as: InService or OutOfService
* Health checks check the instance health by talking to it
* Load balancers have their own DNS names, you are never given an IP address.

#### ALB

Application load balancers are best suited for load balancing of HTTP\(S\) traffic. They operate at Level 7 and are application aware. They are intelligent and you can create advanced request routing, sending specific requests to specific web services.

#### NLB

Network load balancers are best suited for load balancing of TCP traffic where extreme performance is required. Operating at the connection level \(OSI Level 4\), NLB are capable of handling millions of requests per second, while maintaining ultra-low latencies.

#### CLB

Classic load balancers are the legacy ELB. You can load balance HTTP\(S\) applications and use Level 7 specific features, such as X-Forwarded and sticky sessions. You can also use strict Level 4 load balancing for applications that rely purely on the TCP protocol. 

If your application stops responding, CLB responds with HTTP 504 error. This means that application is having issues.

### Sticky sessions

CLB routes each request independently to the registered EC2 instance with the smallest load. Sticky sessions allow you to bind a user session to a specific EC2 instance. This ensures that all requests from the user during the session are sent to the same instance. You can enable sticky sessions for ALB as well, but the traffic will be sent at the target group level.

### Cross-zone load balancing

With cross-zone load balancing each load balancer node for your CLB distributes requests eventy across the registered instances in all enabled AZs. If cross-zone load balancing is disabled, each load balancer node distributes requests evenly across the registered instances in its AZ only.  CLB has cross-zone load balancing off by default, ALB cross-zone load balancing is always on.

### Path patterns

You can create a listener with rules to forward requests based on the URL path. This is known as path-based routing. If you are running microservices, you can route traffic to multiple backend services using path-based routing. For example, you can route general requests to one target group and requests to render images to another target group.

## Pre-warming your ELB 

Are you expecting a large spike in traffic for an event? Get in touch with Amazon and have them pre-warm your load balancers. ELBs scale best with a gradual increase in traffic load. They do not respond well to spiky loads, and can break if too much flash traffic is directed their way.

This is covered under the ELB best practices, and can be critical to having your event traffic handled gracefully, or being left wondering what just happened to your application stack when there is a sudden drop in traffic. Key pieces of information to relay to Amazon when contacting them are the total number of expected requests, and the average request response size.

One other key piece of information has to do with operating at scale. If you are terminating your SSL on the ELB, and the HTTPS request response size is small, be sure to stress that point with Amazon support. Small request responses coupled with SSL termination on the ELB may result in overloading the ELBs, even though Amazon will have scaled them to meet your anticipated demand.

AWS will need to know:

* start and end dates
* expected request rate per second
* total size of a typical request

## Monitoring

Four different ways to monitor your load balancers:

* CloudWatch metrics
* Access Logs
* Request Tracing
* CloudTrai Logs

### CloudWatch metrics

ELB publishes data points to CloudWatch for your load balancers and your targets. CloudWatch enables you to retrieve statistics about those data points as an ordered set of time series data.

#### ELB CloudWatch Metrics 

#### Overall health

* **BackendConnectionErrors** - number of unsuccessful connections to backend instances
* **HealthyHostCount** - number of healthy instances registered
* **UnHealthHostCount** - number of unhealthy instances
* **HttpCode\_Backend\_2XX,3XX,4XX,5XX**

#### Performance

* **Latency** - number of seconds taken for registered instance to respond / connect
* **RequestCount** - number of requests completed / connections made during the specified interval \(1 or 5 minutes\)
* **SurgeQueueLength** - number of pending requests, max 1024, additional requests will be rejected \(Classic ELB only\)
* **SpilloverCount** - number of requests rejected because the surge queue is full \(Classic ELB only\)

### Access Logs

ELB provide access logs that capture detailed information such as a time the request was received, the client's IP address, latencies, request paths, and server responses. You can use these access logs to analyze patterns and troubleshoot issues. Access logging is an optional feature of ELB that is disabled by default. After you enable access logging for your load balancer, ELB captures the logs and stores them in S3 bucket that you specify as compressed files. You can disable access logging at any time.

Access logs can store data where the EC2 instance has been deleted. For example, you have a fleet of EC2 instances behind an autoscaling group. For some reason your application has loads of 5XX errors which is only reported by your end customers a couple of days after the event. If you aren't storing the web server logs anywhere persistent, it's still possible to trace these 5XX errors using access logs which would be stored in S3.

### Request Tracing

You can use request tracing to track HTTP requests from clients to targets or other services. When the load balancer receives a request from a client, it adds or updates the `X-Amzn-Trace-Id` header before sending the request to the target. Any services or applications between the load balancer and the target can also add or update this header. Available for ALB only.

### CloudTrail

You can use CloudTrail to capture detailed logs about the calls made to ELB API and store them as the log files in S3.

