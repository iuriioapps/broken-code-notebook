# Getting started

## Basic terms: system parts

* Kubernetes: the whole orchestration system (K8s "k-eights" or Kube for short)
* Kubectl: CLI to configure Kubernetes and manage apps (using "cube control" official pronunciation)
* Node: single server in the Kubernetes cluster
* Kubelet: Kubernetes agent running on nodes
* Control plane: set of containers that manage the cluster
  * Includes API server, scheduler, controller manager, etcd and more
  * Sometimes called the "master"

## Installing Kubernetes locally

* Kubernetes is a series of containers, CLI's and configurations
* Many ways to install:
  * MacOS: Docker Desktop - Enable in Settings. Sets up everything inside Docker's existing Linux VM
    * Runs, configures Kubernetes Master containers
    * Manages `kubectl` install and certificates
    * Easily install, disable and remove from Docker GUI
  * Docker Toolbox on Windows - MiniKube. Uses VirtualBox to make Linux VM
    * Doesn't install `kubectl`, has to be installed separately
  * Your own Linux host or VM - MicroK8s. Installs Kubernetes right on the OS
    * Uses snap (rather apt or yum) for install.
    * Control MicroK8s service via `microk8s.` commands
    * `kubectl` accessible via `microk8s.kubectl`
    * Add an alias to your shell (.bash\_profile): \
      `alias kubectl=microk8s.kubectl`
* Kuberneters in a Browser
  * [http://play-with-k8s.com](http://play-with-k8s.com)
  * katacoda.com&#x20;

## Kubernetes Container Abstractions

* **Pod**: one or more containers running together on one node. Basic unit of deployment. Containers are always in pods.
* **Controller**: for creating / updating pods and other objects. Controllers manage pods, you almost never manage pods yourself. There are many types of Controllers, including `Deployment`, `ReplicaSet`, `StatefulSet`, `DaemonSet`, `Job`, `CronJob`, etc.
* **Service**: network endpoint to connect to a pod.
* **Namespace**: Filtered group of objects in cluster.
* **Secrets**, **ConfigMaps**, etc.

## Kubernetes Run, Create and Apply

* Kubernetes is evolving, and so is the CLI
* We get 3 ways to create pods from the kubectl CLI
* > `kubectl run` (changing to be only for pod creation)
* > `kubectl create` (create some resources via CLI or YAML)
* > `kubectl apply` (create / update anything via YAML)

### Basic commands

* `kubectl version` - shows a version of Client and Server
* `kubectl cluster-info`
* `kubectl run my-nginx --image nginx` - this will create a pod, replica set and a deployment
* `kubectl get pods` - get current pods
* `kubectl get all` - get all components
* `kubectl delete deployment my-nginx` - delete named deployment

### Scaling ReplicaSets

*   Start new deployment for one replica/pod

    &#x20; `kubectl run my-apache --image httpd`
*   Scale deployment&#x20;

    &#x20; `kubectl scale deploy/my-apache --replicas 2`

    &#x20; `kubectl scale deployment my-apache --replicas 2` (these are same commands)

### Inspecting Deployments

*   Get logs

    &#x20; `kubectl logs deployment my-apache`
*   Get logs from pods with a given label

    &#x20; `kubectl logs -l run=my-apache`
*   Stern for Kubernetes can be used to tail logs from multiple pods ([https://github.com/wercker/stern](https://github.com/wercker/stern))

    &#x20; `brew install stern`

    &#x20; `stern pod-query`
*   Get pod info

    &#x20; `kubectl describe pod/<pod-id>` (pod-id can be obtained with kubectl get pods)\`

