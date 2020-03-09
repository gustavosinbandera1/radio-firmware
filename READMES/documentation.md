<p align="center">
  <a href="https://locha.io/">
  <img height="200px" src="images/LogotipoTurpial-Color.20-09-19.svg">
  </a>
  <br/>
  <a href="https://travis-ci.com/btcven/radio-firmware">
    <img src="https://travis-ci.com/btcven/radio-firmware.svg?branch=master" title="Build Status">
  </a>
</p>

<p align="center">
  <a href="https://locha.io/">Project Website</a> |
  <a href="https://locha.io/donate">Donate</a> |
  <a href="https://github.com/sponsors/rdymac">Sponsor</a> |
  <a href="https://locha.io/buy">Buy</a>
</p>

<h1 align="center">Routing Protocol AODVv2</h1>

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Application of MANETs](#application-of-manets)
  - [**MANET** Routing Protocol Challenges](#manet-routing-protocol-challenges)
    - [Dynamic topology](#dynamic-topology)
    - [Resource constraints](#resource-constraints)
    - [Heterogeneity](#heterogeneity)
  - [AODVv2 Reactive Routing protocol](#aodvv2-reactive-routing-protocol)
  - [AODVv2 the evolution of AODV](#aodvv2-the-evolution-of-aodv)
  - [Degrading performance in AODVv2 to avoid routing loops](#degrading-performance-in-aodvv2-to-avoid-routing-loops)
  - [Route Tables](#route-tables)
    - [In AODVv2, the routing tables can be updated if:](#in-aodvv2-the-routing-tables-can-be-updated-if)
 


## Introduction
Ad hoc networking has gained popularity and is applied in a wide range of applications, such as public safety and emergency response networks. Mobile Ad-hoc Networks (MANETs) are self-conﬁguring networks that support broadband communication without relying on wired infrastructure. Routing protocols of ad-hoc networks are main factors determining performance and reliability of these networks. 
They specify the way of communication among diﬀerent nodes by ﬁnding appropriate paths on which data packets must be sent.

In this work, we focus on the evolution of the Ad-hoc On-demand Distance Vector(AODV) called AODVv2 routing protocol.
**AODV** is one of the four protocols standardised by the **IETF MANET** working group.  The protocol ﬁnds alternative routes on
demand whenever needed, meaning that it is intended to ﬁrst establish a route between a source node and a destination **(route discovery)**, and then maintain a route between the two nodes during topology changes **(route maintenance)**.

## Application of MANETs

The origins of MANETs are to be found in the US military and one envisioneduse is military. For example, in the battleﬁeld, different units could be able to communicate even if an existing infrastructure has been destroyed or is untrusted. Second, MANETs could be used in rescue and disaster relief efforts, for instancein remote areas with little or insufﬁcient communication possibilities.A third application area is in sensor networks. A network of autonomously co-operating sensors can perform tasks not previouslyp ossible in traditional networks. Typically, nodes are relatively small units placed in an environment to monitor some kind of phenomenon. An example could be vehicle-to-vehicle communica-tion. A sensor placed on a vehicle could detect road conditions and propagate this information to other vehicles on the road. A fourth area is temporary networks, for example deployed at conferences, meeting rooms, and airports. Wireless Internet connection at airports can be ex-pensive, and a group of people could share a connection with the use of a MANET. Finally a ﬁfth application area could be a wireless personal area network with watches, laptops, PDAs, cell phones, and wearable computing devices sharing and exchanging information and delivering added convenience for the owner. Some of the motivation of the different application areas can be summarized aseither total lack of an infrastructure, unwillingness to use any existing infrastruc-ture, or the desire to extend coverage of an existing infrastructure.


## **MANET** Routing Protocol Challenges
MANET routing protocols face some challenges to be able to work in the envisioned application areas as described [above](#Application-of-MANETs)

###  Dynamic topology
 Nodes can appear and disappear at random. Nodes can move continuously or be powered off when entering sleep mode. This means that,for example, the routing protocols cannot assume that once information is gained about the topology, it remains ﬁxed and must consider the cost of constantly updating routing information.

### Resource constraints
 Nodes can have limited resources with respect to computational power, e.g., RAM and CPU power available, power supply, and cost of communication. In addition, there are constraints with regard to bandwidth on the wireless link, the effects of multiple access, fading, noise, and interference conditions may severely limit the transmission rate or even prevent link establishment.

### Heterogeneity
 Nodes can have varying characteristics with respect to the resource constraints speciﬁed above and be more or less willing to participate in routing.



## AODVv2 Reactive Routing protocol

AODVv2 reactive protocol look for routes by flooding **on-demand** route requests through the network. This means that when a data packet aimed for some destinations is injected into the network, the protocol initiates the route discovery process to deliver the data packet to its destination.
The picture shows a network containing 4 nodes connected to each other in a linear topology running the AODVv2 routing algorithm; s is the source node,d is the destination node,l and m are the intermediate nodes. Representation of the AODVv2 control messages is also shown in the figure. The field ∗ depicts recipients of (re)broadcast control messages that are nodes in the transmission range of the sender node.




## AODVv2 the evolution of AODV

This section provides a brief overview of AODV v2 protocol, in this one each node maintains a routing table (RT) containing information about the routes to be followed when sending messages to the others nodes of the network. The collective information in the nodes **"Routig table"** is at best a partial representation of network connectivity as it was at some times in the past.

We report a scheme of the AODVv2 protocol with an injected packet having the source node s and destination node d. When s receives the data packet, it ﬁrst looks up an entry for d in its routing table. If there is no such entry, it broadcasts a rreq message through the network. Afterwards when an intermediate node receives the rreq, it ﬁrst checks whether or not the information in the message is new. If this is not the case, the receiving intermediate node discards the rreq and the processing stops. If the information is new, the receiving node updates its routing table based on the information in the rreq. Then, it checks if it has a route to the destination d. If this information is provided, intermediate node sends a rrep back to the source s. By this, AODVv2 establishes bidirectional routes between originator and destination. On the other hand, if the intermediate node does not have any route to d, it adds its own address to the rreq and rebroadcasts the message. When next intermediate node receives the rebroadcast rreq, it updates (if the message is new) the routing table entry associated with s and the corresponding intermediate sender node and repeats the same steps executed by the former intermediate node. Finally when the destination d receives the rreq, it updates its routing table for the source node s and all the intermediate nodes that have rebroadcast the rreq, and then sends a unicast rrep that follows the reverse path towards s. Each node receiving the rrep will update the routing table entry associated with d and intermediate nodes. Nodes also monitor the status of alternative active routes to diﬀerent destina-tions. Upon detecting the breakage of a link in an active route, an rerr message is broadcast to notify the other nodes about the link failure. The rerr message contains the information about those destinations that are no longer reachable toward the broken link. When a node receives an rerr from its neighbours, it invalidates the corresponding route entry for the unreachable destinations.



## Degrading performance in AODVv2 to avoid routing loops

Differents studies have proved the presence of loops in AODV protocol.

In this current version,if route discovery is initiated the intermediate nodes which have active routes
through the destination do not send the rrep to the originator, meaning that the destination of the rreq has sole responsibility for sending the rrep back to the originator. By this, they have solved the problem of having loops in the network, but the performance level has decreased.


## Route Tables

Every received route message contains a route and consequently is evaluated to check for any improvement.

```block
Note that an rreq message contains a route to its source while an rrep message contains a route to its destination. 

Therefore, as the routes are identiﬁed by their destinations, in the former case, the destination of the route 

is the originator of the message and in the latter, it is the destination of the message
``` 
Note that we say a router is better then others if it has either a greater sequence number than others or an equal sequence number. 
The routing table must be updated if one of the following conditions is realized.

### In AODVv2, the routing tables can be updated if:

- No route to the destination exists in the routing table: the route is added to the routing table.
- the incoming route is better than the existing one. Two cases can be distinguished:
  - 1. there is only one matching route with the same destination:
    - the route state of the existing route is invalid: the incoming route must replace the existing one;


- All the existing routes to the destination are unconﬁrmed, i.e., their next hops are unconﬁrmed: the route is added to the routing table.
- If AdvRte is more recent than all matching LocalRoutes. 
- If the sequence numbers are equal, Check that AdvRte is safe against routing loops
- compared to all matching LocalRoutes, 
  
```
If LoopFree(AdvRte, LocalRoute) 
    returns TRUE,
```
compare route costs:
- If AdvRte is better than all matching LocalRoutes, it MUST be used to update the Local Route Set because it oﬀers improvement.
- If AdvRte is not better (i.e. it is worse or equal) but LocalRoute is Invalid, AdvRte **SHOULD** be used to update the Local Route Set because it can safely repair the existing Invalid LocalRoute.




