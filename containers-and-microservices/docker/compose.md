---
description: Running multiple containers
---

# Compose

### Why

* Configure relationships between containers
* Save docker container run settings in easy-to-read file
* create one-liner development environment startups

Comprised of 2 separate but related things:

* YAML formatted file that describes solution options for:
  * containers
  * volumes
  * networks
* A CLI tool docker-compose used for local dev/test automation

Installed automatically on Mac and Windows with Docker Desktop, for Linux must be downloaded separately.

### Links

* [Compose on GitHub](https://github.com/docker/compose)
* [Compose documentation](https://docs.docker.com/compose/compose-file/)

### Example

```yaml
version: '3'

services:
  drupal:
    image: drupal:8-apache
    ports:
    - '8080:80'
    volumes:
    - drupal-modules:/var/www/html/modules
    - drupal-profiles:/var/www/html/profiles
    - drupal-sites:/var/www/html/sites
    - drupal-themes:/var/www/html/themes
    restart: always
  postgres:
    image: postgres:10
    environment:
      POSTGRES_PASSWORD: pgPass@1
    restart: always

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:

```

Commands

To start

```yaml
docker-compose up [--build] [...options]
```

To tear down

```yaml
docker-compose down [-v] [...other options]
```

### Using compose with build

* compose can also build your images
* will build them with `docker-compose up` if not found in cache
* also rebuild with `docker-compose build`
* great for complex builds that have lots of vars or build args

Example

```yaml
version: '3'

services:
  proxy:
    build:
      context: .
      dockerfile: nginx.Dockerfile
    image: nginx-custom
    ports:
    - '80:80'
  web:
    image: httpd
    volumes:
    - ./html:/usr/local/apache2/htdocs/
```

### Restart policies

* `"no"` - never attempt to restart this container if it stops or crashes \(must be in ""\)
* `always` - if this container stops, for any reason, always attempt to restart it
* `on-failure` - only restart if the container stops with an error code
* `unless-stopped` - always restart unless we forcibly stop it



