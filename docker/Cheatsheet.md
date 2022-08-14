# Cheat sheet of :whale2:Docker CLI commands

## Images

| Command                         | Description |
| -------------                   | ------------- |
| `docker images`          | View all images  |
| `docker pull redis`         | Pull a docker image from docker hub  |
| `docker rmi redis`         | Remove a docker image<  |
| `docker tag redis redis:3 `  | Tag a docker image  |
| `docker rmi redis:1`        | Remove docker image by tag id  |
| `docker rmi 53aa81e8adfa`   | Remove docker image by unique id  |
| `docker inspect redis`      | Inspect docker image  |
| `docker image tag myService:1 myPrivateRegistry/myService:1`| Tag image to push to private registry  |
| `docker image push myPrivateRegistry/myService:1`| Push image to private registry  |
| `docker container commit c16378f943fe myImage:1` | Create image from running contaienr by saving its current state  |
| `docker save <imagename_here> > imagename.tar`| Save image to file on disk  |
| `docker load < imagename.tar`| Load image from a file on disk  |
| `docker image prune -a`| Remove all unused images  |
| `docker image prune -a --filter "until=24h"`| Remove all unused images in with age up to 24h  |

## Containers

| Command                         | Description |
| -------------                   | ------------- |
| `docker ps `             | View running containers  |
| `docker ps -a`             | View all containers, including stopped ones  |
| `docker start <container id>`  | Start a stopped container  |
| `docker stop <container id>`  | Stop a running container  |
| `docker pause <container id>`  | Pause a running container  |
| `docker unpause <container id>`  | Unpause a running container  |
| `docker restart <container id>`  | Restart a running container  |
| `docker kill <container id>`  | Force stop a running container  |
| `docker rm <container id>`  | Remove a stopped container  |
| `docker inspect <container id>`  | Inspect details about a container  |
| `docker stats <container id>`  | View stats about a container  |
| `docker container prune`  | Removes all stopped containers  |
| `docker container prune -filter "until=24h`  | Removes all stopped containers created more than 24 hours ago |

## Volumes

| Command                         | Description |
| -------------                   | ------------- |
| `docker volume ls`             | View all volumes  |
| `docker volume create my-vol`    | Create named volume |
| `docker volume inspect my-vol`    | Inspect volume |
| `docker volume rm my-vol`    | Remove volume |

## Networking

## Image creation

## Container creation
| Command                         | Description |
| -------------                   | ------------- |
| `docker run -d nginx`   | Create basic container |
| `docker run -d --name ingress nginx`   | Name it |
| `docker run -it ubuntu bash`   | Attach to its console |
| `docker run --rm busybox top`   | Removes itself automatically after it stops |
| `docker run -d -p 80:80 yeasy/simple-web:latest`   | Expose port 80 |
| `docker run -d --restart unless-stopped redis`   | Restarts itself unless manually stopped |
| `docker run -e SOMEVARIABLENAME=<value here>  ubuntu`   | Set Environment variable |
| `docker run -d -v myvol2:/app  nginx`   | Mount volume |
| `docker run -d -v /containers/nginx_data:/app  nginx`   | Mount bind mount |
| `docker run  -v /foo busybox top`   | Mount anonymous volume |

## Compose


