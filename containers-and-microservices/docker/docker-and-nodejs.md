---
description: Recommendations for dealing with NodeJS in Docker environments
---

# Docker & NodeJS

## Compose YAML v2 vs v3

* v3 does not replace v2
* v2 focus: single-node dev / test
* v3 focus: multi-node orchestration
* If not using Swarm / Kubernetes, stick to v2

### Some best practices

* Use COPY to copy files, use ADD only if there is a reason to do so
* Always cleanup after npm install

    `RUN npm install && npm cache clean --force`

* CMD 'node', instead of 'npm'. Reasons:
  * requires another application to run \(npm launches, and then starts node\)
  * not as literal in Dockerfiles
  * 'npm' does not work well as an 'init' or 'PID 1' process, container will not be terminated correctly as npm might not send SIGTERM to node process
* Use WORKDIR not RUN mkdir \(unless you need to 'chown'\)

## FROM base image guidelines

* Stick to even numbered major releases
* Don't use `:latest` tag, be specific about the version
* Start with Debian if migrating
* Move to Alpine later \(or start with Alpine, if it's a new project\)
* Don't use :slim \(it's just a smaller version of Debian, and using Alpine is preferred\)
* Don't use :onbuild

## Least privilege: Using 'node' User

* Official node images have a '`node`' user, but it's not used by default
* Do this after 'apt/apk' and 'npm install -g'
* Do this before 'npm install'
* This may cause permissions issues with write access
* May require 'chown node:node'
* Change user from 'root' to 'node': USER node
* Only the CMD, ENTRYPOINT and RUN will use a user set by user, and everything else will use root, and that can lead to problems \(like installing packages, writing to files, bind mounts permissions, etc\)
* Set permissions on application directory

    `RUN mkdir app && chown -R node:node .`

* Run a command as a root in a container

    `docker-compose exec -u root`

  since docker will assume 'node' as default user otherwise

## Making Images Efficiently

