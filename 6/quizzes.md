#PROBLEM 6-1A  (1.0/1.0 points)
Which of the following statements are true? Choose all correct answers.

Flooding is a very inefficient way to route packets and therefore it is never
used.

Flooding is an efficient way to route packets if the network topology does not
have a loop, as packets will never loop the network.

+ If flooding is used in an IP network, loops can be avoided by decrementing the
  time-to-live (TTL) field in the IP header and dropping packets whose TTL is 0.

+ In a connected network (ie. if there is a path between every pair of end
  hosts), flooding ensures every destination sees every packet.

+ EXPLANATION
+ A is not true as flooding is useful when the router has no routing entries.
+ B is not true, as bandwidth is still wasted sending packets to every host
  apart from the destination.

#PROBLEM 6-1B  (3.0/3.0 points)
Which of the following statements are true about source routing?

+ End hosts must know the topology in advance.

The routers must keep track of the shortest path to reach the source.

+ If a network link fails, then every end host must be updated with the new
  topology.

+ Source routing gives the end host control over the path taken by a packet to
  its destination.

#PROBLEM 6-2A  (1/1 point)
Which of the following statements are true?

+ If the longest loop free path in the network is N, then the Bellman Ford
  routing algorithm will converge to the lowest cost spanning tree after N
  iterations.

As an optimization in Bellman Ford, a router need not send any advertisements to
its neighbours after the distance drops from infinity to a finite value.

Bellman Ford algorithm converges in finite time if all link costs are negative.

+ EXPLANATION
+ b is not true. Say the cost of reaching a router B from A drops from infinity
  to 100. There could still be a lower cost path (e.g. of cost 10) to reach B
  from A through other hops.
+ c Traversing a negative weight loop multiple times will always result in a
  lower cost, and thus the Bellman Ford will never converge. Hints: If link
  costs are negative, what happens if there is a loop in the topology?

#PROBLEM 6-2B  (1/1 point)
Which of the following statements are true for the Bellman Ford algorithm if we
set “infinity” to be equal to 16.

+ The algorithm is guaranteed to terminate whenever the network changes.

+ When the algorithm terminates, routers that are unreachable will have distance
  infinity

If the longest loop-free path in the network is 25, the algorithm is guaranteed
to converge to the correct solution.

+ EXPLANATION
+ Answer: a, b. Because infinity is set to 16 the algorithm must terminate and
  routers that are unreachable will have a distance of infinity. If the longest
  path in the the network is less than infinity (length of 15 or less), then the
  algorithm will converge to the correct solution. However, because we are told
  the longest loop free path is greater than 16, the algorithm may mislabel some
  routers as unreachable.

#PROBLEM 6-3A  (3/3 points)
Which of the following statements are true about a link state routing protocol
(e.g. Dijkstra’s algorithm)?

Every router floods the state of the entire network topology to every other
router.

+ Every router floods the state of its links to every other router.

+ Each router computes a spanning tree to reach every other router in the
  network.

Each router takes constant time to compute the min-cost spanning tree,
regardless of the number of routers in the network.

+ Unlike the Bellman Ford algorithm, Dijkstra’s algorithm does not suffer from
  the “count to infinity” problem.

+ EXPLANATION
+ Ans: b,c,e
+ In a link state routing protocol, each router floods the network with the
  state of its own links, not its entire view of the network topology. The time
  to compute the min-cost spanning tree depends on the network.

#PROBLEM 6-4A  (3/3 points)
Which of the following is true about autonomous systems (AS)?

+ An AS is a group of routers that belong to a single administrative domain.
+ AS’s can have any number of exit points that connect it to other AS’s.
AS identifiers are typically shared by multiple enterprises.
+ As of today, AS’s that wish to be part of the Internet must use BGP-4 to
  exchange routing information with each other.

+ EXPLANATION
+ An AS is a group of nodes assigned a unique identifier (AS number), that can
  have multiple exit points, where BGP must be used to exchange routing
  information.
+ Note: Many large enterprises have several AS’s, each has a different AS
  number.

#PROBLEM 6-4B  (1/1 point)
Which of the following statements are true?

+ Large Tier 1 ISPs provide connectivity between each other in a settlement
  free fashion.

+ Regional ISPs often peer with each other to avoid sending traffic to their
  providers, to reduce cost.

Access ISPs always directly connect to Tier 1 ISPs for optimum performance.

Regional ISPs are customers of Access ISPs.

+ EXPLANATION
+ ISPs will peer with one another in a settlement free fashion to reduce costs.
  Tier 1 ISPs must peer with one another to be able to reach the entire
  Internet, and by definition, they can do so by settlement free peering. ISPs
  will pay for traffic they send to their providers, and the general hierarchy
  is: Access ISPs pay Regional ISPs pay Tier 1 ISPs.

#PROBLEM 6-5A  (1/1 point)
Which of the following are true?

+ BGP is an example of a path vector protocol where routers advertise not only
  the next hop to a destination prefix, but also an AS path to it.

+ BGP is used to route traffic between AS’s.

