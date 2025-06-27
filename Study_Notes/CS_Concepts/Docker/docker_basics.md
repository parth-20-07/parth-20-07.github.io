---
id: sw_essentials
aliases:
  - Docker Basics
tags:
---

# Docker Basics

## Background

Docker is a platform used to develop, ship, and run applications inside containers. A container is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, runtime, system tools, libraries, and settings. Containers are isolated from each other and the host system.

Here’s a deeper dive into Docker and its advantages:

1. **What is Docker?**
    - **Containerization**: Docker allows you to package an application and its dependencies into a single unit called a container. This container can be moved across different stages of development or different environments (like dev, staging, and production) ensuring consistency.
    - **Images and Containers**: In Docker, an “image” is a lightweight, stand-alone, executable software package that includes everything needed to run a piece of software. A “container” is a running instance of an image.
    - **Docker Engine**: This is the heart of Docker. It’s responsible for creating, running, and managing containers.
    - **Docker Hub**: A cloud-based registry where Docker users and partners can create, test, store, and distribute container images.
2. **Why Use Docker?**
    - **Consistency**: One of the most significant benefits of Docker is its ability to provide consistent environments. Developers can define the environment in which an application runs, which eliminates the “it works on my machine” problem.
    - **Isolation**: Containers run in isolation, ensuring that they don’t interfere with each other or with the host system. This makes it easier to manage dependencies and resources.
    - **Portability**: Since containers encapsulate all the dependencies an application needs to run, they can be moved seamlessly across different stages of development or different environments.
    - **Efficiency**: Containers are lightweight compared to traditional virtual machines because they share the host system’s OS kernel, rather than needing their operating system.
    - **Version Control for Environments**: Docker images can be versioned, allowing teams to track changes, roll back, and share versions among team members.
    - **Rapid Deployment**: Containers can be started in seconds, making scaling, deploying, and rollback faster and more efficient.
    - **Integration and CI/CD**: Docker fits well into continuous integration and continuous deployment (CI/CD) workflows. Changes can be packed into containers and tested as they progress through the pipeline, ensuring that each stage is tested in an environment that’s identical to production.
3. **Common Use Cases**:
    - **Microservices**: Breaking down applications into smaller services that run in their containers.
    - **Development Environments**: Ensuring that every developer works in a consistent environment.
    - **Testing**: Quickly creating disposable instances of an application to run tests.
    - **Scaling & Load Balancing**: Easily scaling out applications by running multiple containers across multiple host machines.
    - **Rapid Deployment**: Quickly rolling out features, patches, or updates.

In summary, Docker offers a solution to the problem of “how to get software to run reliably when moved from one computing environment to another.” This could be from a developer’s local machine to a test environment, from a staging environment into production, or a physical machine in a data centre to a virtual machine in a private or public cloud.

## Basics

### Installing Docker

Install docker in Ubuntu from a terminal using

```bash
sudo apt-get install docker.io
```

### Common Helping Commands

- List Docker Containers: `docker ps`
    - Add `a` flag for stopped containers
- List Images: `docker images`
- Delete Docker Image: `docker rmi <image-name>`
- Delete Docker Containers: `docker container <container-name/container-id>`
- Docker Resource Utilization: `docker stats`

### Pulling an Image from DockerHub

Syntax: `docker pull <image-name:version>`

If you don’t mention version, the latest one is pulled. There is only one copy of each unique version of images you pull. So if you retry pulling the same image, it won’t happen.

Example: Pull the centOS from DockerHub

```bash
docker pull centos
```

**Complete Docker Pull Guide:** [https://docs.docker.com/engine/reference/commandline/pull/](https://docs.docker.com/engine/reference/commandline/pull/)

### Loading an Image into a Container

Syntax: `docker run <flags> <image-name>`
Return: Container ID

Example: Running centOS Image with the name `cantcontainmyself`

```bash
docker run -d -t --name cantcontainmyself centos
```

Here:

- `-d`: Detached Mode - Run container in background and do not allow direct interaction with it.
- `-t`: Allocate a pseudo-tty
- `--name <name>`: Allows you to manually name a container which can help you to remember which container does what

**Complete Docker Run Guide**: [https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)

### Executing Commands in the Container

Syntax: `docker exec <flags> <container-id/container-name>`

Example: Using `cantcontainmyself` container.

```bash
docker exec -it cantcontainmyself bash
```

Here: - `-i`: Interactive Mode - Keeps STDIN Open. ie You can input keyboard values to it. - `-t bash`: Allocate Pseudo-TTY of type bash. Exact tty type can be found using `docker ps`.

Exit from container using `exit` keyword.

**Complete Docker Exec Guide**: [https://docs.docker.com/engine/reference/commandline/exec/](https://docs.docker.com/engine/reference/commandline/exec/)

### Starting and Stopping Containers

- Stop a running container using:`docker stop <container-name>`
- Start a stopped container using: `docker start <container-name>`

## Resources
