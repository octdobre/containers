# :hibiscus: MAC VLAN Networking Exercises

## Exercise 1: Two containers on same network
Create `network`:
```
docker network create -d macvlan \
--subnet=<subnet of physical network> \
--gateway=<IP of gateway> \
-o parent=<your Network Interface> my_macvlan
```
Open a second `terminal`. Run this each `command` in each `terminal`:
```
docker run --name agent1 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
```
Run this commaand in both and find out each `container's` `ip`:
```
ip a
```
Run  this command  to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
Let's  inspect the MAC address of the containers:
```
docker container inspect agent1
```
You should see a mac adress that is unique on the Network.
```
 "Networks": {
                "my_macvlan": {
                     ...
                    "Gateway": "192.168.178.1",
                    "IPAddress": "192.168.178.2",
                    "IPPrefixLen": 24,
                    ...
                    "MacAddress": "02:42:c0:a8:b2:02",
                }
            }
```
This part is unique to your setup.

Open your routers UI menu and visualize the MAC addresses of all devices connected to the network.

Ping google from both containers:
```
ping google.com
```
You should receive a reply meaning that the containers have outside conectivity.

Cleanup:
```
docker stop agent1 agent 2
docker network rm my_macvlan
```

## Exercise 2: 802.Q1 
Macvlan 802.Q1 creates sub-interfaces on the host interface.
These need to be named also in the creation of the network.
Example:
```
mainnic.10
```
We do not need to specify the subnet of the physical network because this is a virtual network
and packages will be router in the physical network with VLAN tags.
Only sub-interfaces with this same VLAN configuration will listen to these ethernet 
packages with the same VLAN tag.

Create `network`:
```
docker network create -d macvlan \
--subnet=10.0.0.0/24 \
--gateway=10.0.0.1 \
-o parent=<your Network Interface>.<add here a number between 1 and 255> my_macvlan
```

Open a second `terminal`. Run this each `command` in each `terminal`:
```
docker run --name agent1 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
```
Run this commaand in both and find out each `container's` `ip`:
```
ip a
```
Run  this command  to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
Let's  inspect the MAC address of the containers:
```
docker container inspect agent1
```
You should see a mac adress that is unique on the Network.
```
 "Networks": {
                "my_macvlan": {
                     ...
                    "Gateway": "10.0.0.1",
                    "IPAddress": "10.0.0.2",
                    "IPPrefixLen": 24,
                    ...
                    "MacAddress": "02:42:c0:a8:b2:02",
                }
            }
```
This part is unique to your setup.

Open your routers UI menu and visualize the MAC addresses of all devices connected to the network.

Run `ip a` on the host of the docker daemon and view the new sub-interface being created.
```
8: mainnic0.10@mainnic0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether <redacted IPv4 IP> brd ff:ff:ff:ff:ff:ff
    inet6 <redacted IPv6 IP>/64 scope link
       valid_lft forever preferred_lft forever
```

Ping google from both containers:
```
ping google.com
```
You should notice that the containers do not have outside conectivity.

Cleanup:
```
docker stop agent1 agent 2
docker network rm my_macvlan
```
## Exercise 3: Two hosts two containers, same vlan

TODO


## :books: Documentation

:point_right::link:[VLAN networking](https://github.com/Shopify/docker/blob/master/experimental/vlan-networks.md)

