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
