# CRD's and The Operator Pattern

## CRD's and The Operator Pattern

* You can add 3rd-party Resources and Controllers
* This extends Kubernetes API and CLI
* A pattern is starting to emerge of using these together
* Operator: automate deployment and management of complex apps, e.g. Databases, monitoring tools, backups, and custom ingresses

## Higher Deployment Abstractions

* All out `kubectl` commands just talk to the Kubernetes API
* Kubernetes has limited built-in templating, versioning, tracking and management of your apps
* There are now many tools to do that, but many are defunct
* Helm is the most popular
* 'Compose on Kubernetes' comes with Docker Desktop
* These are optional, and your distro may have a preference
* Most distros support Helm
* Many of the deployment tools have templating options
* You'll need a solution as a number of environments / apps grow
* Helm was the first 'winner' in this space, but can be complex
* Official 'Kustomize' \([https://github.com/kubernetes-sigs/kustomize](https://github.com/kubernetes-sigs/kustomize)\) feature works out-of-the-box
* `docker app` and `Compose-on-Kubernetes` are Docker's way

