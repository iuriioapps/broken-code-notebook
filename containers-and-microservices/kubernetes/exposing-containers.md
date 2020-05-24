# Exposing containers

## Exposing containers

* `kubectl expose` creates a service for existing pods
* A service is a stable address for pod\(s\)
* If we want to connect to pod\(s\) we need a service
* CoreDNS \(part of control plane\) allows us to resolve services by name
* There are different types of services
  * **ClusterIP** \(default\)
    * Single, internal virtual IP allocated
    * Only reachable from within cluster \(nodes and pods\)
    * Pods can reach service on apps port number
    * Always available in Kubernetes
  * **NodePort**
    * High port allocated on each node
    * Port is open on every node's IP
    * Anyone can connect \(if they can reach node\)
    * Other pods need to be updated to this port
    * Always available in Kubernetes
  * **LoadBalancer**
    * Controls LB endpoint external to the cluster
    * Only available when infrastructure provider gives you a load balancer \(AWS ELB, etc\)
    * Creates NodePort + ClusterIP services, tells external load balancer to send traffic to NodePort
  * **ExternalName**
    * Adds CNAME DNS records to CoreDNS only
    * Not used for pods, but for giving pods a DNS name to use for something outside Kubernetes

### Creating a ClusterIP service

* `kubectl get pods -w`
* `kubectl create deployment httpenv --image=bretfisher/httpenv`
* `kubectl scale deployment/httpenv --replicas=5`
* `kubectl expose deployment/httpenv --port 8888`
* `kubectl get service`
* `kubectl run --generator run-pod/v1 tmp-shell --rm -it --image=bretfisher/netshoot -- bash`
* `curl httpenv:8888`

```text
{
  "HOME": "/root",
  "HOSTNAME": "httpenv-7cc9888d59-s9sj6",
  "KUBERNETES_PORT": "tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP": "tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP_ADDR": "10.96.0.1",
  "KUBERNETES_PORT_443_TCP_PORT": "443",
  "KUBERNETES_PORT_443_TCP_PROTO": "tcp",
  "KUBERNETES_SERVICE_HOST": "10.96.0.1",
  "KUBERNETES_SERVICE_PORT": "443",
  "KUBERNETES_SERVICE_PORT_HTTPS": "443",
  "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
}
```

### Creating a NodePort service

* `kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort`
* `kubectl get services`

| NAME | TYPE | CLUSTER-IP | EXTERNAL-IP | PORT\(S\) | AGE |
| :--- | :--- | :--- | :--- | :--- | :--- |
| httpenv | ClusterIP | 10.107.92.157 | none | 8888/TCP | 24h |
| httpenv-np | NodePort | 10.109.25.172 | none | 8888:31642/TCP | 8s |
| kubernetes | ClusterIP | 10.96.0.1 | none | 443/TCP | 2d13h |

* 8888:31642/TCP - from port 8888 inside the cluster to port 31642 externally accessible
* Default port ranges in the cluster 30000-32767
* These 3 service types are additive, each one creates one above it:
  * ClusterIP
  * NodePort
  * LoadBalancer
* `curl localhost:31642` \(on Linux - goes directly to NodePort, on Mac/Windows goes through Docker VPNKit\)

### Creating a LoadBalancer service

* Docker Desktop provides a built-in LoadBalancer that publishes the port on localhost
* `kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer`
* `curl localhost:8888`

| NAME | TYPE | CLUSTER-IP | EXTERNAL-IP | PORT\(S\) | AGE |
| :--- | :--- | :--- | :--- | :--- | :--- |
| httpenv | ClusterIP | 10.107.92.157 | none | 8888/TCP | 24h |
| httpenv-np | NodePort | 10.109.25.172 | none | 8888:31642/TCP | 8s |
| httpenv-lb | LoadBalancer | 10.98.39.13 | localhost | 8888:30695/TCP | 19s |
| kubernetes | ClusterIP | 10.96.0.1 | none | 443/TCP | 2d13h |

### Cleanup

* `kubectl delete service/httpenv service/httpenv-np service/httpenv-lb deployment/httpenv`

## Kubernetes Service DNS

* Starting with 1.11, internal DNS is provided by CoreDNS
* Like Swarm, this is DNS-based service discovery 
* Accessing services by hostname works only if they're in the same namespace
* `kubectl get namespaces`
* Services also have a FQDN

    `curl <hostname>.<namespace>.svc.cluster.local`



