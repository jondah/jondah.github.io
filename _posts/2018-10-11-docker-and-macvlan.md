---
id: 182
title: 'Docker and macvlan'
date: '2018-10-11T12:46:23+00:00'
author: jdahlgren
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=182'
permalink: /2018/10/11/docker-and-macvlan/
geo_public:
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
timeline_notification:
    - '1539258387'
    - '1539258387'
    - '1539258387'
    - '1539258387'
    - '1539258387'
    - '1539258387'
    - '1539258387'
categories:
    - Docker
    - networking
---

If you want to use docker containers in your regular LAN subnet you need to setup a new Docker network with macvlan driver.

First create your Docker network. *— ip-range*  specifies all addresses that Docker will manage. Chose a part of your subnet outside your DHCP-scoop if you have one to avoid ip conflicts.  
*–aux-address=’host=192.168.6.4′ docker\_net* is tied to your host interface to allow your containers to comunicate with your host.  
`<br></br>[root@docker01 ~]# docker network create -d macvlan -o parent=ens224 \<br></br>  --subnet 192.168.6.0/24 \<br></br>  --gateway 192.168.6.1 \<br></br>  --ip-range 192.168.6.192/27 \<br></br>  --aux-address='host=192.168.6.4' docker_net<br></br>`

As you can see when running *docker network ls* we have a new network called docker\_net with macvlan driver.  
<figure aria-describedby="caption-attachment-183" class="wp-caption alignnone" id="attachment_183" style="width: 700px">![](http://192.168.5.181/wp-content/uploads/2018/10/2018-10-10-09_32_47-rootdocker01_.png?w=700)<figcaption class="wp-caption-text" id="caption-attachment-183">Docker network</figcaption></figure>  
Next step is to create a macvlan interface, in this example called docker\_int.  
`[root@docker01 ~]# ip addr add docker_int link ens224 type macvlan mode bridge`

Configure the interface with your selected host address and bring it up. Last step is to add a IP route to tell your host how to connect to to al Docker containers.  
`<br></br>[root@docker01 ~]# ip link add docker_int link ens224 type macvlan mode bridge<br></br>[root@docker01 ~]# ip link set docker-shim up<br></br>[root@docker01 ~]# ip route add 192.168.1.192/27 dev docker_int`

Run a container and connect it to docker\_net  
`[root@docker01 ~]# docker run nginx -network docker_net`

If you want to check container ip run:  
`<br></br>[root@docker01 ~]# docker inspect CONTAINER_ID<br></br>`