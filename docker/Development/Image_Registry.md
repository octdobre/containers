# Image Registry

## Introduction

A **Docker registry** is a place where **Docker images** are stored and distributed. It serves as a `repository` for **Docker images**, making them available to other `users` or `systems`. 

The most commonly used **Docker registry** is **Docker Hub**, which is a public `registry` that allows developers to share their **Docker images** with others. 

However, **Docker** also supports the use of private `registries`, which are hosted within an organization's `network` and provide a secure way to manage and distribute **Docker images** internally.

The `installation` of **Docker Engine** on either **Windows** or **Linux** comes with a `private local image registry`. 

## Pulling Images

The following `commands` can be used to `pull` an `image` from the **Docker hub** to the local **Docker Engine** `installation`.

```
docker pull  <image name>
docker pull  <image name>:<imagetag>
docker pull  <image name>@<digest>
```

When creating a `container`, and the `image` is not present locally, by default **Docker** first 
`downloads` the `image` from **Docker Hub** and then creates the `container`.

If the `image` is already present in the `local registry` then **Docker** will not try to pull the `image` again.

## Pushing Images to a Registry

By default, `images` created with the **Docker CLI**, are stored in the `local registry`.

To add them to another `registry` the `docker image push` command is used.

To `push` an `image` the following steps must be followed:

1. `Tag` the `image` by prefixing the `URL` of the `registry`:
    ```
    docker image tag weatherapi:latest  https://my-external-registry.com/weatherapi:latest
    ```
2. Now the `image` is ready to be `pushed` by running the `command`:
    ```
    docker image push https://my-external-registry.com/weatherapi:latest
    ```

## Adding multiple tags to a image and pushing to a registry

An `image` can have multiple `tags`.

```
docker image tag weatherapi:1 docker image tag weatherapi:latest
docker image tag weatherapi:1 docker image tag weatherapi:beta
docker image tag weatherapi:1 docker image tag weatherapi:dev
```

To `push` all `tag` versions to a `registry` run the `command` with the `--all-tags` `argument`:
```
docker image push --all-tags https://my-external-registry.com/weatherapi
```

## :books: Documentation

:point_right::link:[Docker Hub](https://hub.docker.com/)
