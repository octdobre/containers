#  :hammer_and_wrench: Services Containers Reference


An excerpt of the most important specifications for services.


## Basic

* Container Name
* Entrypoint
* Command
* Environment variables

* STDIN OPEN
* TTY 
* PID
* Read Only Filesystem
* Restart Policy

```
services:
  agent1:    #<--- this will not be used as the name of the service                                
    container_name: agent1             # name of service
    image: nicolaka/netshoot           # image used
    entrypoint: /code/entrypoint.sh    # overrides 
    command: /bin/bash                 # command that will run inside container, replaces default CMD command
    stdin_open: true                   # docker run -i
    tty: true                          # docker run -i
    environment:
      RACK_ENV: development            # define environment variables
    pid: 1001
    read_only: true                    # filesystem is readonly
    restart: unless-stopped            # restart policy
```


## Networking

* Network
* DNS
* Hostname  (All hostnames are domain names)
* Domain Name
* Aliases 
* Extra hosts    
* Exposing ports
* MAC Address

```
services:
  agent1:                       
    image: nicolaka/netshoot
    ports:                        # open port
      - "443:8043"                # single port
      - "9090-9091:8080-8081"     # port range, yes even different, should be same number
      - "6060:6060/udp"           # also protocol 
    domainname: 'agent1'          # domain name
    hostname: 'agent1'            # hostname        
    dns:
      - 8.8.8.8
      - 9.9.9.9
    aliases:
      - agent1.2
      - agent1.3           
    networks:                
      net1-ds:
        ipv4_address: 10.0.10.2
        ipv6_address: 2001:3984:3989::10
    extra_hosts:
      - "somehost:162.242.195.82"
      - "otherhost:50.31.209.229"
    mac_address: 00-B0-D0-63-C2-26 
...
```

## Connecting to a Volume

* Simple volume mount
* Read Write volume : rw
* Read Only  volume : ro
* Volumes mounted from other containers
* Temporary File System

```
services:
  agent1:                       
    image: nicolaka/netshoot           
    volumes:
      - volume1:/etc/data
      - volume2:/etc/data/readwrite:rw     
      - volume3:/etc/data/readonly:ro
      - /host/path:/container/path         # Bind mount
      - ./compose/path:/container/path     # Bind mount, relative to docker compose folder 
    volumes_from:
      - service_name1:ro
      - service_name2:rw
    tmpfs:                                 # Temporary File System
      - /run
      - /tmp
...
```

## Healthchecks

* Container Healthcheck
* Start container only after other containers started successfully

```
services:
  web1:                       
    image: nginx
  web2:                       
    image: nginx
    healthcheck:                                         # healthcheck 
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s
    depends_on: web1                                     # web1 depends on web2
  web3:                       
    image: nginx
    depends_on:                                          # web3 depends on both web1 and web1
      - web1
      - web2
  web4:
    image: nginx
    depends_on:
      web2:
        condition: service_healthy                      # web2 must be healthy for web4 to start
      web3:
        condition: service_started                      # web3 must be started for web5 to start   
...
```

## Deployment

* Deploy Mode (global, replicated)  [global=one container per node]
* Replica numbers
* Placement Preferrence
* Placement Constraints
* Resouce Limits
* Device Capabilities
* Restart Policy

```
services:
  web1:                       
    image: nginx
    deploy:
      mode: replicated                         
      replicas: 2                              # Number of containers 
      placement:       
        preferences:                           
          - datacenter=us-east                 # Placement preffered location
        constraints:
          - disktype=ssd                       # Placement constained capability
      resources:                               # Resouce Limits
        limits:
          cpus: '0.50'
          memory: 50M
          pids: 1                              # Process ID Limits
        reservations:
          cpus: '0.25'
          memory: 20M
          devices:
            - capabilities: ["nvidia-compute"]                        
              driver: nvidia
            - capabilities: ["gpu"]
              device_ids: ["GPU-f123d1c9-26bb-df9b-1c23-4a731f61d8c7"]
            - capabilities: ["tpu"]                 #AI Processor
              count: 2
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
```

## Documentation

:point_right::link:[Docker Compose Deploy](https://docs.docker.com/compose/compose-file/deploy/)