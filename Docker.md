- [Introduction to Containers](#introduction-to-containers)
  - [What are Container](#what-are-container)

# Introduction to Containers

## What are Container

- In the old days
  - Developers would develop new applications. Once an application was completed in their eyes, they would hand that application over to the operations engineers, who were then supposed to install it on the production servers and get it running. If the operations engineers were lucky, they even got a somewhat accurate document with installation instructions from the developers
  - Usually, each application has some external dependencies, such as which framework it was built on, what libraries it uses, and so on. Sometimes, two applications use the same framework but of different versions that might or might not be compatible with each other
- But these days
  - One of the first approaches was to use **virtual machines (VMs)**.
    - Instead of running multiple applications all on the same server, companies would package and run a single application on each VM
    -
