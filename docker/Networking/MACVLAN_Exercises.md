# :hibiscus: MAC VLAN Networking Exercises

## Promiscuous Mode
`MACVLAN` `networks` work because of functionality called `promiscuous mode`.
This feature allows multiple `MAC` addresses to be assignable to a single `ethernet physical port`.

In **Linux** it is enabled with the following command:
```
sudo ip link set <name of your ethernet interface> promisc on
```
Furthermore, the `router` must enable `promiscuous mode`. Normally it is enabled by default.
This part is unique to your setup.
Read the manual of the `router` on how to enable this if the device offers this feature.

## IP Availability

The `MACVLAN` will overlay the real physical `network` therefore it will share the same
`IP range` as all of the `network devices`. By default, it will start numbering `IPs` of `containers`
from 1 if no `IP` is specified for a `container`. If the `IP` is already occupied by another device,
the container will fail to be created. 
To assign a specific `IP` to a `container` the following `flag` must be used:
```
--ip <your ip here>
```


## Exercise 1: Two containers on the same network
Create `network`:
```
docker network create -d macvlan \
--subnet=<subnet of physical network> \
--gateway=<IP of gateway> \
-o parent=<your Network Interface> my_macvlan
```
Open a second `terminal`. Run each `command` in each `terminal`:
```
docker run --name agent1 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
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
You should see a mac address that is unique on the Network.
```
 "Networks": {
                "my_macvlan": {
                     ...
                    "Gateway": "<IP of gateway>",
                    "IPAddress": "<IP of the container>",
                    "IPPrefixLen": 24,
                    ...
                    "MacAddress": "02:42:c0:a8:b2:02",
                }
            }
```
This part is unique to your setup.

Open your router's UI menu and visualize the `MAC` addresses of all devices connected to the `network`.

Ping google from both `containers`:
```
ping google.com
```
You should receive a reply meaning that the `containers` have outside connectivity.

Other devices on the network cannot ping these containers but they can still reach these containers.

Cleanup:
```
docker stop agent1 agent 2
docker network rm my_macvlan
```

## Exercise 2: 802.1Q 

TODO - TESTING 

Macvlan 802.1Q creates sub-interfaces on the host interface.
These need to be named also in the creation of the network.
Example:
```
mainnic.10
```
We do not need to specify the subnet of the physical network because this is a virtual network
and packages will be a router in the physical network with VLAN tags.
Only sub-interfaces with this same VLAN configuration will listen to these ethernet 
packages with the same VLAN tag.

Create `network`:
```
docker network create -d macvlan \
--subnet=10.0.0.0/24 \
--gateway=10.0.0.1 \
-o parent=<your Network Interface>.<add here a number between 1 and 255> my_macvlan
```

Open a second `terminal`. Run each `command` in each `terminal`:
```
docker run --name agent1 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network my_macvlan nicolaka/netshoot /bin/bash
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
You should see a mac address that is unique on the Network.
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

Open your router's UI menu and visualize the MAC addresses of all devices connected to the network.

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
You should notice that the containers do not have outside connectivity.

Cleanup:
```
docker stop agent1 agent 2
docker network rm my_macvlan
```
## Exercise 3: Two hosts two containers, same VLAN

TODO 


## :books: Documentation

:point_right::link:[VLAN networking](https://github.com/Shopify/docker/blob/master/experimental/vlan-networks.md)

