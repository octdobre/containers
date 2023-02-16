# :ledger: Dockerfile 

## Introduction

A `Dockerfile` is a file that describes how a **Docker** `Image` will be `built`.

A `Dockerfile` does not create **Docker** `Containers` !!!

A **Docker** `Image` is like a template from which one can create multiple `containers`.


## Basic example

Here is an example of a basic `Dockerfile` used to create an `image` containing a **.Net** `Web API`. 

```
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
EXPOSE 80
COPY . /app
RUN apt-get update
ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
```
 
In the example above we can see a few things:
-  `FROM mcr.microsoft.com/dotnet/aspnet:7.0` defines the `base image`.
-  `WORKDIR /app` is setting the `working directory` that will be inside the `container`.
-  `EXPOSE 80` is defining that **Docker** should expose `port` `80` when creating the `container`.
-  `COPY . /app` is defining that the `application` contents will be copied over to the `/app` `folder` inside the `image`.
-  `RUN` is updating the `base image` `libraries` and adding the result as a new `layer` on the `image`.
-  `ENTRYPOINT ["dotnet", "WeatherAPI.dll"]` defines the `command` that will `run` when the `container` will be created.

## Dockerfile Directives

A `Dockerfile Directive` is a `command` inside a `Dockerfile`. 

`Dockerfile Directives` dictate how a **Docker** `image` is `built`. 

Each `directive` adds a new `layer` to the `image`.

### FROM Directive 

It is used to define the `base image` for the created `image`. 

```
FROM mcr.microsoft.com/dotnet/aspnet:7.0
```
`mcr.microsoft.com/dotnet/aspnet:7.0` is a `base image` found on **DockerHub**.

All `base images` are found on **DockerHub**. 

**DockerHub** is an `online platform` for and a massive `image registry` where one can `download` and `upload` `images`.

### ARG Directive

It is used to pass `arguments` to the **Dockerfile**. Can only be used before `FROM` `directive`.

```
ARG IMAGE_VERSION=7.0
FROM mcr.microsoft.com/dotnet/aspnet:{IMAGE_VERSION}
```

### ENV/LABEL Directives

The `ENV` `directive` is used to set `environment` `variables` inside the future `containers`.
These `environment` `variables` can be overridden when the `container` is created. 
```
ENV ASPNETCORE_ENVIRONMENT="development"
```

The `LABEL` `directive` is used to add `metadata` to an `image`.
```
LABEL buildNumber="1.1.1"
```

### WORKDIR Directive

It is used to set the `working directory`. It has the same behavior as the `command line`.

All `commands` will execute in the set `working directory`.

The default `working directory` is `/`.

### COPY/ADD Directives

Both `directives` are used to copy `files`, and `directories` to the `filesystem` of the `image`.
The `ADD` `directive` is more powerful because it can also `download` `files` from a `URL`,
and `unzip` `compressed` `files`.

```
COPY . .   //copy from the host current directory (.) to the image's current working directory (.)
```
### USER Directive

The `USER` `directive` is used to set the `user` and `group`.
If affects all future `directives`.

```
RUN groupadd -r mygroup && useradd -r -g mygroup myuser
USER myuser:mygroup
```

In this example, we're creating a new `user` called `myuser` and a new group called `mygroup` using the `RUN` `directive`. The `-r` `flag` creates a `system` `user`, which means that it won't have a `home directory` or a `shell`. The -`g` `flag` sets the `user's` primary `group` to the `mygroup` group.

We then set the `user` and `group` to `myuser:mygroup` using the `USER` `directive`. This means that any `commands` that are `executed` after this point in the `Dockerfile` will be run as the `myuser` `user` in the `mygroup` `group`.

### RUN Directive

It is used to run `commands` inside the `image`. It uses the `WORKDIR` as the `working directory`.

It can be a bit confusing to think that you can `run` a `command` inside an `image`.

The idea of the `RUN` `directive` is to modify something on the `base image`and actually add it as a new `layer`.

