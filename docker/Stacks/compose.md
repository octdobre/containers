#  :books: Docker Stacks (Docker Compose)

#  :ring: One File To Rule Them All

**Docker Stacks**⟨™⟩ is a feature to create and manage multiple `containers` under one umbrella called a **Stack**⟨™⟩.

The `tool` is now integrated into the **Docker** `command line tool`.

The short story is that you can define multiple `containers`, `networks`, `volumes` and other **Docker** `objects` within
one `file`. With one `command` many `objects` can be created or removed.


# :musical_score: Compose

A **Docker** `compose file` is of type `YAML`.

Here is an example of a `compose file` that creates two `containers` and one `bridge network`:
```
version: "3.9" 
services:
  agent1:                       
    container_name: agent1      
    image: nicolaka/netshoot    
    command: /bin/bash          
    stdin_open: true            
    tty: true                   
    networks:                
      net1-ds:
        ipv4_address: 10.0.10.2
  agent2:
    container_name: agent2
    image: nicolaka/netshoot
    command: /bin/bash
    stdin_open: true 
    tty: true        
    networks:
      net1-ds:
        ipv4_address: 10.0.10.3
networks:
  net1-ds:
    name: net1
    ipam:
      driver: default
      config:
        - subnet: "10.0.10.0/24"
          ip_range: "10.0.10.0/24"
          gateway: 10.0.10.1
```

The `command` to `run` this `file` is:
```
docker compose up -d
```
This will create the **Stack**.

The `file name` must be `docker-compose.yml`, otherwise it must be specified explicitly:
```
docker compose up -f <file-name>
``` 

To remove the stack:
```
docker compose down
```

To only stop the `containers` in the **Stack**:
```
docker compose stop
```
And to start them again:
```
docker compose start
```

All of the same commands for `containers` also apply here such as :

`starting, stopping, pausing, unpausing, killing, pulling, pushing, logging and etc.`

# :mag_right: Specific Topics

To view references and examples:

:point_right::link:[Services Containers](service_container.md)

:point_right::link:[Networking](stack_network.md)

:point_right::link:[Volumes](stack_volume.md)

:point_right::link:[Exercises](stacks_exercises.md)


# :books: Documentation

:point_right::link:[Docker Compose](https://docs.docker.com/compose/)

:point_right::link:[Docker Compose Specification](https://docs.docker.com/compose/compose-file/)