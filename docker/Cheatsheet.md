# Cheat sheet of :whale2:Docker CLI commands

## Images

| Command                                                     | Description |
| -------------                                               | ------------- |
| `docker images`                                             | View all images  |
| `docker pull redis`                                         | Pull a docker image from docker hub  |
| `docker rmi redis`                                          | Remove a docker image<  |
| `docker tag redis redis:3 `                                 | Tag a docker image  |
| `docker rmi redis:1`                                        | Remove docker image by tag id  |
| `docker rmi 53aa81e8adfa`                                   | Remove docker image by unique id  |
| `docker inspect redis`                                      | Inspect docker image  |
| `docker image tag myService:1 myPrivateRegistry/myService:1`| Tag image to push to private registry  |
| `docker image push myPrivateRegistry/myService:1`           | Push image to private registry  |
| `docker container commit c16378f943fe myImage:1`            | Create image from running container by saving its current state  |
| `docker save <imagename_here> > imagename.tar`              | Save image to file on disk  |
| `docker load < imagename.tar`                               | Load image from a file on disk  |
| `docker image prune -a`                                     | Remove all unused images  |
| `docker image prune -a --filter "until=24h"`                | Remove all unused images in with age up to 24h  |

## Containers

| Command                                      | Description |
| -------------                                | ------------- |
| `docker ps `                                 | View running containers  |
| `docker ps -a`                               | View all containers, including stopped ones  |
| `docker start <container id>`                | Start a stopped container  |
| `docker stop <container id>`                 | Stop a running container  |
| `docker pause <container id>`                | Pause a running container  |
| `docker unpause <container id>`              | Unpause a running container  |
| `docker restart <container id>`              | Restart a running container  |
| `docker kill <container id>`                 | Force stop a running container  |
| `docker rm <container id>`                   | Remove a stopped container  |
| `docker inspect <container id>`              | Inspect details about a container  |
| `docker stats <container id>`                | View stats about a container  |
| `docker container prune`                     | Removes all stopped containers  |
| `docker container prune -filter "until=24h`  | Removes all stopped containers created more than 24 hours ago |

## Volumes

| Command                            | Description |
| -------------                      | ------------- |
| `docker volume ls`                 | View all volumes  |
| `docker volume create my-vol`      | Create named volume |
| `docker volume inspect my-vol`     | Inspect volume |
| `docker volume rm my-vol`          | Remove volume |
| `docker volume ls -f dangling=true`| View dangling volumes |

## Networking

| Command                                        | Description |
| -------------                                  | ------------- |
| `docker network ls`                            | List all networks  |
| `docker network create my-net`                 | Create a quick bridge type network  |
| `docker network inspect my-net`                | View details about a network  |
| `docker network rm my-net`                     | Remove a network  |
| `docker network connect my-net container1`     | Connect container1 to my_net network  |
| `docker network disconnect my-net container1`  | Disconnect container1 from my_net network  |
| `docker network create --driver bridge my_net` | Specify type(driver) of network  |

## System
| Command                                        | Description |
| -------------                                  | ------------- |
| `docker info`                                  | Display system-wide information  |
| `docker system prune -f`                       | Removes untagged images and dangling volumes and containers  |
| `docker system df`                             | Inspect disk usage  |

## Dockerfile

| Command                                     | Description |
| -------------                               | ------------- |
| `FROM <image name>`                         | Specify base image  |
| `FROM <image name>:<image tag>`             | Specify base image with specific tag  |
| `FROM <image name>@<digest>`                | Specify base image with specific sha digest |
| `FROM <image name>@<digest>`                | Specify base image with specific sha digest |
| `ARG Argument1`                             | Specify argument |
| `ARG Argument1=defaultvalue`                | Specify argument with default value |
| `FROM <image name>:$Argument1`              | Argument usage |
| `FROM <image name>:$Argument1`              | Argument usage |
| `ENV ASPNETCORE_ENVIRONMENT="development"`  | Specify environment variable |
| `LABEL buildNumber="1.1.1"`                 | Specify image metadata label |
| `WORKDIR /app`                              | Specify work directory |
| `WORKDIR /app`                              | Specify work directory |
| `COPY . .`                                  | Copy all files and folders from host to image filesystem |
| `ADD <url> .`                               | Add file from URL to image filesystem |
| `ADD --chown=myuser:mygroup . .`            | Add as myuser and mygroup in the image |
| `USER myuser:mygroup`                       | Sets the user and group that further directives will run under |
| `RUN apt-get && apt-upgrade`                | Run a command whos result is added as a new layer  |
| `ENTRYPOINT ["dotnet","Weather.API"]`       | Set container executable EXEC mode|
| `ENTRYPOINT dotnet Weather.API`             | Set container executable SHELL mode |
| `CMD ["-p"]`                                | Provide further parameters to the executable |


## Image creation

| Command                                           | Description |
| -------------                                     | ------------- |
| `docker build -t <image name>:<image version> . ` | Build and name image  |

## Container creation
| Command                                                 | Description |
| -------------                                           | ------------- |
| `docker run -d nginx`                                   | Create basic container |
| `docker run -d --name ingress nginx`                    | Name it |
| `docker run -it ubuntu bash`                            | Attach to its console |
| `docker run --rm busybox top`                           | Removes itself automatically after it stops |
| `docker run -d -p 80:80 yeasy/simple-web:latest`        | Expose port 80 |
| `docker run -d --restart unless-stopped redis`          | Restarts itself unless manually stopped |
| `docker run -e SOMEVARIABLENAME=<value here>  ubuntu`   | Set Environment variable |
| `docker run -d -v myvol2:/app  nginx`                   | Mount volume |
| `docker run -d -v /containers/nginx_data:/app  nginx`   | Mount bind mount |
| `docker run  -v /foo busybox top`                       | Mount anonymous volume |

## Compose

| Command                                | Description |
| -------------                          | ------------- |
| `docker compose up -d`                 | Deploy the docker stack in detached mode    |
| `docker compose  -f <file-name> up -d` | Deploy the docker stack from specific file  |
| `docker compose down`                  | Remove the docker stack deployment          |
| `docker compose stop`                  | Stop all containers but dont remove         |
| `docker compose start`                 | Start all containers                        |