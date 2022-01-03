# Kubernetes Management Techniques

## Run, Create and Expose Generators

* These commands use helper templates called "generators"
* Every resource in Kubernetes has a specification or "spec"
* `kubectl create deployment sample --image nginx --dry-run -o yaml`
* You can output those templates with `--dry-run -o yaml`
* You can use those YAML default as a starting point for your own templates
* Generators are "opinionated defaults"

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: sample
  name: sample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sample
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: sample
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

## Three Management Approaches

* **Imperative commands**: run, expose, scale, edit, create deployment
  * best for dev / learning / personal projects
  * easy to learn, hardest to manage over time
* **Imperative objects**: create -f file.yml, replace -f file.yml, delete -f file.yml
  * good for prod of small environments, single file per command
  * store your changes in git-based yaml files
  * hard to automate
* **Declarative objects**: apply -f file.yaml or dir\\, diff
  * best for prod, easier to automate
  * harder to understand and predict changes
* **Most important rule**:
  * don't mix these three approaches

