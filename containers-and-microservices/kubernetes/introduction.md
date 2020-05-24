# Introduction

## What is Kubernetes

* Kubernetes \(k8s\) - popular container orchestrator
* Container orchestration - make many servers act like one
* Released by Google in 2015, maintained by large community
* Runs on top of Docker \(usually\) as a set of APIs in containers
* Provides API / CLI to manage containers across servers
* Many clouds provide it for you
* Many vendors make a "distribution" of it

## Why Kubernetes

* Orchestration: Next logical step in journey to a faster DevOps
* First, understand why you _may_ need orchestration, as not every solution needs orchestration
* Number of servers + change rate = benefits of orchestration
* Then, decide which orchestrator
* If Kubernetes, decide which distribution
  * cloud or self-managed \(Docker Enterprise, Rancher, OpenShift, Canonical, VMWare PKS\)
  * don't usually need pure upstream version of k8s from Github

## Kubernetes or Swarm

* Kubernetes and Swarm are both container orchestrators
* Boths are solid platforms with vendor backing
* Swarm: easier to deploy / manage
* Kubernetes: More features and more flexibility
* Understand both and know your requirements

### Advantages of Swarm

* Comes with Docker, single vendor platform
* Easiest orchestrator to deploy / manage yourself
* Follows 80/20 rules \(somewhat\), 20% of features compared to k8s cover 80% of use cases
* Run anywhere Docker does
  * local, cloud, datacenter
  * ARM, Windows, 32-bit, etc

### Advantages of Kubernetes

* Clouds will deploy / manage Kubernetes for you
* Infrastructure vendors are making their own distributions
* Widest adoptions and community
* Flexible: covers widest set of use cases
* "Kubernetes first" vendor support
* "No one ever got fired for buying IBM"
  * picking solutions isn't 100% rational
  * trendy, will benefit your career
  * CIO / CTO checkbox



