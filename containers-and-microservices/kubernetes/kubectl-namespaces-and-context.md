# Kubectl Namespaces and Context

* Namespaces limit scope, aka 'virtual clusters'
* Not related to Docker / Linux namespaces
* Won't need them in small clusters
* There are some built-in, to hide system stuff from kubectl users
  * `kubectl get namespaces`
  * `kubectl get all --all-namespaces`
* Context changes kubectl cluster and namespace, see `~/.kube/config` file
  * `kubectl config get-contexts`
  * `kubectl config set*` &lt;-- see help for this

