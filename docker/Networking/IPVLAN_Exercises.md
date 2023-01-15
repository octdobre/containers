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

