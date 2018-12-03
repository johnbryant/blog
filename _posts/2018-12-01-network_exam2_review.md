---
layout: post
title: "Computer Network Exam2 Review outline"
date: 2018-12-01
description: "Chapter 4,5,8,9"

tags: network



---

## network layer

- primary role: to move packets from a send host to a receiving host

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

  - ![traditional_control_plane](/blog/images/posts/2018-12-01-network_exam2_review/traditional_control_plane.png)

  - traditional routing algorighms - implemented in routers

  - ![SDN](/blog/images/posts/2018-12-01-network_exam2_review/SDN.png)

  - SDN - implemented in (remote) servers



### What's inside a Router

#### input port processing

![input_port_porcessing](/blog/images/posts/2018-12-01-network_exam2_review/input_port_porcessing.png)

- destination-based forwarding

- forwarding table

![forwarding_table1](/blog/images/posts/2018-12-01-network_exam2_review/forwarding_table1.png)

![forwarding_table2](/blog/images/posts/2018-12-01-network_exam2_review/forwarding_table2.png)

- the `destination address range` can be write as `prefix`

![forwarding_table3](/blog/images/posts/2018-12-01-network_exam2_review/forwarding_table3.png)

- When an input packet arrives, it need to be matched the address in forwaring table. If there are **multiple matches**, the router uses the **longest prefix matching rule**:

  -  it finds the longest matching entry in the table and forwards the packet to the link interface associated
    with the longest prefix match

#### Switching

- Through the switching fabric, the packets are switched from an input port to an output port

- switching can be accomplished by several ways:

  - via memory

    - simplest

    - uder direct control of CPU

  - via bus

    - use shared bus

    - routing processor not used

    - protential blocking

  - via corssbar (interconnection)

    - higher BW possible (no blocking)

    - more expensive

#### Output port processing

![output_port_processing](/blog/images/posts/2018-12-01-network_exam2_review/output_port_processing.png)





### CIDR

- Classless Interdomain Routing

- used to generalize the notion of subnet addressing (like the scope of the IP address in a organization)

- 32-bit IP address is divided into two parts and has the form `a.b.c.d/x`, where x indicates the number of bits in the first part of the address

- Example: the ISP may itself have benn allocated the address block `200.23.16.0/20`, then it divides its address block into eight equal-sized contiguous address blocks and give one of these address blocks out to each of up to eight organizations that are supported by this ISP, as shown below:

  ```
  ISP’s block:     200.23.16.0/20     11001000 00010111 00010000 00000000
   
  Organization 0   200.23.16.0/23     11001000 00010111 00010000 00000000
  Organization 1   200.23.18.0/23     11001000 00010111 00010010 00000000
  Organization 2   200.23.20.0/23     11001000 00010111 00010100 00000000 ... ...              ...
  Organization 7   200.23.30.0/23     11001000 00010111 00011110 00000000
  ```

  

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

- from A (223.1.1.1) to C (223.1.2.2): send from A via 223.1.1.4 (gateway) and 223.1.2.9 (gateway) to B

### IP Fragmentation & reassembly

- reason: link layer have MTU (max transfer size), different link types, different MTUs

- large IP datagram divided into small

- reassembleed only at final destination

- e.g: datasize is 4000 ( data 3980 + header 20), MTU is 1500. we can divide it to 3 part

  - length = 1500 (data 1480 + header 20), offset = 0, fragflag = 1

  - length = 1500 (data 1480 + header 20), offset = 1480, fragflag = 1

  - length = 1040 (data 1020 + header 20), offset = 2960, fragflag = 0



### IPv6

![IPv6_data_format](/blog/images/posts/2018-12-01-network_exam2_review/IPv6_data_format.png)

- have this new:

  - Expanded addresseing capability: from 32 to 128 bits

  - A streamlined 40-byte header

  - Flow labeling

- no longer have:

  - Fragmentation / reassembly

  - Header checksum

  - Options

- Transitioning from IPv4 to IPv6

  - use **tunneling**: Suppose two IPv6 nodes (B and E) want to interoperate using IPv6 datagrams but are connected to each other by intervening IPv4 routers. We refer to the intervening set of IPv4 routers between two IPv6 routers as a tunnel. Here is the prccedure:

  ![tunneling](/blog/images/posts/2018-12-01-network_exam2_review/tunneling.png)



