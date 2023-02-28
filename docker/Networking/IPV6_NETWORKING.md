# IPV6 Networking Exercises

## Introduction

I have created a special page for network using IPV6 due to it being different in functionality than IPV4.

All docker networks that have IPV6 enabled are dual stacked. Containers will receive both a IPV4 and a IPV6 ip.

Currently (2023) there is no possibility to create a IPV6 only network on docker.

It is important to note that currently (2023) docker supports creating networks with an ip range of exactly `/80` when
the containers need to interract with the physical network. 

## Discovering the IPV6 details of the host network

In order to find the IPV6 details of the network of the host, please consult the menu of the router.

You should be looking for either a IPV6 Prefix or IPV6 Subnet. 

The prefix is usually somewhere around `/50-/60` in subnet range. You need to reduce this to a `/80` subnet in order to use it as a ip range for the docker network.

You can use [IPV6 Subnet Calculator](http://www.gestioip.net/cgi-bin/subnet_calculator.cgi) to calculate the subnet.

Note: You also need to find the IP of the gateway, otherwise routing will not work.

To view the gateway ip:
```
ip -6 route

route -6

netstat -r -n -6
```
To view the ip of the host:
```
ip -6 a
```



## Enabling IPv6

To make containers communicate with each other over IPv6 in Docker, you need to enable IPv6 support for Docker daemon first. You can do this by adding the following options to the `daemon.json` file:
```
cd /etc/docker
```
```
sudo nano daemon.json
```
Copy this to the file and fill in the IP range:
```
{
  "ipv6": true,
  "fixed-cidr-v6": "<IPv6 subnet>"
}
```

## Bridge(User) Network


## MACVLAN & IPVLAN L2 Network

MAVLAN or IPVLAN containers can be exposed to the network of the host. In order to do that the details should match the ones of the router to which the host is connected.

Run this to find the name of the network interface:
```
ip -6 a
```
Create the network using this command:
```
docker network create -d macvlan \
--subnet="<ip range of the host>" \
--ip-range="< a /80 ip range based on the one above>"  \
--gateway="<ip of gateway>" \
-o parent=<network interface> \
--ipv6 \
mac6
```
For an `IPVLAN` network replace the `macvlan` keyword with `ipvlan` in the `driver type`.

Add a debug container to it:
```
docker run --name agent1 --rm -it --network mac6 --cap-add=NET_ADMIN nicolaka/netshoot /bin/bash
```

Find the IPV6 or another host on the network and ping it:
```
ping -6 -c 2  <ipv6>
```

Check conectivity to outside:
```
ping -6 -c 2 ipv6.google.com
```

Run a webserver on the IPV6 network:
```
docker run --name=webserverv6 --network mac6 -d httpd
```

Find out the IP of the webserver:
```
docker network inspect mac6
```

Open a browser on a different host and put the ip of the webserver in the adress bar surrounded by brackets[] like this:
```
hhtp://[2000::]  
//example ip
```

The browser should navigate to a page with the text `It works!`.

Note that because the MACVLAN network is created with the details of the host network, no routes need to be created for duplex conectivity.

## IPVLAN L3 Network

An IPVLAN L3 network is a complete network on its own. 

The network details can be any because it does not depend on the host's network.

The IPVLAN L3 will attach to the hosts network and routes have to be created mandatory.

Duplex resolution might not always be possible in this scenario. 

Create the L3 network:
```
docker network create -d ipvlan \
--subnet="2000::/57" \
--ip-range="2000::1/80"  \
--gateway="2000::1" \
-o parent=<network interface or host> \
-o ipvlan_mode=l3 \
--ipv6 \
ipvlan_v6_L3
```

After the network has been created, routes have to be created.
 
For ingress (North-South) communication, a route has to be set up in the router of the network of the host.
The detials should be the subnet of the IPVLAN L3 network and the ip of the gateway of this network, in which case it is the IP of the docker host. 

In the example above the subnet is `2000::/57`. The gateway value is not the value from the IPVLAN L3 network, but instead the IP of the docker host.

Please consult the manual of the menu of such a device or software on how to add static routes.

Create a debug container:
```
docker run --name agent1 --rm -it --network ipvlan_v6_L3 --cap-add=NET_ADMIN nicolaka/netshoot /bin/bash
```
From a different host ping the IP of the debug container. A reply should be received.


Create a webserver:
```
docker run --name=webserverv6 --network ipvlan_v6_L3 -d httpd
```

Open a browser on a different host and put the ip of the webserver in the adress bar surrounded by brackets[] like this:
```
hhtp://[2000::3]
//add actual ip of webserver
```
The browser should navigate to a page with the text `It works!`.

## :books: Documentation

:point_right::link:[IPV6 Subnet Calculator](http://www.gestioip.net/cgi-bin/subnet_calculator.cgi)