#   :closed_book: Docker Volumes 

## :cloud_with_rain: Ephemereal Containers

**Docker**:whale2: `containers` are ephemeral. Once a `container` is removed, the local `data` is stored will be removed along with the `container`. 

When you want to update a `service` running as a `container`, you update the `image` of the `container`.
The process of updating the `image` of a `container` is to remove the old `container` and create a new one.
By doing this, everything that was stored locally in the `container` is automatically deleted. 

What if we want to preserve `data` in between `container` updates?

This is what `volumes` are for.

## :floppy_disk: Long Term Storage

`Volumes` are not ephemeral. They are not dependent on a `container`. 

`Containers` use `volumes` to store data. If a `container` is removed, the `volume` is not removed, 
and can be used by another container. 

A `service` running inside a `container` can store `state` in a `volume`, update itself, and then read the `state` from the same `volume`.

`Volumes` are managed by **Docker**. They are stored on the host `file system`. Usually in `/var/lib/docker/volumes/` on `linux`.

##  :abacus: Storage Types

**Docker** offers multiple types of persistable storage. Two of them are:
* `Volumes`
* `Bind Mounts`

`Volumes` persist data on the `host file system` but are managed by **Docker**. 

You can create, manage and delete `volumes`with **Docker** commands.

`Bind mounts` are just references to `folders` on the `host file system`. 

While `volumes` can only exist in the `/var/lib/docker/volumes/` location, `bind mounts` can reference any `folder` on the `host`.

##  :wrench: Usage

Storage is `mounted` inside the `container` as a `folder`.

Using the `-v` flag we can `mount` both `volumes` and `bind mounts`.

Synthax `-v <volume/bindmount>:<paht inside container>`.

* Example **VOLUMES**:
```
docker run -d -v myvol2:/app  nginx
```
Here we `mount` the `volume` `myvol2` to the path `/app` inside the `container`.

* Example **BIND MOUNTS**

```
docker run -d -v /containers/nginx_data:/app  nginx
```
Here we `mount` the `bind mount` path `/containers/nginx_data` to the path `/app` inside the `container`.

* Options

Options can also be specified after the `container` path like so: `-v myvolume:/app:<options here>`.
In most cases, you won't need to use options.

The `flag` `--mount` can be used for more complex scenarios. Check the [official docs](https://docs.docker.com/storage/volumes/) for more details.

##  :hammer_and_wrench: Docker Commands

**Docker** commands only apply to **Docker** `volumes` since they are the only ones that are managed. `Bind mounts` cannot be managed by **Docker** using the `CLI`.

Creating a `volume`:
```
docker volume create my-vol
```

Listing `volumes`:
```
docker volume ls
```

Inspect details about a `volume`:
```
docker volume inspect my-vol
```

Remove a `volume`:
```
docker volume rm my-vol
```

:exclamation: `Volumes` cannot be removed if `mounted` to a `container`:exclamation: 

:exclamation: The `container` must first be removed in order to remove the `volume`:exclamation:

## :performing_arts: Anonymous Volumes

If you create a `container` and want to save state in a `volume` that is not already created, you can instruct **Docker** to create an `anonymous volume` which will be named with a random string.

```
docker run  -v /foo busybox top
```

You can also instruct **Docker** to automatically remove the `volume` when the `container` is removed with the `--rm` flag.

```
docker run --rm -v /foo -v awesome:/bar busybox top
```

## :lock: Read only volumes

`Volumes` can be `mounted` in `read-only` mode.
```
docker run -d -v nginx-vol:/usr/share/nginx/html:ro  nginx:latest
```

##  :racing_car: Volume Drivers

**Docker** offers the possibility to specify a `driver` for a `volume`. If you want your `volume` to point to something different than a `folder` on the `host file system`, that is possible using `volume drivers`.

Some examples of `volume drivers`:
* **SSHFS** - Use a `volume` that is backed by an **SSHFS** connection
* **CIFS/SAMBA** - Mount a **Samba** `volume` in the `container`


## :books: Documentation

:point_right::link:[Docker Volumes Reference](https://docs.docker.com/storage/volumes/)

