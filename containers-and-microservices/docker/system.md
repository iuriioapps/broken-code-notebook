---
description: System
---

# System

#### Get information about Docker disk usage

```text
docker system df
```

will show information about local images, containers, volumes and build cache.

#### Remove unused images

```text
docker image prune
```

#### Remove stopped containers

```text
docker container prune
```

#### Cleanup system

pass `-a` to "nuke" pretty much everything, that is not running

```text
docker system prune
```

