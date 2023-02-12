# :ledger: Dockerfile 

## Introduction

A `Dockerfile` is a file that is used to define how a **Docker** `Image` will be created.

A `Dockerfile` does not create **Docker** `Containers` !!!

A **Docker** `Image` is like a template from which one can create multiple containers.


## Basic example

Here is an example of a basic `Dockerfile` used to create a image containing
a .Net Web API. 

```
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
EXPOSE 80
COPY . /app

ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
```
 
In the example above we can see a few things:
-  `FROM mcr.microsoft.com/dotnet/aspnet:7.0` defines the base image.
-  `WORKDIR /app` is setting the working directory that will be inside the container.
-  `EXPOSE 80` is defining that Docker should expose the port 80 when creating the container.
-  `COPY . /app` is defining that the application contents will be copied over to the `/app` folder inside the image.
-  `ENTRYPOINT ["dotnet", "WeatherAPI.dll"]` defines the command that will run when the container will be created.

## Base images

Docker base images are pre-built images that contain the basic operating system environment and libraries required to run applications. 

These images serve as a starting point for creating custom images that contain specific applications or dependencies. 

Base images are commonly stored on a Docker registry, such as Docker Hub, and can be pulled down and used as the starting point for a new image build.

## Base Operating System

A base operating system for a Docker image is the foundation upon which the image is built. 

It provides the file system structure, libraries, tools, and utilities that the image requires to run the desired application or service. Examples of base operating systems include Alpine, Ubuntu, and Centos. 

The choice of base operating system depends on the specific requirements of the application or service being containerized.

## Layering system

Docker images are composed of multiple layers, each layer representing a change to the file system from the previous layer. These layers are stacked on top of each other to form a single image. Each layer is a distinct read-only filesystem, and when an image is run, a new read-write layer is added on top to store any changes made to the image.

The layering system allows for images to be created and shared more efficiently, as multiple images can share common layers. For example, two images that both have the same base operating system layer can share that layer instead of having two separate copies. This reduces the disk space and bandwidth required to transfer images.

Additionally, the layering system allows for images to be created incrementally, with each layer representing a step in the build process. This allows for easy modification of images, as only the layers that have changes need to be rebuilt and pushed to a registry.

Overall, the docker image layering system provides a flexible and efficient way to build, store, and share images for use in Docker containers.

## Multi-Stage-Build

A multi-stage Dockerfile allows you to build an image in multiple stages, each stage using the result of the previous stage as a base image.

Each stage is defined by a FROM instruction, and the name of the stage is specified as an alias. The final stage will be the default stage for the resulting image, and only its contents will be included.

Here is an example that builds a .Net app, runs unit tests,
and then copies the build artifacts to a Debian image for deployment:
```
# Build stage
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
COPY . .
WORKDIR /src/Weather
RUN dotnet publish --no-restore -c Release -o /app

FROM build as unittest
WORKDIR /src/Weather.UnitTests

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Weather.dll"]
```

In this example we are defining the aspnet:7.0 image as base, then we are defining dotnet/sdk:7.0 
as a first stage to build the app. We then copy the contents and build the application in this stage.
Then we run a unit test stage based on the build stage and then finally we copy the build output to the final
image. The other stages get discarded and only a slim image with the necessary files to run the app are left in the final image.

## :books: Documentation

:point_right::link:[Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)