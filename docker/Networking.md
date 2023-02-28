#  :electric_plug: Docker Networking 

## :zap: Unlimited Power!!!

Docker functionality around networks is vast. 

From enclosed networks to containers having their IP on the physical network,  to Layer 3 networks, Dockers offers
the possibility to do a lot of things with networking.


## :abacus: Network Types

### 1. Bridge 
* Docker comes with a default enclosed network type `bridge` which is named also "bridge".
* Ports must be exposed for ingress to containers
* Containers have full egress access
* :exclamation: Containers are automatically assigned to the bridge network if no network flag is specified :exclamation:

---
### 2. User Defined
* Simple network
* Type bridge
* Can define the subnet
* Ports must be exposed for ingress to containers
* Containers have full egress access

Create a bridge network:
```
docker network create my-net
```
Full with the optional flag: 
```
docker network create --driver bridge my-net  
```
Assign container to network:
```
docker run -d -p 9000:8000 \
 -p 9001:9443 \
 --network my-net \
 portainer/portainer-ce:latest
```
---
### 3. Host 
* Type Host
* The container behaves like a service on the host computer
* Ports do not need to be exposed but must be free otherwise they will not be able to bind them

Use the `--network host ` to connect a container directly to the host network:
```
docker run -d  --network host  portainer/portainer-ce:latest
```

---
### 4. MAC VLAN
* Type macvlan
* The container behaves as a physical host on the network
* It receives an IP and MAC from the physical router
* Useful for a legacy application that needs to physically connect to a network
* The network card and router/switch must support promiscuous mode, multiple mac addresses on one network port


#### a) Bridge mode
* The IP should be specified manually otherwise it will clash with other real devices on the network.
* Does not create a network interface on the host

#### b) 802.1q trunk bridge mode
* Works by creating a new sub-interface on the host
* It behaves like a new network interface on the host, each container in this mode will get its network interface
* Trunking - TODO

To create such a network you need to specify the parent as the network card of the host:
```
docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1 \
  -o parent=eth0 pub_net
```
---
### 5. IP VLAN

* Type ipvlan

#### a) Layer 2
* Like MAC VLAN but the container only has a unique IP, the MAC is the same as the Host
* Router must support multiple MAC addresses on different IPs

#### b) Layer 3
* At this level there is full control over the network
* It behaves as a fully new network 
* A static route must be set in the router for ingress to be possible to this new network
* Full control over IP addresses, Routing, and Routes
* Acts like a complete Host

---
### 6. Overlay network
* Used to connect multiple docker hosts under the umbrella of a single network
* Used in swarm scenarios
* TODO
---
### 7. None
* A network with no IP addresses
* Containers assigned to this network receive no IP,
* Containers in this network do not have egress or ingress
* Useful for containers that do not need to access the internet, or be reachable

## :six: Enabling IPv6
Create `file`:
```
sudo nano /etc/docker/daemon.json
```
Copy content:
```
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8:1::/64"
}
```
Reload `docker` daemon:
```
sudo systemctl reload docker
```

## :weight_lifting: Exercises

:point_right: :link: [Docker Networking Bridge Exercises](Networking/Bridge_Exercises.md)

:point_right: :link: [Docker Networking MACVLAN Exercises](Networking/MACVLAN_Exercises.md)

:point_right: :link: [Docker Networking IPVLAN Exercises](Networking/IPVLAN_Exercises.md)

:point_right: :link: [Docker Networking IPV6 Exercises](Networking/IPV6_NETWORKING.md)



## :books: Documentation

:point_right::link:[Overview of networks](https://docs.docker.com/network/)

:point_right::link:[Overview of networks with examples](https://www.youtube.com/watch?v=bKFMS5C4CG0)

:point_right::link:[IPv6](https://docs.docker.com/config/daemon/ipv6/)

:point_right::link:[Build IPv6 Network](https://dev.to/joeneville_/build-a-docker-ipv6-network-dfj)