```
RUN apt-get && apt-upgrade
```
This `command` updates the `software` that was on the `base imag`e and adds it as a new `commit` to the `image`.
It is considered a new `image layer`. 
Future `RUN` `directives` will be aware of the changes that the previous `directives` have done to the `image`.

### ENTRYPOINT/CMD Directives

In a **Dockerfile**, `ENTRYPOINT` and `CMD` are `directives` that define the behavior of the `container` when it starts.

The `ENTRYPOINT` specifies the `command` that will be executed when the `container` is started, and it cannot be overridden by any `command-line arguments` that are passed to the `docker run` `command`. It is used to specify the main `executable` of the `container`.

The `CMD` `directive` is used to specify the `arguments` that will be passed to the `ENTRYPOINT` `command`. If there is no `ENTRYPOINT` specified, `CMD` can be used to specify the main `executable` of the `container`.

```
ENTRYPOINT ["dotnet"]
CMD ["WeatherAPI.dll"]
```
The `command` that will be `executed` is `dotnet` and the `parameter` is `WeatherAPI.dll`.

This can also be written simply as:
```
ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
```
## Image Theory

### Base images

**Docker** `base images` are pre-built `images` that contain the basic `operating system` `environment` and `libraries` required to `run` `applications`. 

These `images` serve as a starting point for creating custom `images` that contain specific `applications` or `dependencies`. 

Base `images` are commonly stored on a **Docker Registry**, such as **Docker Hub**, and can be pulled down and used as the starting point for a new `image` `build`.

### Base Operating System

A `base operating system` for a **Docker** `image` is the foundation upon which the `image` is built. 

It provides the `file system structure`, `libraries`, `tools`, and `utilities` that the `image` requires to `run` the desired `application` or `service`. Examples of `base operating systems` include **Alpine**, **Ubuntu**, and **Centos**. 

The choice of the `base operating system` depends on the specific requirements of the application or service being `containerized`.

### :books: Layering system

**Docker** `images` are composed of multiple `layers`, each `layer` representing a change to the `file system` from the previous `layer`. These `layers` are `stacked` on top of each other to form a single `image`. Each `layer` is a distinct `read-only` `filesystem`, and when an `image` is `run`, a new `read-write` `layer` is added on top to store any changes made to the `image`.

The `layering` system allows for `images` to be created and shared more efficiently, as multiple `images` can share common `layers`. For example, two images that both have the same `base operating system` `layer` can share that `layer` instead of having two separate copies. This reduces the `disk space` and `bandwidth` required to transfer `images`.

Additionally, the `layering` system allows for `images` to be created incrementally, with each `layer` representing a step in the `build` process. This allows for easy modification of `images`, as only the `layers` that have changed need to be `rebuilt` and `pushed` to a `registry`.

Overall, the `docker image layering` system provides a flexible and efficient way to `build`, `store`, and `share` `images` for use in **Docker** `containers`.

## Multi-Stage-Build 

A `multi-stage` **Dockerfile** allows you to `build` an `image` in multiple stages, each stage using the result of the previous stage as a `base image`.

Each stage is defined by a `FROM` `directive`, and the name of the stage is specified as an alias. The final stage will be the default stage for the resulting `image`, and only its contents will be included.

Here is an example that builds a **.Net** `app`, `runs `unit tests`,
and then copies the `build artifacts` to a **Debian** `image` for `deployment`:
```
# Build stage
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
COPY . .
WORKDIR /src/Weather
RUN dotnet publish --no-restore -c Release -o /app

FROM build as unit test
WORKDIR /src/Weather.UnitTests

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Weather.dll"]
```

In this example we are defining the `aspnet:7.0` `image` as "base", then we are defining `dotnet/sdk:7.0`
as a first stage to building the `app`. We then copy the contents and `build` the `application` in this stage.
Then we run a `unit test` stage based on the build stage and then finally we copy the `build` output to the final
`image`. The other stages get discarded and only a slim `image` with the necessary `files` to `run` the `app` is left in the final `image`.

## :books: Documentation

:point_right::link:[Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)

:point_right::link:[Best Practices for writing a dockerfile](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)