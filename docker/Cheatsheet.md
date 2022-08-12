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
| `docker load < imagename.tar`| Load image from file on disk  |
| `docker image prune -a`| Remove all unused images  |
| `docker image prune -a --filter "until=24h"`| Remove all unused images in with age up to 24h  |



## Build

## Containers

## Volumes

## Networking
