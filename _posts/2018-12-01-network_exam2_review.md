---

layout: post
title: "Computer Network Exam2 Review outline"
date: 2018-12-01
description: "Chapter 4,5,8,9"

tags: network

## network layer

### two key functions

- **forwarding**: move packets from router’s input to appropriate router output

- **routing**: determine route taken by packets from source to destination

  - routing algorithms



### data plane & control plane

#### data plane

- determine how datagram arriving on router input port is forwarded to router output port

- local, per-router function

- forwarding function

#### control plane

- determine how datagram is routed among routers along end-end path from source host to destination host

- network-wide logic

- two approaches:

  - traditional routing algorighms - implemented in routers

  - SDN - implemented in (remote) servers



### routing algorighm

#### link-state

- use global info

- dijkstra

```java
Initialization:
 N’ = {u}
 for all nodes v
 	if v is a neighbor of u
     then D(v) = c(u, v)
	else D(v) = ∞
   
Loop
	find w not in N’ such that D(w) is a minimum
	add w to N’
	update D(v) for each neighbor v of w and not in N’:
		D(v) = min(D(v), D(w)+ c(w, v) )
    /* new cost to v is either old cost to v or known
		least path cost to w plus cost from w to v */
until N’= N
     

```



#### distance vevtor

- iterative, asynchronous, and distributed

- Bellman Ford

  - Dx(y) = minv{c(x, v) + Dv(y)} (v is the neighbor of x)

```java
Initialization:
for all destinations y in N:
Dx(y)= c(x, y)/* if y is not a neighbor then c(x, y)= ∞ */
for each neighbor w
Dw(y) = ? for all destinations y in N
for each neighbor w
send distance vector Dx = [Dx(y): y in N] to w
loop
	wait (until I see a link cost change to some neighbor w or
           until I receive a distance vector from some neighbor w)
	for each y in N:
		Dx(y) = minv{c(x, v) + Dv(y)}
	if Dx(y) changed for any destination y
		send distance vector Dx = [Dx(y): y in N] to all neighbors 18
forever
```

#### compare

| Algorithm | message complexity         | speed                          | robustness (if one node fault)       |
|:--------- |:-------------------------- |:------------------------------ | ------------------------------------ |
| LS        | O(nE) msgs sent each       | O(n^2)                         | each node compute only its own table |
| DV        | exchange between neighbors | varies (from O(n) to infinity) | each node uses other's data          |

### DHCP

- Dynamic Host Configuration Protocol

- dynamically get address: *plug-and-play*

  - host broadcasts `DHCP discover` msg

  - DHCP server responds with `DHCP offer` msg

  - host request IP address: `DHCP request` msg

  - DHCP server sends address: `DHCP ack` msg

- DHCP use UDP, because the host has no idea with DHCP server's IP address at beginner, so it **broadcast** the DHCP discover msg. TCP doesn't support broadcast.



### get a datagram from source to dest

- from A (223.1.1.1) to B (223.1.1.2): send datagram directly from A to B

- from A (223.1.1.1) to C (223.1.2.2): send from A via 223.1.1.4 and 223.1.2.9 to B



### IP Fragmentation & Reassembly

- reason: link layer have MTU (max transfer size), different link types, different MTUs

- large IP datagram divided into small

- reassembleed only at final destination

- e.g: datasize is 4000 ( data 3980 + header 20), MTU is 1500. we can divide it to 3 part

  - length = 1500 (data 1480 + header 20), offset = 0, fragflag = 1

  - length = 1500 (data 1480 + header 20), offset = 1480, fragflag = 1

  - length = 1040 (data 1020 + header 20), offset = 2960, fragflag = 0



### ICMP

- Internet Control Message protocol

- used by hosts, routers, gageways to communication network-level information

  - error reporting

  - echo request/reply (used by ping)

- ICMP msgs are carried in IP datagram



### Scalable routing

- aggregate routers into regions known as `autonomous system` (AS)

- intra-AS routing

  - routing among hosts, routers in same AS

  - all routers in AS must run **same** intra-domain routing protocol

  - routers in different AS can run different intra-domain routing protocol

  - gateway router: at *edge* of its own AS, has link to routers in other AS

- inter-AS routing

  - routing among AS

  - gateways perform inter-domain routing



#### Inter-AS

- suppose router in AS1 receives datagram destined outside of AS1, AS1 should:

  - learn which dests are reachable through AS2, which through AS3, ...

  - propagate this reachability info to all routers in AS1



### Intra-AS Routing

- also known as **interior gateway protocols (IGP)**

- most common intra-AS routing protocols:

  - `RIP` Routing Information Protocol

  - `OSPF` Open Shortest Path First

  - `IGRP` Interior Gateway Routing Protocol



#### OSPF (Open Shortest Path First)

- *open* : publicly available

- use **link-state** algorithm
