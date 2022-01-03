---
description: Docker container clusters
---

# Swarm

## docker swarm init

* Lots of PKI and security automation
  * Root signing certificate create for our Swarm
  * Certificate is issued for first Manager node
  * Join tokens are created
* Raft database create to store root CA, configs and secrets
  * Encrypted by default on disk
  * No need for another key / value system to hold orchestration / secrets
  * Replicates logs amongst Managers via mutual TLS in "control plane"

`docker swarm join-token manager`

* scaling up to multiple containers:
* `docker service update  --replicas <#>`



## Swarm Stacks: Production grade compose

* IN Docker 1.13 Docker adds a new layer of abstraction to Swarm called Stacks
* Stacks accept Compose files as their declarative definition for services, networks and volumes.
* We use '`docker stack deploy`' rather than docker stack create
* Stacks manages all those objects for us, including overlay network per stack. Adds stack name to start of their name.
* New '`deploy`:' key in Compose file, does not support '`build`:'
* Compose now ignores '`deploy`:'; Swarm ignores '`build`:'
* `docker-compose` CLI is not needed on Swarm server

`docker stack deploy -c my-stack.yml demoapp`\
`docker stack services demoapp`

## &#x20;Swarm Secrets Storage

* Easiest "secure" solution for storing secrets in Swarm
* Supports generic strings or binary content up to 500Kb in size
* Doesn't require apps to be rewritten
* As of Docker 1.13 Swarm Raft DB is encrypted on disk
* Only stored on disk on manager nodes
* Default is Managers and Workers "control plane" is TLS + Mutual Auth
* Secrets are first stored in Swarm then assigned to a Service
* Only containers in assigned Services can see them
* They look like files in container but are actually in-memory file system
* `/run/secrets/{secrets-name}` or `/run/secrets/{secrets-alias}`
* Local docker-compose can use file-based secrets, but not secure

### Option 1 - create from file&#x20;

`docker secret create psql_user psql_user.txt`

### Option 2 - Create from STDIN&#x20;

`echo "MyDbPassWORD" | docker secret create psql_user -`

// Enable secret in the service&#x20;

`docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres`

With compose 3.1 secrets are supported inside Compose files

```yaml
secrets:
  my_first_secret:
    file: ./secret_data
  my_second_secret:
    external: true
    name: redis_secret
```

