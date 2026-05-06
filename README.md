# Docker_zero-to-Hero

# What is Docker

Docker is a containerization platform used to package applications along with their dependencies into lightweight units called containers

Containers allow applications to run consistently across different environments such as development testing and production

Docker uses operating system level virtualization which means containers share the host OS kernel but run in isolated environments

## Why Docker is used

Ensures consistency across environments
Faster deployment and startup time
Efficient resource utilization
Easy scaling of applications
Supports microservices architecture

## Core Components of Docker

Docker Client
Command line tool used by users to interact with Docker

Docker Daemon
Background service that manages containers images networks and volumes

Docker Engine
Complete platform that includes client daemon and APIs

containerd
Manages container lifecycle

runc
Low level runtime that creates and runs containers

Docker Registry
Stores and distributes container images

## Docker Container Execution Flow

When a docker run command is executed the container creation follows a structured flow from client to kernel

Step 1 Docker Client
User runs a command like docker run nginx
Docker CLI sends request to Docker Daemon

Step 2 Docker Daemon
Receives the request
Checks if the image exists locally
If not available pulls the image from Docker Hub or another registry

Step 3 Image Preparation
Image is downloaded and stored locally
Container configuration is prepared

Step 4 containerd
Docker Daemon sends request to containerd
containerd manages container lifecycle

Step 5 runc
containerd uses runc to create the container
runc sets up isolation using namespaces and resource control using cgroups

Step 6 Linux Kernel
Kernel runs the container as a process
Provides isolation and resource management

Step 7 Container Running
Application inside container starts running
Container is now active

## Key Points

Docker containers are lightweight and fast
Containers share the host operating system kernel
Each container runs as an isolated process
Docker follows a layered architecture
containerd and runc follow open container standards

# Containers vs Virtual Machines

Both containers and virtual machines are used to run applications in isolated environments, but they differ in architecture, performance, and use cases.

---

## What are Containers

Containers package an application along with its dependencies and run as isolated processes on the host system. They share the host operating system kernel, making them lightweight and fast.

Example technology
Docker

---

## What are Virtual Machines

Virtual machines emulate a complete system with their own operating system. They run on a hypervisor which manages multiple virtual machines on a single physical machine.

---

## Difference Table

| Feature             | Containers              | Virtual Machines              |
| ------------------- | ----------------------- | ----------------------------- |
| Virtualization Type | OS level virtualization | Hardware level virtualization |
| Operating System    | Share host OS kernel    | Each VM has its own OS        |
| Startup Time        | Seconds                 | Minutes                       |
| Performance         | High (lightweight)      | Lower (heavier)               |
| Resource Usage      | Low                     | High                          |
| Size                | Small (MBs)             | Large (GBs)                   |
| Isolation           | Process level isolation | Full OS level isolation       |
| Portability         | High                    | Moderate                      |
| Management          | Easier and faster       | More complex                  |
| Boot Process        | No OS boot required     | Full OS boot required         |

---

## When to Use Containers

Microservices architecture
CI CD pipelines
Cloud native applications
Fast scaling systems

---

## When to Use Virtual Machines

Running different operating systems
Strong isolation and security requirements
Legacy systems
Full environment simulation

---

## Key Points

Containers are lightweight and fast because they share the host OS kernel
Virtual machines are heavier because they include a full operating system
Containers are ideal for modern DevOps workflows
Virtual machines are useful when complete isolation is required

# Docker Images Foundation

---

## What is a Docker Image

A Docker image is a read only template used to create containers
It contains application code runtime libraries dependencies and configuration required to run an application

An image does not run by itself
It becomes a running instance called a container

---

## Key Characteristics

Read only
Layered structure
Reusable
Portable across environments

---

## How Images are Created

Images are built using a Dockerfile

A Dockerfile is a set of instructions that defines how the image should be built

---

## Example Dockerfile

```
FROM nginx
COPY . /usr/share/nginx/html
RUN apt-get update
CMD ["nginx", "-g", "daemon off;"]
```

---

## Image Build Flow

Step 1 Write Dockerfile
Define base image and application setup

Step 2 Build Image
Run docker build command to create image

Step 3 Store Image
Image is stored locally on the system

Step 4 Push Image
Image can be pushed to a registry like Docker Hub

Step 5 Pull Image
Other systems can pull the image from registry

Step 6 Run Container
Image is used to create and run a container

---

## Example Flow

```
Dockerfile → docker build → Image → docker push → Registry → docker pull → docker run → Container
```

---

## Image Layers Concept

Each instruction in Dockerfile creates a layer

Layers are cached and reused which improves performance

Example

Base image layer
Application dependencies layer
Application code layer

---

## Important Commands

docker build builds an image
docker images lists images
docker pull downloads image
docker push uploads image
docker run creates container from image

---

## Key Points

Images are the foundation of containers
Containers are created from images
Images are immutable meaning they do not change once created
Layering improves efficiency and reuse

---

## Final Understanding

Docker image is a packaged blueprint of an application
It ensures consistency across environments
It is the starting point for running containers
