---
description: DNS server
---

# Route53

Route53 is a highly available and scalable DNS web service.

### Routing policies

* Simple routing
* Weighted routing
* Latency-based routing
* Failover routing
* Geoproximity routing \(traffic flow only\)
* Multivalue Answer routing

#### Simple Routing

If you choose the simple routing policy you can only have one record with multiple IP addresses. If you specify multiple values in a record, Route53 returns all values to the user in a random order.

#### Weighted Routing

Allows you to split your traffic based on different weights assigned.

#### Latency Routing

Allows you to route your traffic based on the lowest network latency for your end user \(i.e. which region will give them the fastest response time\). To use latency-based routing, you create a latency resource record set for the EC2 \(or ELB\) resource in each region that hosts your website. When Route53 receives a query for your site, it selects the latency resource record set for the region that gives the user the lowest latency. Route53 then responds with the value associated with that resource record set.

#### Failover Routing

Failover routing policies are used when you want to create an active/passive setup. For example, you may want your primary website to be in _eu-west-2_ and your secondary website in _ap-southeast-2_. Route53 will monitor the health of your primary site using a health check \(a healthcheck monitors the health of your endpoints\).

#### Geolocation Routing

Geolocation routing policy lets you choose where your traffic will be sent based on the geographic location of your users \(i.e. the location from which DNS queries originate\).

#### Geoproximity Routing

Geoproximity routing lets Route53 route traffic to your resources based on the geographic location of your users and your resources. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource. To use groproximity routing, you must use Route53 traffic flow.

#### Multivalue Answer Routing

Multivalue answer routing lets you configure Route53 to return multiple values, such as IP addresses for your web server, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route53 returns only values for healthy resources. This is similar to simple routing, however it allows you to put healthchecks on each record set.

### Limits

With Route 53 there is a default limit of 50 domain names. However, this limit can be increased by contacting AWS support.

