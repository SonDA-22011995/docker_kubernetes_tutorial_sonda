- [Introduction to Containers](#introduction-to-containers)
  - [What are Container](#what-are-container)
    - [In the old days](#in-the-old-days)
    - [But these days](#but-these-days)
      - [One of the first approaches was to use **virtual machines (VMs)**.](#one-of-the-first-approaches-was-to-use-virtual-machines-vms)
      - [The second approaches is a software packaging mechanism is the **Docker container**](#the-second-approaches-is-a-software-packaging-mechanism-is-the-docker-container)
  - [Docker products](#docker-products)
    - [Docker Desktop](#docker-desktop)
    - [Docker Hub](#docker-hub)
    - [Docker Enterprise Edition](#docker-enterprise-edition)
  - [Containers vs Virtual Machines](#containers-vs-virtual-machines)
  - [Docker components](#docker-components)
    - [Docker Architecture Overview](#docker-architecture-overview)
      - [The Docker Client](#the-docker-client)
      - [B. The Docker Host](#b-the-docker-host)
      - [C. Image Registry (Container Registry)](#c-image-registry-container-registry)
    - [Common Workflows](#common-workflows)
      - [Scenario 1: Running a Container (`docker run`) from an image](#scenario-1-running-a-container-docker-run-from-an-image)
      - [Scenario 2: Building and Pushing (`docker build` \& `push`)](#scenario-2-building-and-pushing-docker-build--push)
    - [Key Concepts to Remember](#key-concepts-to-remember)
    - [Summary Table: Local vs. Remote](#summary-table-local-vs-remote)
- [Mastering Containers](#mastering-containers)
  - [Running the container](#running-the-container)
    - [Create and run a new container from an image](#create-and-run-a-new-container-from-an-image)
      - [Example](#example)
    - [List containers](#list-containers)
      - [Example](#example-1)
    - [Stop one or more running containers](#stop-one-or-more-running-containers)
  - [Container Lifecycle](#container-lifecycle)
  - [Docker Cleanup Commands Reference](#docker-cleanup-commands-reference)
    - [💡 Best Practices for Windows Developers](#-best-practices-for-windows-developers)

# Introduction to Containers

## What are Container

### In the old days

- Developers would develop new applications. Once an application was completed in their eyes, they would hand that application over to the operations engineers, who were then supposed to install it on the production servers and get it running. If the operations engineers were lucky, they even got a somewhat accurate document with installation instructions from the developers
- Usually, each application has some external dependencies, such as which framework it was built on, what libraries it uses, and so on. Sometimes, two applications use the same framework but of different versions that might or might not be compatible with each other

![In the old days](static/image/img0001.png)

### But these days

#### One of the first approaches was to use **virtual machines (VMs)**.

- Instead of running multiple applications all on the same server, companies would package and run a single application on each VM
- Unfortunately, that happiness didn’t last long. VMs are pretty heavy beasts on their own since they all contain a full-blown operating system such as Linux or Windows Server, and all that for just a single application

#### The second approaches is a software packaging mechanism is the **Docker container**

- Developers package their applications, frameworks, and libraries into Docker containers, and then they ship those containers to the testers or operations engineers
- The engineers know that if any container runs on their servers, then any other containers should run too
- Docker then coined the phrase Build, ship, and run anywhere

![But these days](static/image/img0002.png)

## Docker products

### Docker Desktop

- Docker Desktop (along with Docker Toolbox) is a developer-focused application designed for building, debugging, and testing containerized apps on macOS, Windows, and Linux.

- Key Takeaways
  - Target Audience: Primarily built for developers.

  - Functionality: Provides a complete development environment for the entire lifecycle (build, debug, and test) of dockerized services.

  - Integration: Deeply integrated with the host OS's network, filesystem, and hypervisor framework.

  - Performance: Recognized as the fastest and most reliable method to run Docker locally across different operating systems.

### Docker Hub

- Docker Hub is the most popular service for finding and sharing container images
- Docker images can be uploaded and shared inside a team, an organization, or with the wider publi

### Docker Enterprise Edition

- Docker EE – now owned by Mirantis – consists of the Universal Control Plane (UCP) and the Docker Trusted Registry (DTR), both of which run on top of Docker Swarm. Both are Swarm applications. Docker EE builds on top of the upstream components of the Moby project and adds enterprise-grade features such as role-based access control (RBAC), multi-tenancy, mixed clusters of Docker Swarm and Kubernetes, a web-based UI, and content trust, as well as image scanning on top.

## Containers vs Virtual Machines

![Containers vs Virtual Machines](static/image/img0003.png)

![Containers vs Virtual Machines](static/image/img0004.png)

| Feature           | Virtual Machines (VMs)                                                                                                                                     | Docker Containers                                                                                                                                    |
| :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Isolation**     | **Strong isolation**: Each VM has its own OS, providing complete isolation.                                                                                | **Process-level isolation**: Containers share the host OS kernel.                                                                                    |
| **Size/Overhead** | **Larger**: VMs have a larger footprint due to the guest OS and virtual hardware.                                                                          | **Lightweight**: Containers have minimal overhead, as they share the kernel.                                                                         |
| **Portability**   | **Less portable**: VMs can be tied to specific hypervisors and guest OS configurations.                                                                    | **Highly portable**: Containers are platform-agnostic and run consistently.                                                                          |
| **When to use**   | <ul><li>Need strong isolation between environments.</li><li>Dealing with legacy applications.</li><li>Replicating a complete system environment.</li></ul> | <ul><li>Building modern, cloud-native microservices.</li><li>Need to scale quickly and efficiently.</li><li>Portability is a top priority.</li></ul> |

## Docker components

### Docker Architecture Overview

![Containers vs Virtual Machines](static/image/img0005.png)

- The Three Main Components: A Docker system consists of three primary parts that interact via APIs.

#### The Docker Client

- **Definition:** The **Docker CLI** (Command Line Interface) used to issue commands.
- **Function:** It translates user commands into **REST API calls** sent to the Docker Host.
- **Flexibility:** The client can run locally or connect to a remote Docker Host (e.g., in the cloud).

#### B. The Docker Host

This is where the actual "action" takes place. It contains:

- **Docker Daemon:** The core engine that listens for API requests and manages images, containers, networks, and volumes.
- **Image Cache:** Local storage for downloaded images.
- **Containers:** The actual running (or stopped) instances of your applications.

[Image of Docker architecture showing Client, Host, and Registry interaction]

#### C. Image Registry (Container Registry)

- **Definition:** A stateless, highly scalable server-side application that stores and lets you distribute Docker images.
- **Docker Hub:** The default public registry.
- **Function:** Used to share images. Registries can be **public** or **private**.

---

### Common Workflows

#### Scenario 1: Running a Container (`docker run`) from an image

1. **Command:** You enter `docker run <image>` in the CLI.
2. **API Call:** CLI sends a request to the Docker Host's REST API.
3. **Daemon Check:** \* If the image is **NOT** in the local cache, the Daemon pulls it from the **Registry**.
   - If the image **IS** in the local cache, it skips the download.
4. **Execution:** The Daemon Host instantiates a new **Container** based o the image.

> **Pro Tip:** Think of an **Image** as a **Class** (the blueprint) and a **Container** as an **Instance/Object** (the running entity).

#### Scenario 2: Building and Pushing (`docker build` & `push`)

1. Issue `docker build` in the Docker CLI
2. Docker CLI sends a request to the Docker host's REST API
   - This also includes the respective **Dockerfile** and **Context**
3. The Daemon builds the image according to the **Dockerfile** and saves it in the **Image Cache**.
4. **Push:** Images remain local by default. Use `docker push` to upload the image to a Registry.
5. **Auth:** Pushing (or pulling from private repos) requires the Host to be **authenticated** (logged in).

---

### Key Concepts to Remember

| Term              | Description                                              |
| :---------------- | :------------------------------------------------------- |
| **Docker Daemon** | The background process managing all Docker activities.   |
| **Image**         | A read-only template used to create containers.          |
| **Container**     | A runnable instance of an image.                         |
| **Registry**      | A service that provides storage and delivery for images. |

### Summary Table: Local vs. Remote

- **Local Setup:** When using Docker Desktop, the **Client** and **Host** both run on your machine.
- **Remote Setup:** You can point your local **Client** to a **Host** running on a server or cloud provider.

# Mastering Containers

## Running the container

- We want to make sure that Docker is installed correctly on your system and ready to
  accept your commands. Open a new terminal window and type in the following command

```
docker version

# or docker --version
```

### Create and run a new container from an image

```
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

#short-command:
docker run
```

- A quick reference guide for the most commonly used `docker run` options and flags.

| Option      | Full Name       | Description                                              | Example                                             |
| :---------- | :-------------- | :------------------------------------------------------- | :-------------------------------------------------- |
| `-d`        | **Detached**    | Runs the container in the background (hidden).           | `docker run -d nginx`                               |
| `-p`        | **Publish**     | Maps a port from the Container to your Host (Laptop).    | `docker run -p 8080:80 nginx`                       |
| `--name`    | **Name**        | Assigns a custom name for easy management.               | `docker run --name my-web nginx`                    |
| `-v`        | **Volume**      | Mounts a folder from your host into the container.       | `docker run -v C:/html:/usr/share/nginx/html nginx` |
| `-e`        | **Env**         | Sets environment variables (passwords/API keys).         | `docker run -e MYSQL_ROOT_PASSWORD=123 mysql`       |
| `--rm`      | **Remove**      | Automatically deletes the container when it stops.       | `docker run --rm alpine echo "Hello"`               |
| `-it`       | **Interactive** | Connects your terminal to the container shell.           | `docker run -it ubuntu bash`                        |
| `--network` | **Network**     | Connects the container to a specific Docker network.     | `docker run --network my-net nginx`                 |
| `--restart` | **Restart**     | Defines if the container should restart on crash/reboot. | `docker run --restart always nginx`                 |
| `-m`        | **Memory**      | Limits the maximum RAM the container can use.            | `docker run -m 512m nginx`                          |

---

#### Example

- Running a Web Server

```bash
docker container run -d -p 8080:80 --name my-nginx nginx:stable-alpine
```

Meaning: Run Nginx in the background (-d), connect your browser at localhost:8080 to the container's port 80 (-p), and name it my-nginx

- Running a Temporary Task

```bash
docker container run --rm alpine ls -l
```

Meaning: Start a tiny Alpine container, list the files (ls -l), and immediately delete the container (--rm) so it doesn't waste disk space on your laptop.

### List containers

```
docker container ls [OPTIONS]

#short-command
docker ps
docker container ps
docker container list
```

- Docker Container LS Options

The `docker container ls` command is used to list the containers on your system. By default, it only shows running containers, but these options allow you to see much more.

| Option       | Full Name         | Description                                                     | Example                                                  |
| :----------- | :---------------- | :-------------------------------------------------------------- | :------------------------------------------------------- |
| `-a`         | **All**           | Shows all containers (including those that are stopped/exited). | `docker container ls -a`                                 |
| `-q`         | **Quiet**         | Only displays the Container IDs (useful for scripts).           | `docker container ls -q`                                 |
| `-l`         | **Latest**        | Shows the last container created (even if it's not running).    | `docker container ls -l`                                 |
| `-n [X]`     | **Last X**        | Shows the last [X] containers created.                          | `docker container ls -n 3`                               |
| `-s`         | **Size**          | Displays the total file size of each container.                 | `docker container ls -s`                                 |
| `--filter`   | **Filter**        | Filters the list based on conditions (status, name, etc.).      | `docker container ls --filter "status=exited"`           |
| `--format`   | **Format**        | Pretty-prints the output using a Go template.                   | `docker container ls --format "{{.Names}}: {{.Status}}"` |
| `--no-trunc` | **No Truncation** | Shows the full Container ID and command without shortening.     | `docker container ls --no-trunc`                         |

#### Example

- Find Every Container on Your System
  By default, `ls` hides stopped containers. Use `-a` to see everything that is taking up space.

```bash
docker container ls -a
```

### Stop one or more running containers

```bash
docker container stop [OPTIONS] CONTAINER [CONTAINER...]

# short-command
docker stop
```

## Container Lifecycle

![Container Lifecycle](static/image/img0006.png)

- The Creation Phase (Run vs. Create)
  - `docker run`: A high-level command that combines two steps: docker create (preparing the container with an image) and docker start (executing it).
  - Images & Tags: If no tag is specified (e.g., nginx), Docker defaults to the `:latest tag`.
  - Independence: You can manually create a container without starting it, or start one that has already been created.

- The Running Phase
  - Once a container is in the Running State, you can interact with it using:

  - `docker logs`: To view the output.

  - `docker inspect`: To see detailed configuration.

  - `docker exec`: To run commands inside the active container.

- Pausing vs. Stopping
  - There are different ways to halt a running container:

  - `docker pause`: Suspends the container but keeps the memory contents intact. You can return to the running state via docker unpause.

  - `docker stop`: Shuts down the container gracefully, clearing the memory.

  - `docker kill`: Forcefully stops the container (SIGKILL). This is faster but risks data loss as the process cannot "wrap up" properly.

- Exit Codes & Restart Policies
  - When a container's main process (PID 1) finishes, it exits with a code:

  - **Exit Code 0**: Success/Completed task.

  - **Non-zero Exit Code**: An error occurred.

  - **Restart Policies**: Docker can be configured to automatically restart containers if they exit with an error (though this is covered later in the course).

- The Stopped & Removed Phases
  - **Stopped State**: The container is no longer active but still exists on the disk. You won't see it with docker ps unless you use the -a (all) flag. You can still view its logs or use docker start to run it again.

  - `docker rm`: This is the final step. It completely deletes the container and its contents, freeing up system resources. Once removed, you can no longer inspect it or view its logs.

## Docker Cleanup Commands Reference

| Command                         | Description                                                                    | Risk Level           |
| :------------------------------ | :----------------------------------------------------------------------------- | :------------------- |
| `docker system df`              | **Display** a summary of Docker disk usage.                                    | **Safe**             |
| `docker system prune`           | Removes all stopped containers, unused networks, and **dangling** images.      | **Low**              |
| `docker system prune -a`        | Removes **all** unused images (not just dangling ones) and stopped containers. | **Medium**           |
| `docker system prune --volumes` | Same as `system prune` but also deletes **all unused volumes**.                | **High (Data Loss)** |
| `docker image prune`            | Removes only dangling images (those tagged as `<none>`).                       | **Low**              |
| `docker image prune -a`         | Removes all images not currently used by a container.                          | **Medium**           |
| `docker container prune`        | Removes all stopped containers.                                                | **Low**              |
| `docker volume prune`           | Removes all local volumes not used by at least one container.                  | **High (Data Loss)** |
| `docker rm -f $(docker ps -aq)` | **Forcefully** stops and removes ALL containers.                               | **High**             |

### 💡 Best Practices for Windows Developers

- The "Auto-Cleanup" Flag: When testing an image, always use the `--rm` flag. Docker will delete the container immediately after you stop it.

```bash
docker run --rm nginx
```
