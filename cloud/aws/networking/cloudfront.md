---
description: CDN - Content Delivery Network
---

# CloudFront

CloudFront is a content delivery network. 

**A content delivery network \(CDN\)** is a system of distributed servers \(network\) that delivers web pages and other web content to a user based on a geographic location of the user, the origin of the webpage, and a content delivery server.

CloudFront can be used to deliver your entire website, including dynamic, static, streaming and interactive content using the global network of edge locations. Requests for your content are automatically routed to the nearest edge location, so content is delivered with the best possible performance.

One of the purposes of CloudFront is to reduce the number of requests that your origin server must respond to directly. This reduces the load on your origin server and also reduces the latency because more objects are served from CloudFront edge locations which are closer to your users.

* **Edge location** - The location where content will be cached. This is separate to an AWS Region / Zone
* **Origin** - This is the origin of all the files that the CDN will distribute. This can be either an S3 bucket, an EC2 instance, Elastic Load Balancer or Route53.
* **Distribution** - This is a name given to the CDN which consists of collection of Edge locations

  * Web distribution - for websites
  * RTMP - for media streaming

* Edge locations are not just for READ only operations - you can write to them to
* Objects are cached for the life of TTL \(Time To Live\)
* You can clear cached objects, but you will be charged

### Cache Hit Ratios

The more request the CloudFront is able to serve from edge locations, the better it works. The ratio of requests served from edge locations \(rather than the origin\) is known as cache hit ratio. The more requests from edge location, the better the performance. You can view the percentage of viewer requests that are hits, misses and errors in the CloudFront console.

### Maximizing Cache Hit Ratio

The following strategies can maximize your cache hit ratios:

* **Specifying how long CloudFront caches your objects** - to increase your cache hit ratio, you can configure your origin to add a `Cache-Control=max-age` directive to your objects, and specify the longest practical `max-age`. The shorter the cache duration, the more frequently CloudFront forwards another request to your origin to determine whether the object has changed and, if so, to get the latest version.
* **Caching based on query string parameters**
* **Caching based on cookie values** - create separate cache behaviors for static and dynamic content, and configure CloudFront to forward cookies to your origin only for dynamic content. 
* **Caching based on request headers** - if you configure CloudFront to cache based on request headers, you can improve caching if you configure CloudFront to forward and cache based on only specific headers instead of forwarding and caching based on all headers. Also try to avoid caching based on headers that have large number of unique values.
* **Remove `Accept-Encoding` header when compression is not needed** - by default, when CloudFront receives a request, it checks the value of the `Accept-Encoding` header. If the value of the header contains `gzip`, then CloudFront adds the header `Accept-Encoding: gzip` to the cache key and then forwards it to the origin. The behavior ensures that CloudFront serves either an object or a compressed version of the object, based on the value of the `Accept-Encoding` header. If compression is not enabled - because the origin does not support it, CloudFront doesn't support it, or the content is not compressible - you can increase the cache hit ratio by specifying different behavior.
* **Serve media content by using HTTP** - you can use CloudFront to deliver on-demand video or live streaming video using any HTTP origin. One way you can setup video workflows in the cloud is by using CloudFront together with AWS Media Services.