+ In general, a peering relationship does not involve a financial settlement.
  Therefore, peers do not act as a transit for other peers.

If peers don’t act as a transit to other peers, all packets in an AS should either originate from within the AS or be destined to it.

+ EXPLANATION
+ D is not true because an AS X can act as a transit for a packet from its
  customer, to X’s provider.

#PROBLEM 6-5B  (1/1 point)
Which of the following statements are true?

To route a packet to a prefix P, a BGP router always chooses the shortest ASpath
advertised by one of its peers.

All BGP messages are ASpath advertisements.

BGP floods link state to every other BGP router.

+ A BGP router can locally and independently pick among advertised ASpaths.

+ EXPLANATION
+ A BGP router can locally and independently pick among advertisements and
  select which routes it advertises to each of other routers that it is
  connected to.

#PROBLEM 6-6A  (1/1 point)
Which of the following statements are true about multicast?

Hint: In both flooding and multicast, the sender sends one packet that is
delivered to all receivers.

In general, sending a multicast packet to a multicast group requires the sender
to transmit fewer packets to reach all end hosts than flooding.

+ In general, sending a multicast packet to a multicast group is most efficient
because it saves the source from sending several unicast packets.

+ Multicast routing requires support from the network to duplicate packets.

The network can support only one multicast session at a time.

+ EXPLANATION
+ Multicast routing allows a host to send one packet that get replicated to the
  whole group, but to do so it requires support from the network for replication.
  Flooding also involves replication, which is why a is false.

#PROBLEM 6-6B  (1/1 point)
Using the “Reverse path broadcast + pruning” technique, a router that receives a
packet destined to multicast group G can send a prune message to its neighbour
if no *host* directly attached to the router is part of G. True/False?

Hint: Can you construct a network where the router that sends the prune message
to its neighbour is the only router on the path to other nodes in G?

+ False
+ EXPLANATION
+ A router that is not connected to any of the hosts in G may be on the shortest
  path to G. If this were the case, pruning the tree may result in a
  disconnected graph.

  For example, consider this network:

  Sender --- R1 --- R2 --- ... --- Rn
  (R1, R2, ..., Rn are the routers, and Rx1, Rx2, ..., etc. are receivers
  attached to R1, R2, etc. respectively.)

  The network itself is the spanning tree. Suppose you want to broadcast from
  Sender to every receiver except Rx2. If R2 sends a prune to R1, R1 cannot just
  remove R2 and stop sending packets to R2.

#PROBLEM 6-7A  (2/2 points)
Which of the following statements are true?

+ If an Ethernet switch receives a packet with destination address that is not
  in its table, it floods the packet to all active ports except the one through
  which it arrived.

Ethernet switches use the destination IP addresses to forward packets.

Ethernet switches forward packets over a shortest path spanning tree rooted at
the destination.

+ Entries in the Ethernet forwarding table are specific to a destination MAC
  address.

As a safeguard against loops, Ethernet switches use a TTL in the packet header.

+ EXPLANATION
+ Ethernet switches build one spanning tree over which packets are forwarded.
  Ethernet frames do not have any TTL field, so it’s very important to prevent
  loops! That’s what the Spanning Tree Protocol does.

#PROBLEM 6-7C  (1/1 point)
Which of the following statements are true about the spanning tree protocol?
Hints: After the protocol converges the switch with the lowest ID will be root.

+ Periodically, all switches broadcast a message containing an ID, the ID of the
  root, and the distance of the switch from the root. This message is called the
  Bridge Protocol Data Unit (or BPDU).

After the spanning tree protocol converges, the switch with the highest ID will
be the root.

Packet forwarding (except for BPDU messages) only happens on designated ports.

+ Spanning tree protocol is inefficient as many network links will not be used
  to forward packets.

+ EXPLANATION
+ Third choice is not correct since it says packet forwarding happens except
  BPDU packets.
+ BPDUs are always sent whether it is Designated or Root port or even on Blocked
  ports

#PROBLEM 6-8A  (1/1 point)
Select the best answer. The number of IPv6 addresses is _____ larger than the
number of IPv4 addresses.

2 times
16 times
96 times
124 times
+ more than 2e^95 times
+ EXPLANATION
+ 2e^128 / 2e^32 = 2e^96 times larger

#PROBLEM 6-8B  (1/1 point)
Which of these are valid IPv6 addresses? Choose all correct answers.

+ 1803:4455:6677:8899:0011:2233:4455:6677
AABB:CCDD:EEFF:GGHH:IIJJ:KKLL:MMNN:OOPP
+ AABB::
127.0.0.1
+ 1122:3344:5566:7788:9900:AABB:CCDD:EEFF
+ 1233::FFFF
+ ::FFFF

+ EXPLANATION
+ Addresses are written in hexadecimal as 8 blocks of 16 bits. (B) is incorrect
  because G, H, I, J, K,... are not valid hexadecimal characters. (D) is
  incorrect because it is missing 96 bits (it’s an IPv4 address). (C) ,(F) and
  (G) are OK because IPv6 allows for a single run of zeros to be elided by ::
