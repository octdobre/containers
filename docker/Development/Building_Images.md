# Building Images

Prerequisite to this article :point_right::link:[Dockerfile Reference](https://docs.docker.com/engine/reference/builder/).

## Introduction

**Docker** `build` is a command that allows you to create a Docker image from a [Dockerfile ](https://docs.docker.com/engine/reference/builder/). The [Dockerfile ](https://docs.docker.com/engine/reference/builder/) is a simple text file that contains instructions on how to build a **Docker** image.

Example [Dockerfile ](https://docs.docker.com/engine/reference/builder/):
```
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
EXPOSE 80
COPY . /app
RUN apt-get update
ENTRYPOINT ["dotnet", "WeatherAPI.dll"]
```

The `WeatherAPI` `dll` and the [Dockerfile ](https://docs.docker.com/engine/reference/builder/) should be in the same `folder`.

`Run` the following `command`, from the same `folder`, to `build` the `image`:
```
docker build -t weather-api:1 .
```
In the above example we can see the following things:
- The command to build is `docker build`.
- `-t` `flag` is used to name the `image` according to the format `<image_name>:<image_tag_or_version>`.
- `.` (dot) is used to specify what is the [build context](#build-context).

## Flags

```
docker build \
-t weather-api:1 \
-f "dockerfile.dev" \
--build-arg sqlconn="REDACTED" \
--build-arg ASPNETCORE_ENVIRONMENT \
--add-host 10.21.41.12 \
--label build-date="2023-02-17" \
--force-rm /
.
```
In the above example we can see the following things:
- `-f` is used to specify a **Dockerfile** other than the default.
- `--build-arg` is used to pass `arguments` to the `ARG` `directive` in the `image`. 

    `Build arguments` should always be written using uppercase letters.

    The `--build-arg` without a value passed in the `command`, will take the value from the `host environment` if exists.

- `--add-host` is used to add additional `host entries`.
- `--label` is used to add `metadata` to the `image`.
- `--force-rm` is used to force the removal of `intermediate layers` even if the `build` fails. 

    It is helpfull for not gathering unused `layers` in the **Docker Engine** local `cache`.

## Other build arguments

### Custom build outputs `--output`

The `--output` argument can be used to export the `build` `artifacts` back to the `host`.

```
docker build -o artifacts.
```

This `command` will output the `build artifacts` to the `artifacts` `folder` on the `host`.

If the `folder` does not exist, it will be created.

### Set target build stage `--target` in multi-stage Dockerfiles

The `--target` `argument` is used to `build` only a specific `stage` in a `multi-stage` **Dockerfile**.

```
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
# ...

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS publish
# ...
```

```
docker build --target build  -o build-artifacts.
```
This `command` will only run the `build` stage and copy the outputs to the `build-artifacts` `folder` on the `host`.

## Build context 

The **Docker** `build context` is the set of `files` and `directories` that are available to the `docker build` `command` when building a `Docker image`. It includes the **Dockerfile** and any other `files` that are needed to `build` the `image`. The `build context` is specified as the `directory` that contains the **Dockerfile**.

During the `build` process, **Docker** sends the entire `build context` to the **Docker daemon**, which then uses it to build the `Docker image`. The `build context` is sent over to the `daemon`, which includes all `files` in the `directory`, so it is important to make sure that only the necessary `files` are included, as sending unnecessary `files` can increase `build` time and `network` traffic.

For example, if you have a **Dockerfile** that specifies that `files` from the `app directory` should be copied into the `image`, the `build context` should be the `parent directory` of the `app`. By default, the `docker build command` uses the current `directory` as the `build context`, but it can be changed using the `-f` `flag` to specify a different **Dockerfile**, and the `.` (dot) to specify a different `build context`.


## :books: Documentation

:point_right::link:[Docker Build](https://docs.docker.com/engine/reference/commandline/build/)