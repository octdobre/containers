# :globe_with_meridians: IPVLAN Networking Exercises

## No Promiscuous Mode Required
Yes

One MAC Address, 20 IP addresses.

Containers share the same MAC as the Host.

## Exercise 1: Layer 2, Two containers on the same network
Create `network`:
```
docker network create -d ipvlan \
--subnet=<subnet of physical network> \
--gateway=<IP of gateway> \
-o ipvlan_mode=l2 \
-o parent=<your Network Interface> my_ipvlan
```
These flags are optional and will default:
```
--gateway=<IP of gateway> \
-o ipvlan_mode=l2 \
```
Open a second `terminal`. Run each `command` in each `terminal`:
```
docker run --name agent1 --rm -it --network my_ipvlan nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network my_ipvlan nicolaka/netshoot /bin/bash
```
Run this command in both and find out each `container's` `ip`:
```
ip a
```
Run this command to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
Let's  inspect the MAC address of the containers:
```
docker container inspect agent1
```
The container has no MAC Address
```
 "Networks": {
                "my_macvlan": {
                     ...
                    "Gateway": "<IP of gateway>",
                    "IPAddress": "<IP of the container>",
                    ...
                    "MacAddress": "",
                }
            }
```
The Container's MacAddress is empty because it shares it with the host.

Find out the MacAddress of the host.
Run this on the linux host where docker is installed:
```
ip a 
```
Ping a container from a Windows machine:
```
ping <IP of the container>
```
Then check the arp table of the container:
```
arp -a <IP of the container>
```
Then check the arp table of the linux host:
```
arp -a <IP of the container>
```

The Physical Address is the same as on the linux host.


Cleanup:
```
docker stop agent1 agent 2
docker network rm my_ipvlan
```

## Exercise 2: Layer 3 Networking

`Layer 3` `networking` means creating an entire new `network` with its dedicated `subnet`.

For `containers` to `reach` the outside and `hosts` to `reach` `containers` inside this `network`
a `static ip route` must be set up either in the `hosts` or in the `router`.

Setting up a `static router` is different with each `router` so please check the respective manual.

Create `network`:
```
docker network create -d ipvlan \
--subnet=192.168.100.0/24 \
--gateway=192.168.100.1 \
-o ipvlan_mode=l3 \
-o parent=<your Network Interface> my_l3_ipvlan
```
Remember, the `subnet` must not overlap with the `host's` real `physical subnet` as in the other examples.

Open a second `terminal`. Run each `command`:
```
docker run --name httpserver -d --network my_l3_ipvlan nginx
docker run --name agent1 --rm -it --network my_l3_ipvlan nicolaka/netshoot /bin/bash
```
Open another `terminal` and find the `ip's` of both `containers` by running:
```
docker network inspect my_l3_ipvlan
```
If you didn't yet add the `static route` now is the moment.

After adding the `static route`, execute in the terminal with the agent1 container:
```
ping google.com
```
A `reply` should be visible in the `terminal`.
This means that there is `connectivity` to and from the outside.

Open a `browser` on a `host` running on the `host network`.

`Navigate` to the `IP` of the `httpserver` `container IP`.

The `nginx` `landing page` should be visible.

This should be an indication that the `Layer 3` `network` has been `deployed` and is accessible.


PS: To create multiple `L3 networks` on the same `host` just add multiple `subnets` to the same 
`docker network`.

```
docker network create -d ipvlan \
--subnet=192.168.100.0/24 \
--gateway=192.168.100.1 \
--subnet=192.168.200.0/24 \
--gateway=192.168.200.1 \
-o ipvlan_mode=l3 \
-o parent=<your Network Interface> my_l3_ipvlan
```

`Containers` in different `subnets` of this `docker IPVLAN Layer 3 network` have `connectivity` 
between themselves.

## :books: Documentation

:point_right::link:[IPVLAN networking](https://docs.docker.com/network/ipvlan/)