# Overview of :whale2:Docker Features

Short descrition of features with quick links to deep dives.

For quick commands check cheat sheet here :point_right: [Cheat Sheet](Cheatsheet.md) 

## Docker Desktop

Docker Desktop is the company name that also created the technology.

Docker Desktop is the software from Docker that contains 
all the services needed to develop containers.

:point_right: :link:[Docker Desktop Installer](https://docs.docker.com/desktop/windows/install/)

## Basic Topics

### Images

A Docker image is like a zip file that contains an operating system and a single executable ready to be run.

:point_right: :link: [Docker Images](Images.md)

### Containers

A Docker container is a running version of an image.

Creating a container is similar to a process. Multiple processes can be started from a single executable file and multiple containers can be created from a single Docker image.

:point_right: :link: [Docker containers](Containers.md)

### Volumes

A Volume is like a USB Stick. In this case Volumes can be mounted in Docker containers in case the processes living in the containers need to interact with the files in the volumes. 

:point_right: :link: [Docker volumes](Volumes.md)

## Advanced Topics

### DockerFile

A **Dockerfile** is a configuration file used to create **Docker** images.

:point_right: :link: [Dockerfile](Development/Dockerfile_reference.md)

### Docker Build

The Docker `CLI` allows us to build `images` using `Dockerfiles`.

:point_right: :link: [Building Docker Images](Development/Building_Images.md)

### Image Registry

The Registry is used to store images for later distribution.

:point_right: :link: [Docker Registry](Development/Image_Registry.md)

### Networking

With Docker, it is possible to control the networking capabilities of a container 
such as creating virtual networks and assigning the container an IP on that network or 
attaching the container to a real physical network.

:point_right: :link: [Docker Networking](Networking.md)

:point_right: :link: [Docker Networking Bridge Exercises](Networking/Bridge_Exercises.md)

:point_right: :link: [Docker Networking MACVLAN Exercises](Networking/MACVLAN_Exercises.md)

:point_right: :link: [Docker Networking IPVLAN Exercises](Networking/IPVLAN_Exercises.md)

:point_right: :link: [Docker Networking IPV6 Exercises](Networking/IPV6_NETWORKING.md)

### Docker Stacks (Compose)

Docker Stacks, also known as Docker Compose, allows us to deploy multiple containers or other Docker
objects (Volumes/Networks/Configurations) using a single file to define all of them. 

:point_right: :link: [Docker Stacks](Stacks/compose.md)

:point_right: :link: [Docker Stacks Services](Stacks/service_container.md)

:point_right: :link: [Docker Stacks Volumes](Stacks/stack_volume.md)

:point_right: :link: [Docker Stacks Networking](Stacks/stack_network.md)

:point_right: :link: [Docker Stacks Exercises](Stacks/stacks_exercises.md)

### DevContainers

### Dev Environments

### Docker Extensions

## Niche Topics

### Docker Containers on Azure

### WSL + Docker

### Docker Development in Visual Studio

### Docker Compose in Visual Studio

### Docker Services in Visual Studio