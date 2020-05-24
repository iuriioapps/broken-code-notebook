# Labels and Annotations

* Labels go under metadata: in your YAML
* Simple list of `key: value` for identifying your resource later by selecting, grouping, or filtering for it
* Common examples include 'tier: frontend, app: api, env: prod, customer: acme.co'
* Not meant to hold complex, large or non-identifying information, which is what annotations are for
* Filter a get command by label:

    `kubectl get pods -l app=nginx`

* Apply only matching labels:

    `kubectl apply -f myfile.yml -l app=nginx`