* Pick proper FROM \(be as specific as possible with image tags\)
* Line order matters \(layers that change more often should be close to the bottom, and vice versa, layers that don't change that often should be close to the top \)
* COPY package.json \(and lock\) first, run npm install and then copy rest of the code

    `COPY package*.json ./`

    `RUN npm install && npm cache clean --force`

    `COPY . .`

* One 'apt-get' \(or other package manager\) line per Dockerfile

    `RUN apt-get update && apt-get install curl && apt-get clean` \(or apt-get autoclean\)

## Node process management in containers

* No need for nodemon, forever or pm2 on server
* Use nodemon in dev for watching file changes
* Docker manages app start, stop, restart, healthcheck
* Node multi-threaded: Docker manages multiple "replicas"
* One npm / node problem: they don't listen for proper shutdown signal by default
* PID 1 \(process identifier\) is the first process in a system \(or container\), AKA init
* Init process in the container has two jobs:
  * reap zombie processes
  * pass signals to subprocesses
* Zombie processes is not a big Node issue
* Focus on proper Node shutdown
* Docker uses Linux signals to stop app \(SIGINT / SIGTERM / SIGKILL\)
* SIGINT / SIGTERM allow graceful stop
* NPM doesn't respond to SIGINT / SIGTERM
* Node doesn't respond by default, but a code can be added to handle it
* Docker provides a init PID 1 replacement option \(tini, for instance\)

### Proper Node shutdown options

* Temporary: use --init to fix Ctrl+C for now

    `docker run --init -d nodeapp`

* Workaround: add 'tini' to your image

    `RUN apk add --no-cache tini`

    `ENTRYPOINT ["sbin/tini", "--"]`

    `CMD ["node", "./bin/www"]`

* Production: Your app captures SIGINT / SIGTERM for proper exit

```javascript
// quit on ctrl-c when running docker in terminal
process.on('SIGINT', function onSigint () {
	console.info('Got SIGINT (aka ctrl-c in docker). Graceful shutdown ', new Date().toISOString());
  shutdown();
});

// quit properly on docker stop
process.on('SIGTERM', function onSigterm () {
  console.info('Got SIGTERM (docker container stop). Graceful shutdown ', new Date().toISOString());
  shutdown();
})

// shut down server
function shutdown() {
  // NOTE: server.close is for express based apps
  // If using hapi, use `server.stop`
  server.close(function onServerClosed (err) {
    if (err) {
      console.error(err);
      process.exitCode = 1;
		}
		process.exit();
  })
}
```

## Multi-stage builds

* New feature in Docker 17.06 \(mid-2017\)
* Build multiple images from one Dockerfile
* Those images can FROM each other
* COPY files between them
* Space and security benefits \(included only what is needed for the app, nothing else\)
* Great for "artifact only" \(see above\)
* Great for dev + test + prod

Example:

```yaml
# a base stage for all future stages
# with only prod dependencies and
# no code yet
FROM node:10 as base
ENV NODE_ENV=production
WORKDIR /app
COPY package*.json ./
RUN npm install --only=production \
    && npm cache clean --force
ENV PATH /app/node_modules/.bin:$PATH

# a dev and build-only stage. we don't need to 
# copy in code since we bind-mount it
FROM base as dev
ENV NODE_ENV=development
RUN npm install --only=development
CMD ["/app/node_modules/.bin/nodemon"]

FROM dev as build
COPY . .
RUN tsc
# you would also run your tests here

# this only has minimal deps and files
FROM base as prod
COPY --from=build /app/dist/ .
CMD ["node", "app.js"]
```

* To build image from specific stage 

    `docker build -t myapp:prod --target prod .`

## Cloud Native application guidelines

* Follow 12factor.net principles, especially:
  * Use environment variables for config. Docker and Compose are great at this with multiple options
  * Log into stdout / stderr
  * Pin all versions: images, packages installed locally and globally
  * Graceful exit with SIGINT / SIGTERM
* Create a .dockerignore
  * prevent bloat and unneeded files
    * .git/
    * node\_modules/
    * npm-debug/
    * docker-compose\*.yml
    * any other file\(s\) or folder\(s\) that should not be a part of the image
  * not needed but useful in image
    * Dockerfile
    * README.md

## Compose Project Tips: Do's

```yaml
version: '2.4'
# GOOD: stick with 2.x versions if you aren't using this yaml file with swarm/k8s

services:

  ghost:
    image: ghost:alpine
    ports:
      - 8090:2368
    # GOOD: v2 compose file supports depends_on, it's useful for establishing 
    # dependencies. For true wait-for-it style, you need a healthcheck: and 
    # condition: object
    depends_on:
      - db
    # GOOD: 
    volumes:
      - ./content:/var/lib/ghost/content:delegated
    environment:
      database__client: mysql
      database__connection__host: db
      database__connection__user: root
      database__connection__password: YOURDBPASSWORDhere
      database__connection__database: ghost

  db:
    image: mysql:5
    volumes:
      #GOOD: created a named volume so our data is kept between docker-compose ups
      - db:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: YOURDBPASSWORDhere

volumes:
  db:
```

## Compose project tips: Don'ts

```yaml
version: '2.4'

services:

  my-ghost:
    image: ghost:alpine
    # BAD: just make your service name the desired DNS name. docker-compose
    # handles the rest. If more then one replica are started, docker-compose
    # creates aliases for them (DNSRR)
    alias: ghost
    # BAD: links are legacy. All compose services are added to a default
    # bridge network and can freely talk via service name as their DNS name
    links:
      - db
    ports:
      - 8090:2368
    # BAD: all ports are exposed in docker networks by default
    # so this does nothing. Note it's still good to have EXPOSE
    # in dockerfile for better documentation
    expose:
      - "2368"
    # BAD: see networks at bottom. A default network is already created, making
    # this one moot
    networks:
      - ghostnetwork
    # BAD: if bind-mounting folders or files to host, always use relative file
    # paths (starting with .). This makes your compose file reusable for others, 
    # and won't break if you move your project around.
    volumes:
      - /my/path/on/host/:/var/lib/ghost
    environment:
      database__client: mysql
      database__connection__host: db
      database__connection__user: root
      database__connection__password: YOURDBPASSWORDhere
      database__connection__database: ghost

  db:
    image: mysql:5.7
    # BAD: this is rarely needed. You can use docker-compose commands that 
    # reconize service names, like `docker-compose logs db` or 
    # `docker-compose exec db bash` so you rarely need to control container 
    # directly from docker CLI (which is why people usually manually set 
    # container_name
    container_name: db
    # BAD: see my-ghost:links above
    links:
      - my-ghost
    # BAD: see networks at bottom. A default network is already created, making
    # this one moot
    networks:
      - ghostnetwork
    volumes:
      # BAD: don't bind-mount databases to host OS. You'll get bad performance
      # and many times it won't even work. Best to use named volumes.
      - ./database:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: YOURDBPASSWORDhere

volumes:
  db:
    # BAD: no need to set driver local, it's default
    driver: local

networks:
  # BAD: no need to create a network if only one is needed. By default
  # compose creates a bridge network for all services to connect to
  # custom networks are only needed if you need more then one, or special
  # drivers or settings are needed
  ghostnetwork:
    driver: bridge
```

## node\_modules in Images

* Images should not be build with node\_modules from host, some NPM packages are build when they're installed for specific architecture \(for example, node-gyp\).
* Add .gitignore to exclude node\_modules. This will have added benefit of not sending unnecessary payload as context when image builds.

## Startup order and dependencies

* Problem: multi-service apps start out of order, node might exit or cycle
* Multi container apps need:
  * dependency awareness
  * name resolution \(DNS\)
  * connection failure handling
* 'depends\_on' \(from compose.v2\): when "up X", start Y first
  * Fixes name resolution issues with "can't resolve "
  * Only for Compose, not for orchestration
  * compose V2: works with healthchecks like a "wait for script"
* Connection failure handling
  * restart: on-failure
    * good: helps slow db startup and NodeJS failing. Better: 'depends\_on'
    * bad: could spike CPU with restart cycling
  * Solution: build connection timeout, buffering, retries and exponential backoffs in your app

### Example of the compose with healthchecks

```yaml
version: '2.4'

services:

  frontend:
    image: nginx
    depends_on:
      api:
        # this requires a compose file version => 2.3 and < 3.0
        condition: service_healthy
        
  api:
    image: node:alpine
    healthcheck:
      test: curl -f http://127.0.0.1
    depends_on:
      postgres:
        condition: service_healthy
      mongo:
        condition: service_healthy
      mysql:
        condition: service_healthy

  postgres:
    image: postgres
    healthcheck:
      test: pg_isready -U postgres -h 127.0.0.1

  mongo:
    image: mongo
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongo localhost:27017/test --quiet

  mysql:
    image: mysql
    healthcheck:
      test: mysqladmin ping -h 127.0.0.1
    environment:
      - MYSQL_ALLOW_EMPTY_PASSWORD=true
```

## Templating 

A relatively new and lesser-known feature is Extension Fields, which lets you define a block of text in Compose files that is reused throughout the file itself. This is mostly used when you need to set the same environment objects for a bunch of microservices, and you want to keep the file DRY:

```yaml
version: '3.4'
 
x-logging: 
  &my-logging
    options: 
      max-size: '1m' 
      max-file: '5'
 
services: 
  ghost: 
    image: ghost 
    logging: *my-logging 
  nginx: 
    image: nginx 
    logging: *my-logging 
```

You'll notice a new section starting with an x-, which is the template, that you can then name with a preceding & and call it from anywhere in your Compose file with \* and the name. Once you start to use microservices and have hundreds or more lines in your Compose file, this will likely save you considerable time and ensure consistency of options throughout.

## Avoiding devDependencies in Production

* Multi-stage Dockerfile can solve this
* prod stages: `npm i --only=production`
* dev stages: `npm i --only=development`
* Use `npm ci` to speed up builds
* Ensure NODE\_ENV is set

## Dockerfile documentation

* Document every line that isn't obvious
* FROM stage, document why it's needed
* COPY = don't document
* RUN - maybe document
* Add LABELs
  * LABEL has OCI standard now
  * LABEL org.opencontainers.image.
  * Use ARG to add info to labels like build date or git commit
  * Docker Hub has built-in env.vars for use with ARGs
* Inlcude "RUN npm config list" to get information about NPM / Node in the logs

## Dockerfile Healthchecks

* Always include HEALTHCHECK
* Docker run and docker-compose: info only
* Docker Swarm: Key for uptime and rolling updates
* Kubernetes: Not used, but helps in others making readiness / liveness probes

```yaml
# option 1, using curl to GET the default app url
# NOTE: ensure curl is installed in image
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

# option 2, using curl to GET a custom url with app logic
# NOTE: ensure curl is installed in image
HEALTHCHECK CMD curl -f http://localhost/healthz || exit 1

# option 3, a custom code healthcheck that could
# do a lot more things then a simple curl
# or simply avoid needing curl to begin with
HEALTHCHECK --interval=30s CMD node hc.js
```

