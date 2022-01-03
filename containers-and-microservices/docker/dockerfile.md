---
description: What Dockerfile is and common commands
---

# Dockerfile

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

## Instructions

* :arrow\_forward: **FROM** - The `FROM` instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile must start with a `FROM` instruction. The image can be any valid image
* :arrow\_forward: **ENV** - The `ENV` instruction sets the environment variable
* :arrow\_forward: **RUN** - The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`.
* :arrow\_forward:**LABEL** - The `LABEL` instruction adds metadata to an image. A `LABEL` is a key-value pair. To include spaces within a `LABEL` value, use quotes and backslashes as you would in command-line parsing.
* :arrow\_forward:**EXPOSE** - The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.
* :arrow\_forward:**COPY** - The `COPY` instruction copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`
* :arrow\_forward:**WORKDIR** - The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the `Dockerfile`. If the `WORKDIR` doesn’t exist, it will be created even if it’s not used in any subsequent `Dockerfile` instruction.
* :arrow\_forward:**CMD** - Specifies the command to run when container starts. If you would like your container to run the same executable every time, then you should consider using `ENTRYPOINT` in combination with `CMD`

Order of instructions in Dockerfile matters, as all commands on Dockerfile as executed sequentially, and some instruction resulting in its own layer, increasing image size as a result. Instructions that are changing more frequently should be placed closer to the bottom, and instructions that don't change or change less frequently, should be placed higher in the Dockerfile. When one of the instruction changes, `COPY` for instance, all subsequent instructions must be re-run.

For more best practices see: [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile\_best-practices/)

## Sample Dockerfile

```yaml
# From Node 10, Alpine 3.9 OS.
FROM node:10-alpine3.9

# Current directory.
WORKDIR /usr/app

# Install NPM packages
COPY ./package.json ./
RUN npm install

# Copy app files
COPY ./*.js ./

# Run app
CMD ["npm", "start"]
```

## Building image

