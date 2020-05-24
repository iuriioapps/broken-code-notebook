# Declarative YAML

* Kubernetes configuration files - YAML or JSON
* Each file contains one or more manifests
* Each manifest describes an API object \(deployment, job, secret\)
* Each manifest needs 4 parts \(root key:values in the file\):
  * apiVersion - Which version of the Kubernetes API you’re using to create this object
  * kind - What kind of object you want to create
  * metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
  * spec - What state you desire for the object

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.17.3
    ports:
    - containerPort: 80
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.3
        ports:
        - containerPort: 80
```

Declaring multiple types in a single file - separate them with "---"

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-nginx-service
spec:
  type: NodePort
  ports:
  - port: 80
  selector:
    app: app-nginx
--- # !!! Separator
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app-nginx
  template:
    metadata:
      labels:
        app: app-nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.3
        ports:
        - containerPort: 80
```

* `kubectl api-resources`
* `kubectl api-versions`
* We can get all the keys each kind supports: `kubectl explain <kind> --recursive`
* We can drill down into subnodes of the kind:

    `kubectl explain <kind>.<subnode>`

    `kubectl explain services.spec`

### Dry Runs with Apply YAML

* dry run client-side only:

    `kubectl apply -f file.yml --dry-run`

* dry run server-side \(will compare current file with what's already deployed on server\):

    `kubectl apply -f file.yml --server-dry-run`

* see the diff visually:

    `kubectl diff -f file.yml`



