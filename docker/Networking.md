#  :electric_plug: Docker Networking 

## :zap: Unlimited Power!!!

Docker functionality around networks is vast. 

From enclosed networks to containers having their own IP on the phisical network,  to Layer 3 networks, Dockers offers
the possibility to do alot of things with networking.


## :abacus: Network Types

### Bridge 
* Docker comes with a default enclosed network called "bridge" which is of type `bridge`, ironic.
* Ports must be exposed for ingress to containers
* Containers have full egress access
* :exclamation: Containers are automatically assigned to the bridge network if no network flag is specified :exclamation:

---
### User Defined
* Simple network
* Type bridge
* Can define subnet
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
### Host 
* The container behaves like a service on the host computer
* Ports do not need to be exposed, but must be free otherwise it will not be able to bind them

Use the `--network host ` to connect a container directly to the host network:
```
docker run -d  --network host  portainer/portainer-ce:latest
```

---
### MAC VLAN
* The container behaves as a complete new computer on the network
* It receives a IP and MAC from the phisical router
* The network card and router/switch must support promiscuous mode, multiple mac addresses on one network port


#### Bridge mode
* The IP must be specified manually because the container will not get one from DHCP automatically

#### 802.1q trunk bridge mode
* Works by creating a new sub interface on the host
* It behaves like a new network interface on the host, each container in this mode will get its own network interface
* Trunking - TODO

To create such a network you need to specify the parent as the network card of the host:
```
docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1 \
  -o parent=eth0 pub_net
```
---
### IP VLAN
#### Layer 2
* Like MAC VLAN but the container only has a unique IP, the MAC is the same as the Host
* Router must support multiple MAC addresss on different IP's

#### Layer 3
* At this level there is full controll over the network
* It behaves as a full new network 
* A static route must be set in the router in order for ingress to be possible to this new network
* Full controll over IP addresses, Routing and Routes
* Acts like a complete Host

### Overlay network
* Used to connect multiple docker hosts under the umbrella of a single network
* Used in swarm scenarios
* TODO
	
### None
* A network with no IP addresses
* Containers assinged to this network receive no IP,
* Containers in this network do not have egress, or ingress
* Usefull for containers that do not need to access internet, or be reachable

## :hammer_and_wrench: Commands



## :books: Documentation

:point_right::link:[Overview of networks](https://docs.docker.com/network/)
:point_right::link:[Overview of networks with examples](https://www.youtube.com/watch?v=bKFMS5C4CG0)


