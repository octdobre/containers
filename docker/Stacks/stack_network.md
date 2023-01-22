# :electric_plug: Stack Networks

## Bridge Network

* Network Definition
* IPAM Driver
* IPV6 Network

```
services:
  agent1:                       
    image: nginx
    networks:
      net-nginx:
        ipv4_address: 10.0.10.2
networks:
  net-nginx:
    name: net-nginx
    enable_ipv6: true
    ipam:
      driver: default
        config:
          - subnet: "10.0.10.0/24"
            ip_range: "10.0.10.0/24"
            gateway: 10.0.10.1
          - subnet: "2001:db8:2::/64"
            ip_range: "2001:db8:2::/64"
            gateway: "2001:db8:2::1"
...
```

## MACVLAN-IPVLAN

* MACVLAN
* DRIVER OPTIONS
* IPVLAN MODE

```
services:
  agent1:                       
    image: nginx
    networks:
      net-nginx-mac:
        ipv4_address: 10.0.10.2
      net-nginx-ip:
        ipv4_address: 10.0.11.2
networks:
  net-nginx-mac:
    name: net-nginx-mac
    ipam:
      driver: macvlan
        driver_opts:
          parent: <name of nic>
        config:
          - subnet: "10.0.10.0/24"
            ip_range: "10.0.10.0/24"
            gateway: 10.0.10.1
  net-nginx-ipvlan:
    name: net-nginx-ip
    ipam:
      driver: macvlan
        driver_opts:
          parent: <name of nic>
          ipvlan_mode: l2 
        config:
          - subnet: "10.0.11.0/24"
            ip_range: "10.0.11.0/24"
            gateway: 10.0.11.1
...
```

## Host

* Host Network
```
services:
  agent1:                       
    image: nicolaka/netshoot
    networks:
      hostnet: {}
networks:
  hostnet:
    external: true
    name: host
...
```

## None

* None Network
```
services:
  agent1:                       
    image: nicolaka/netshoot
    networks:
      nonet: {}
networks:
  nonet:
    external: true
    name: none
...
```
