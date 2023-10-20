# Docker Repo

## What and why Docker?
It's a virtualization software that makes developing and deploying much easier.

It packages applications with all necessary dependencies, configurations, system tools and runtimes in something called **container**.

The container is a standardized unit that allows to run everything necessary that is configured in it. Also, it's a very easily shareable, portable and distributed.

### Which problems does Docker solve?
Before the containers, the developers should install and configure all tools and dependencies directly on their OS on their local machines. So the problems could be:
- Different installation process for each OS;
- The process could be very exhausting;
- Versions problems;
- Versioning and historic of changes tracking;

The container is an isolated and pre configured environment.
It also allows to run different environment with different versions of a tool or app without any conflicts.

### How was the deployment process BEFORE containers?
The development team produces an application artifact or a package and installation instructions.

Then these informations goes to Ops team, that performs all the configuration process.


## Docker vs. Virtual Machines
The main difference refers to what part/layer of the OS do they virtualize.

The Conatiners virtualize the applications layer. On the other hand, the VM virtualizes both Application and Kernel layer.

The Containers therefore came to be much smaller than the VM.

Also, containers are faster for initializing.

Container are only compatible with Linux distros, while VM are compatible to all. But they developed the Docker Desktop that uses a hypervisor that allows to use Linux applications on mac or windows. It's a lightweight framework.

## Docker Desktop

### Docker Engine
-> A server with a long-running daemon process "dockerd"

-> Manages the images and containers

### Docker CLI - Client
-> CLI for Docker Command for server interaction


## Images vs. Containers

### Images
It's an executable application artifact. Includes all the application source code, but also a complete environment configuration. Allows to add environment variables, create directories, files, etc.

For example, it can include:
- Application Layer: JS App;
- OS Layer: Linux;
- Any other services: node, npm

> command to show all the images and its informations, sucha as:
REPOSITORY, TAG, IMAGE ID, CREATED, SIZE
```shell 
docker images
```

**So a image is an immutable template that defines how  container will be initialized**

### Containers
From the image, a container actually starts the whole application and environment.

It's a running instance of an image.


> command to show all the processes and its informations, such as:
CONTAINER ID, IMAGE, COMMAND, CREATED, STATUS, PORTS, NAME
```shell 
docker ps
```

## Docker Registries
It's a storage and distribution system for Docker Images.

There're **Official** and **Community** images. The first one from official providers, such as Redis, Postgres, MongoDB.

The Docker itself hosts one of the biggest Docker Registry called "Docker Hub".

There's a team responsible for checking, reviewing and publishing all content in the Docker official image.

## Image versioning
Docker images are versioned as well and they are identified by tags.

## Main Docker Commands
```shell 
### PULL ###
docker pull {name of the image}:{tag}
docker pull nginx:1.23

### RUN ###
docker run {name of the image}:{tag}
docker run -d {name of the image}:{tag} 
# -d or --detach--> detach
# the docker run command automatically pulls a image from the registry if it's not locally


### LOGS ###
docker logs {id of the container}
```

## Port Binding
### Container port vs. Host port
An application inside a container runs in an isolated Docker Network. So it's impossible to access from the local computer browser or in another way.

So it's necessary to **expose** the Container Port to the host (the machine the container runs on). This is called Port Binding.

To do that:
```shell
docker run -d -p {host port}:{container port} {name of the image}:{tag}
```

> It's a good practice to use the same port in both, container and host.

## Starting and Stopping Containers
The Docker always start a new container everytime that one is started with the `docker run` command. It doesn't re-use.


To stop a container which is running:
```shell
docker stop {id of the container OR name of the container}
```

To start a container which is stopped:
```shell
docker start {id of the container OR name of the container}
```

To see all containers, even the stopped ones:
```shell
docker ps -a
# -a all flag
```

To set a meaningful name for the container (Docker autogenerate if not provided):
```shell
docker run --name {name of the container} -d -p {host port}:{container port} {name of the image}:{tag}
```

## Public and Private Docker Registries

### Docker Hub
Is the largest public registry and anyone can search and download Docker Images from there.

### Private
A company usually don't wanna your data exposed and for its own container they can use thier own registry.

Some cloud platforms provides a private and secure space and tool to store them.

It's also possible to use another tools like: Nexus, Docker Hub (private feature)

## Registry vs. Repository
### Registry
It's a service providing storage that can be hosted by a third-party service or by yourself.

Inside a Registry, it's possible to have a collection of repositories for each application.

So each application gets its own repository, and in that repository you can store different images of the same application.

### Repository
So, as we saw, it's a collection of related images with same name but different versions.

## Dockerfile - How to Dockerize an app (NodeJS example)
The companies create their own custom images for their applications.

We already know that our containerized applications makes the deployment and running process easier. But how to build the image before?

The `Dockerfile` is the file that contains all the commands and instructions to assemble an image. Afterwards, Docker can build the image by reading these instructions.

### Structure of Dockerfiles
The Dockerfiles start from a parent image, called "base image" with the command `FROM`.

Every image consists in multiple images layers.

To run commands in a shell inside the container can use the `RUN` command.

To copy files from the host directory into the container is used the `COPY {file or directories on host machine} {directory destination on container}` command.

To set the working directory for all following commands, like `cd`, `ls`, etc. is used the command `WORKDIR {directory}`

To run an instruction when a container is started it's used `CMD [....]`. **Important:** There can only be one `CMD` instruction in Dockerfile.

## Build a Image
```shell
docker build -t {name}:{tag} {path to Dockerfile} 

# -t Sets a name and optionally a tag in the "name:tag" format
```
As a Docker image consists of layers, each command will create one layer.

These layers are stacked and each one is a delta of the changes from the previous one.

## Dockerizing your application
Write a Dockerfile --> Build a Docker image --> Run it as a Docker container.