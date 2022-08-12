#  :package: Docker Containers 

## :computer: Classic Virtual Machines

A **Docker**:whale2: `container`:package: is similar to a `virtual machine`:computer:.

A **Docker**:whale2: `container`:package: is a running form of a Docker:whale2: `image` just like a `virtual machine` is 
a running form of a `system image`.

On Linux, docker containers use [OS-Level Virtualization](https://en.wikipedia.org/wiki/OS-level_virtualization).

In short, it is a thinner `virtualization` layer that offers the possibility to run many more `virtual machines`.
`Containers` can start much faster than `virtual machines` and use way less `memory` making, making them very efficient, 
while maintaining almost the same level of `virtualization` as the [hypervisor model](https://de.wikipedia.org/wiki/Hypervisor).

---
## :running: Run it!

Run this command and create your first `container`:package:: 
```
docker run --rm hello-world
```

Run this command to create a `linux`:penguin: `container`:
```
docker run --rm -it ubuntu bash
```

Run this to create a `container` with a `web`:globe_with_meridians: page:
```
docker run --rm -it -p 80:80 yeasy/simple-web:latest
```

Congratulations:tada:, you have created your first `container(s)`:package:(:package:)!


## :gear: Commands

A few commands to manage `containers`:

:mag_right: View all `containers` and their id's:
```
docker ps -a
```
:arrow_forward: Start a stopped `container`:
```
docker start <container id>
```
:stop_sign: Stop a running `container`(gracefully):
```
docker stop <container id>
```
:arrows_counterclockwise: Restart a `container`:
```
docker restart <container id>
```
:skull_and_crossbones: Kill a `container`(does not stop the container gracefully):
```
docker kill <container id>
```
:pause_button: Pauses a `container`:
```
docker pause <container id>
```
:arrow_forward: Unpause a `container`:
```
docker unpause <container id>
```
:wastebasket: Remove a `container`(must be stopped first):
```
docker rm <container id>
```
:mag_right: Inspect details of a container:
```
docker inspect <container id>
```
:mag_right: Stats about the `container`:
```
docker stats <container id>
```

## :whale2: Deep Dive!

Typing run commands can be confusing so here is an outline of the `run` command.


`docker run  [1] [2] [3] [4]`

1. `Flags` for creating the `container`. All flags are optional:exclamation:
2. Name of the `image`. This is a mandatory section:exclamation:
3. A `command` to run inside the `container` after it has started. This section is optional:exclamation:
4. `Arguments` for the `command` to be run inside the `container`. This field is optional:exclamation:

:hammer:Breakdown:

```
docker run --rm -it ubuntu bash

docker run | --rm -it | ubuntu | bash 
```
[1] is `--rm -it`

[2] is `ubuntu`

[3] is `bash`


## :dna: Container lifecycle

`Containers` are like a computer. They start up, run, and shut down(by command). 

A `container` starts a single `process` and, if that `process` keeps running, then the `container` keeps running.

**If the `process` stops then the `container` will stop. This is the default behavior.** :exclamation:

You can start a `container` that runs a `batch script`, and when the `script` finishes the `process` end and therefore 
the `container` stops. 

Most `containers` are meant to host `services` such as **nginx** and **redis**. These are long-running `processes` and keep the `container` running. 

Executing `commands` such as `docker stop` and `docker pause` can manipulate the running state of a `container`.

Sometimes a `process` can terminate unexpectedly and therefore makes the `container` stop. 

In `containers` that run applications such as **nginx**, this could  still happen in rare cases.

The **Docker** `daemon` controls the lifecycle of `containers`, but itself has a lifecycle. When the `daemon` starts, only
some `containers` will start. 

When the `daemon` stops or crashes, all `containers` stop. :exclamation:

## :joystick: Container control

When creating `containers`, the most important `commands` go inside the [1] options block.

In this section, we will look at options that manage the created `container`.

### :pen: Naming

You can name a `container` with the command `--name`. 

### :electric_plug: Ports 

**Docker** `containers` run on a `virtual network` inside the **Docker** engine. They get assigned an `IP address` in this `virtual network`.

Exposing a `service` like **nginx** running inside a `container`:
```
docker run -p 80:80 nginx
```

`-p 80:80`  specifies that the `port` `80` inside the `container` is exposed on port `80` on the `localhost`.

This `service` can be reached at `127.0.0.1:80`.

The first number is the `port` on the `localhost` and the second number is the `port` inside the `container`.

Example:
```
docker run 8080:80 nginx
```

`8080` is the `port` that will be available locally at `127.0.0.1:8080`.
`80` is the port at which **nginx** can be reached inside the `container`.

Not all services bind the port `80`. For these services you have to specifiy the `port` that they bind.
For this, you must read the documentation of that specific service(ex:redis) to know which `port` to expose.

```
docker run -d -p 9000:8000 \
 -p 9001:9443 \
 portainer/portainer-ce:latest
```

Multiple `ports` can be exposed. 

**You cannot expose `container ports` to the same `host port`.** :exclamation:

### :put_litter_in_its_place: Autoremove
You have already seen the flag `--rm`. This means that the `container` is deleted after it has stopped.

### :mag: Foreground vs Detached, and Attaching
The option `-it` is a combination of `-i` and `-t`
```
-t              : Allocate a pseudo-tty
-i              : Keep STDIN open even if not attached
```

Otherwise, you can use `docker attach <container id>` to attach to a running `container` after
it has started.

And then you can use `docker exec -it <container id> bash` if you need multiple `terminals` inside the `container`.

It is used to start the `process` in the `container` and attach the `console` to the `process’s` `standard input`. 

This way you can start using `commands` inside the `container`.

For most cases, we will use `containers` in *detached mode*.
To do this specify the `-d` option. 

This will create the `container` without attaching it to its `console`.

**By default when creating a container, if `-d` is not specified, it will attach to its console.**

### :dna: Lifecycle Options

`Container` lifecycle can be specified or modified with `--restart <>`

Example
```
docker run -d --restart unless-stopped redis
```
With `--restart unless-stopped` we can specify that a `container` will always restart unless stopped manually.
If the `daemon` restarts, the `container` will restart, but if the `container` has been stopped manually then
it won't start up when the `daemon` starts.


`--restart always` will make the `container` restart or start even if it is stopped manually or crashes or the `daemon` is restarted.

`on-failure` will restart the container only if it crashed.


### :deciduous_tree: Environment Variables

`Environment variables` can be set, and overrides the default values.
```
docker run -e SOMEVARIABLENAME=<value here>  ubuntu
```


## :books: Documentation

:link:[Docker Run Reference](https://docs.docker.com/engine/reference/run/)

:link:[Start Containers Automatically](https://docs.docker.com/config/containers/start-containers-automatically/)