### OpenFlow

- match-action table

- Match

  - for this exam, we mainly use `Ingress port = 1`, `IP Src = 10.1.*.*`, `IP Dst = 10.2.*.*`

![open_flow_match_table](/blog/images/posts/2018-12-01-network_exam2_review/open_flow_match_table.png)

- Action

  - **Forwarding**: to output port

  - Dropping

  - Modify-field



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

### Scalable routing

- aggregate routers into regions known as `autonomous system` (AS)

- why AS: scale and administrative autonomy

- intra-AS routing

  - routing among hosts, routers in same AS

  - all routers in AS must run **same** intra-domain routing protocol

  - routers in different AS can run different intra-domain routing protocol

  - gateway router: at *edge* of its own AS, has link to routers in other AS

- inter-AS routing

  - routing among AS

  - gateways perform inter-domain routing

#### Inter-AS task

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

- router floods OSPF  link-state advertisement to all other routers in entire AS

  - carried in OSPF message directly over IP (rather than TCP or UDP)

  - link state: for each attached link

- IS-IS routing protocol is nearly identical to OSPF

- pros

  - **security**: exchanges between OSPF routers are authenticated

  - **multiple same-cost paths** allowed

  - integrated support for unicast and **multi-cast** routing

    - multi-cast OSPF use same topology data base as OSPF

  - **hierarchical** OSPF in large domins

#### Hierarchical OSPF

![hierarchical_OSPF](/blog/images/posts/2018-12-01-network_exam2_review/hierarchical_OSPF.png)

- two-level hierarchy: local area, backbone

  - link-state advertisements only in area

  - each nodes has detailed area topology; only know the direction (shorest path) to other areas

- area border router: "summarize" distance to nets in own area, advertise to other area border router

- backbone router: run OSPF routing limited to backbone

- boundary router: connect to other AS



### inter-AS routing

- BGP (Border Gateway Protocol)

- BGP provides each router:

  - obtain prefix reachability information from neighboring ASs (eg. AS2, AS3, X; AS3, X)

  - determine the "best" routes to the prefixes.

- BGP provides each AS a means to:

  - **eBGP (external BGP)**: obtain subnet reachability information from neighboring ASes

  - **iBGP (internal BGP)**: propagate reachability information to all AS-internal routers

  - determine "good" routes to other networks based on reachability information and policy

- allow subnet to advertise its existence to rest of Internet

- BGP connection attributes:

  - AS-PATH

  - NEXT-HOP

![hot_potato_routing](/blog/images/posts/2018-12-01-network_exam2_review/hot_potato_routing.png)

- practice use: **Hot Potato routing**

#### BGP route selection priority (from 1 to 4)

1. local preference value attribute: **policy** decision

2. shortest **AS-PATH**

3. closest **NEXT-HOP** router: hot potato routing

4. additional criteria

#### Different: intra-, inter-AS routing

- policy

  - inter-AS: admin wants control over how its traffic routed, who routes through its net

  - intra-AS: single admin, no policy need

- scale

  - hierarchical routing saves table size, reduced update traffic

- performance

  - intra-AS: can focus on performance

  - inter-AS: policy may dominate over performance



### SDN

- software defined networking

- 



### ICMP

- Internet Control Message protocol

- used by hosts, routers, gageways to communication network-level information

  - error reporting

  - echo request/reply (used by ping)

- ICMP msgs are carried in IP datagram

- Field

  - type

  - code

  - also contain the header the first 8 bytes of the IP datagram that caused the ICMP message to be generated



### SNMP

- Simple Network Management Protocol

- is an application-layer protocol used to convey network-management control and information messasge between a managing server and an agent executing on behalf of that managing server

- usage

  - In a request-response mode, an SNMP managing server sends a requeset to an SNMP agent, who receives the request, performs some action, and sends a reply to the request



### Multicast

- In 1995, the first multicast network: MBone

- DVMRP (Distance Vector Multicast Routing Protocol) can't scale to Internet size

- In 1997, PIM (Protocol Independent Multicast)

- Unicast

  - one sender and one receiver

  - concern about where the packet is going

- Multicast

  - one or more senders to a group of receivers

  - concern about where the packet came from

  - Multicast Routing uses *Reverse Path Forwarding*

  - application: Interactive gaming; shared data apps
