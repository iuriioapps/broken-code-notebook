# Storage in Kubernetes

* Storage and stateful workloads are harder in all systems
* Containers make it both harder and easier than before
* StatefulSets is a new resource type, making Pods more sticky
* Avoid stateful deployments for first few deployments until you're good at the basics
* Use Database-as-a-Service whenever you can
* Creating and connecting volumes: 2 types
  * Volumes
    * Tied to a lifecycle of a Pod
    * All containers in a single pod can share them
  * PersistentVolumes / PersistentVolumeClaims
    * Created at a cluster level, outlives a Pod
    * Separates a storage config from Pod using it
    * Multiple Pods can share them
* CSI \(Container Storage Interface\) plugins are the new way to connect to storage

