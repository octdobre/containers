# :bridge_at_night: Bridge(User Defined) Networking Exercises

## Exercise 1: Two containers on same network

Create `network`:
```
docker network create --subnet 192.168.10.0/24 net1
```

Open a second `terminal`. Run this each `command` in each `terminal`:

```
docker run --name agent1 --rm -it --network net1 nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network net1 nicolaka/netshoot /bin/bash
```

Run this commaand in both and find out each `container's` `ip`:
```
ip a
```

Run  this command  to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
Cleanup:
```
docker stop agent1 agent 2
docker network rm net1
```


## Exercise 2: Three containers, two networks, one on both networks

Create `networks`:
```
docker network create --subnet 192.168.10.0/24 net1
docker network create --subnet 192.168.20.0/24 net2
```

Open two more `terminals`. Run this each command in each `terminal`:

```
docker run --name agent1 --rm -it --network net1 nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network net2 nicolaka/netshoot /bin/bash
docker run --name agent3 --rm -dit  nicolaka/netshoot /bin/bash
```

We will connect agent3 to both networks:
```
docker network connect net1 agent3
docker network connect net2 agent3
```
And then attach to agent3:
```
docker attach agent3
```
Run this command in all and find out each `container's` `ip`:
```
ip a
```
Run  this command  to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
You will notice that only agent3 can reach both containers.
agent1 can only reach agent3 and agent2 can only reach agent3.
This is because only agent3 is attached to both networks.

Inspect the network interfaces on the host:
```
ip a
```
Notice that containers agent1 and agent2 have each one virtual network interface on the host:
```
28: eth0@if29: ...
    inet 192.168.10.2/24 
```
Notice that container agent3 has 3 network interfaces even though it is only connected to 2 networks:
```
32: eth0@if33: ...
    inet 172.17.0.2/16 
34: eth1@if35: ...
    inet 192.168.10.3/24 
36: eth2@if37: ...
    inet 192.168.20.3/24 
```
We did not specify a network when we created agent3 so by default it was assigned to the default "bridge" network.

Cleanup:

```
docker stop agent1 agent2 agent3
docker network rm net1 net2
```


## Exercise 3: Dual stack network

TODO

Create `network`:
```
docker network create --subnet 192.168.10.0/24 --subnet "2001:db8:2::/64" --ipv6  netds
``
Open two more `terminals`. Run this each command in each `terminal`:
```
docker run --name agent1 --rm -it --network netds nicolaka/netshoot /bin/bash
docker run --name agent2 --rm -it --network netds nicolaka/netshoot /bin/bash
``
Inspect the network interfaces on the host:
```
ip a
```
Both containers whould have assigned an IPv4 and an IPv6 IP to each.

Run  this command  to check if the `containers` can reach each other.
```
ping -c 2 <ip of other container>
```
Try the same command using the IPv6 ip.

Cleanup:

```
docker stop agent1 agent2 
docker network rm netds
```
