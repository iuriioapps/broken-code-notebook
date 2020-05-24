# ElastiCache

Amazon ElastiCache is a web service that makes it easy to deploy, operate and scale an in-memory cache in the cloud. The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.

ElastiCache supports two open-source in-memory caching engines:

* Memcached
* Redis

## Monitoring

When it comes to monitoring caching engines, there are 4 important metrics to look at:

* CPU utilization
* Swap usage
* Evictions
* Concurrent connections

### CPU utilizations

* Memcached
  * Multi-threaded
  * Can handle loads up to 90%. If it exceeds 90%, add more nodes to the cluster
* Redis
  * Not multi-threaded. To determine the point in which to scale, take 90% and divide by the number of cores.

### Swap usage

* Memcached
  * Should be around 0 most of the time and should not exceed 50 Mb.
  * If this exceeds 50 Mb, you should increase the `memcached_connections_parameter`
* Redis
  * No swap usage metrics, instead use `reserved_memory`

### Evictions

* Memcached
  * There is no recommended setting. Choose a threshold based off your application. Either scale up \(increase the memory of the existing nodes\) or scale out \(add more nodes\).
* Redis
  * There is no recommended setting, choose a threshold based off your application
  * Only scale out \(add read replicas\)

### Concurrent connections

* Memcached and Redis
  * There is no recommended setting
  * If there is a spike in number of concurrent connections, this can either mean a large traffic spike or your application is not releasing connections as it should be.

