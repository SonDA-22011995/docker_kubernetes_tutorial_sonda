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
