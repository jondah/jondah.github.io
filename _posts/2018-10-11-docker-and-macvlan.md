---
title: "Docker and macvlan"
date: "2018-10-11"
categories: 
  - "docker"
  - "networking"
redirect_from:
 - 2018/10/11/docker-and-macvlan/
---

If you want to use docker containers in your regular LAN subnet you need to setup a new Docker network with macvlan driver.

First create your Docker network. _\-- ip-range_ specifies all addresses that Docker will manage. Chose a part of your subnet outside your DHCP-scoop if you have one to avoid ip conflicts. _\--aux-address='host=192.168.6.4' docker\_net_ is tied to your host interface to allow your containers to comunicate with your host. `[root@docker01 ~]# docker network create -d macvlan -o parent=ens224 \ --subnet 192.168.6.0/24 \ --gateway 192.168.6.1 \ --ip-range 192.168.6.192/27 \ --aux-address='host=192.168.6.4' docker_net`

As you can see when running _docker network ls_ we have a new network called docker\_net with macvlan driver. \[caption id="attachment\_183" align="alignnone" width="700"\]![](/wp-content/uploads/2018/10/2018-10-10-09_32_47-rootdocker01_.png?w=700) Docker network\[/caption\] Next step is to create a macvlan interface, in this example called docker\_int. `[root@docker01 ~]# ip addr add docker_int link ens224 type macvlan mode bridge`

Configure the interface with your selected host address and bring it up. Last step is to add a IP route to tell your host how to connect to to al Docker containers. `[root@docker01 ~]# ip link add docker_int link ens224 type macvlan mode bridge [root@docker01 ~]# ip link set docker-shim up [root@docker01 ~]# ip route add 192.168.1.192/27 dev docker_int`

Run a container and connect it to docker\_net `[root@docker01 ~]# docker run nginx -network docker_net`

If you want to check container ip run: `[root@docker01 ~]# docker inspect CONTAINER_ID`
