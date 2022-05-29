# :cd: Docker Images

## :older_man: Oldschool System Images

A **Docker**:whale2: `image`:cd: is similar to a `system image`:cd:. 

A system image is a copy of the `operating system`:penguin: and its `configuration`, bundled into a `file`:file_folder:, that can later be overwritten to a `disk`.

 In doing so, one does not need to `reinstall` and `reconfigure` the operating system. A `Virtual Machine`:computer: can also be started from a system image.

## :arrow_forward: Enter Docker

A **Docker** image:cd: is a stipped version of a :point_right:system image:cd:. 

The removed components are unnecessary `executables` and `DLLs` and other `files` thus making the image a smaller `filesize`.

The term `'image'` is independent of the company named **Docker**:whale2:. 

These `images` can be created with **Docker** `tools`:hammer: or any other types of tools that can create :point_right:**Container Runtime Interface** compliant images. 

These `images` can be `deployed` on both *Docker* Engine and **Kubernetes**.

## :birthday: Docker Cakes

`Images` are layered:books: and therefore new versions of an image can be created by adding `files` to an existing `image`.

Images like `mcr.microsoft.com/dotnet/aspnet:6.0` contain only the `runtime`:runner: and `libraries`:books: needed to run an **ASP.Net** `application`. 

These are called `base` images.

These `images`, although they can be used to create `containers`:arrows_counterclockwise:, are not used on their own.

To make the `container` useful we add a new `layer`, which contains our `application` and we create a new `image` on top of the `base layer`.

An `image` that contains both the **ASP.Net** `runtime` and our **ASP.Net** `WebApplication`.
This type of image is the intended product for which **Docker** was created. :balloon::smile:

----
There are many `base images` that can be used to support an `application`. 

:point_right:  **Dotnet** runtimes, **NodeJs** runtimes, **Python** runtimes, etc.

There are also `services` as **Docker** `images` that can be used immediately.

:point_right: **Sql Server** 2019, **Redis**, **ElasticSearch**, etc.


## :blue_book: Commands

### :point_left: Pulling an Image :cd:

**Docker Desktop** installs a `CLI` tool called **DockerCLI** that 
it is used for most operations with **Docker**:whale2:.

`Images` live in `registries`:scroll:. **Docker Desktop** installs a `local registry`.

To view `images` in the `local registry`:
```
docker images
```
To add `images` to our `local registry`:
```
docker pull redis
```
Command output :mag_right::
```
Using default tag: latest
latest: Pulling from library/redis
42c077c10790: Pull complete
a300d83d65f9: Pull complete
ebdc3afaab5c: Pull complete
31eec7f8651c: Pull complete
Digest: sha256:1b90dbfe6943c72a7469c134cad3f02eb810f016049a0e19ad78be07040cdb0c
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```
This `image` will be downloaded from `hub.docker.com`. This is where most public `images` live. 

### :x: Removing an Image :cd:
To remove an `image` from the `local registry`.
```
docker rmi redis
```
`Images` can have the same name but a different `tag`. 

```
docker images

REPOSITORY  TAG     IMAGE ID
redis       2       53aa81e8adfa 
redis       latest  53aa81e8adfa
```

### :pencil2: Tagging
`Images` can be `tagged`:pencil2::
```
docker tag redis redis:3 
```

We can see the result running `docker images`:
```
docker images

REPOSITORY  TAG     IMAGE ID
redis       2       53aa81e8adfa
redis       3       53aa81e8adfa    
redis       latest  53aa81e8adfa
```
Notice that we have now 3 `images` with the same name but different `tags`. 
You can also notice that the `Image Id` is the same because the `image` is the same.

We can remove a `image` based on its `Image id`.
```
docker rmi 53aa81e8adfa
```
Now all 3 `images` should have been removed.

### :mag_right: Inspect Details 
To view details of an `image`:
```
docker inspect redis
```
The output:mag_right: will be a `json` with details about the `image`.

```

    {
        "Id": "sha256:53aa81e8adfa939348cd4c846c0ab682b16dc7641714e36bfc57b764f0b947dc",
        "RepoTags": [
            "2:latest",
            "redis:2",
            "redis:latest"
        ],...
```

### :hand: Pushing an Image :cd: to a Registry :scroll:

`Images` and also be pushed to `registries` where we have `permission` to `push`. 
An example `registry` could be an **ACR(Azure Container Registry)** that we have created 
for ourselves.

 To push an `image` to a `registry`, we need to first `retag` it and then `push` it.

```
docker image tag myService:1 myPrivateRegistry/myService:1
```
```
docker image push myPrivateRegistry/myService:1
```
Notice that we have prefixed the `image` name with the name of the `registry`. 
Whenever an `image` contains a `/` in the name, **Docker** will interpret the `string` before the `/` as the name of a `registry`. 

### :floppy_disk: Save the state of a running container

An `image` can be created from a running `container`.
```
docker container commit c16378f943fe myImage:1
```
**c16378f943fe** in this case is the `container ID` that is currently running.

### Saving an Image on disk locally

An `image` can be saved to a `file` and loaded to the `local registry`:

```
docker save <imagename_here> > imagename.tar
```
```
docker load < imagename.tar
```

### :hocho: Pruning
Or cleanup! Pruning removes all `images` that are not being used currently for a running `container`.

Warning!:warning: This will remove any unused `images`:heavy_exclamation_mark::heavy_exclamation_mark::warning:
```
docker image prune -a
```
```
docker image prune -a --filter "until=24h"
```

:heavy_exclamation_mark:`Images` that are currently used for a running `containers` cannot be removed:heavy_exclamation_mark: