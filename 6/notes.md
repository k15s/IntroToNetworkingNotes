#6-0 Routing
+ how do we get a packet from A to B across a network?
+ each router has a forwarding table to figure out where to send next packet to
+ how do these forwarding tables get populated?
    + distributed algorithm so routers can come to conclusion
    + routers build a spanning tree
    + root is destination, leaves are all the other sources
    + Bellman Ford and Dijkstra can build this tree
        + the latter is more widely used

#6-1 Flooding, source routing and spanning trees
+ how should the path from A to B be picked?
    + shortest? least congested? safest? does it matter?
+ approaches
    + flooding
        + simple -> one packet goes to every destination, therefore it must
          reach the destination
        + each router forwards the packet to every interface
        + extremely inefficient
            + packets can loop forever
            + packet crosses every link, sometimes multiple times
        + simple
            + no state needed
            + no forwarding table needed in routers
            + no knowledge of topology of network needed
    + source routing
        + source populates packet with sequence of hops it will visit in its
          path
            + source must know the topology
            + gives the final destination in the sequence
        + routers don't need forwarding tables -> all decision making is made by
          hosts
        + lot of work - packets can carry a lot of addresses
    + forwarding table
        + we already know the internet uses this
        + longest prefix matching combined with forwarding tables to determine
          where the packet will go next
        + we need a way to populate these forwarding tables though
    + spanning tree
        + reaches all leaves/destinations, no loops
        + used to create routing entires to populate forwarding tables
+ how do we calculate the best spanning tree?
+ metrics
    + minimize distance (geographic, link length, etc.)
    + minimize hop count
    + minimize delay
    + maximize throughput
    + most reliable
    + etc.
+ shortest path spanning trees
    + routers exchange information with each other to communicate topology
+ multipath
    + we've assume all packets to a certain destination follow the same path
        + downside to shortest path spanning tree - a path can become very
          congested
    + spread packets to destination over multiple paths
        + this can result in misordering of packet arrivals
+ multicast
    + send packets to a set of hosts, not just one (unicast)
    + A can send a single packet to one host
        + network replicates packet to another destination
        + replication so on and so forth to deliver to everyone

#6-2 Bellman Ford
+ How can routers work together to find the mininimum cost spanning tree?
    + ignore the end hosts, focus on the routers
+ Distributed Bellman-Ford Algorithm
    + assume routers know cost of link to each neighbor
    + router Ri maintains cost Ci to reach R8
        + maintain current lowest cost to reach R8 basically
    + C = [C1, C2,..., Ci-1] is distance vector
    + initially C is [infinity for all values] all think cost is infinite to
      reach target
        + Ri sends Ci to neighbors
        + if Ri finds a lower cost path, update its Ci
        + infinite repeat
    + steps...
        + start at the "target" node
        + iterate outwards in a BFS style - transfer cumulative link distance
          from/to target to next node
+ will the algorith always converge?
    + intuitively, yes, start with infinity
    + only ever replace cost values with lower costs
    + eventually we will find the lowest value, so it will always converge
+ what happens when links fail?
    + in general, continue to converage
    + if a lower cost path appears, algorithm will find it
    + Problem: link no longer exists
        + bad news travels slowly
        + counting to infinity problem, "loop" of values passing back and forth

#6-3 Dijkstra
+ link state protocol
    + exchange link state: router floods to every other router state of links
      connected to it
        + periodically and when link state changes
    + run Dijkstra
        + each router independently runs Dijkstra's algorithm
    + each router finds min-cost spanning tree
    + always run x iterations = x routers in network
+ what if link costs change or links fail?
    + re-calculate from scratch and move on

#6-4 Internet (RIP, OSPF) AS's
+ Hierarchy and Autonomous Systems
    + Hierarchy
        + we initially considered network to be a single collection of routers,
          but this is impractical
        + we have to decompose routing into smaller sets for scale
            + our algorithms would take forever without this
        + create separate autonomous systems
            + different routers
            + single exit point or multiple exit points
            + system is free to pick its own interior routing protocol
                + in one system, we could use OSPF to route packets
                + in another system, RIP or ISPF can be used
        + local customization creates competition for best service and various
          features amongst routers
    + Autonomous Systems
        + basic unit of hierarchy in the Internet
        + within in AS, owner decides how routing is done
        + between AS's, must use BGP-4 (Border Gateway Protocol)
            + consistent method for different autonomous systems to communicate
+ Interior Routing Protocols
    + within those autonomous systems, how do we use these algorithms?
    + RIP (routing information protocol)
        + Bellman-Ford algorithm
        + not as common anymore
    + OSPF
        + flooding is used when we don't have topology information
        + every router uses Dijkstra's
        + widely used
    + how do we exit the autonomous system?
    + Routing to a single exit point
        + default routing
        + each router knows all prefixes within autonomous system
        + packets for another AS are sent to default router
        + default router is border gateway to next AS
        + see address in packet that you don't know what to do with, forward to
          default router
            + routing tables in single exist autonomous systems tend to be small
            + only store prefixes for local system
    + Routing to multiple exit points
        + each router must be told which exit point to use for given destination
          prefix
            + sees prefix not in system, must know which exit point to use
        + large routing tables to store every prefix
            + hot potato routing - send to the closest exit, don't worry about
              best choice for packet (selfish by autonomous system)
                + widely used
            + pick exit closest to destination
+ Exterior Routing Protocol
    + every autonomous system must interconnect to other systems via BGP-4
        + similar to IP for the internet, thin waist for routing protocol is
          BGP-4
    + topology is extremely complicated with many different autonomous systems
    + each system is autonomously controlled, so link costs may be defined
      differently
    + systems may not trust each other
    + systems may have different priorities in their routing policies
+ The structure of the Internet
    + peer (horizontally) each other in same tier without charge
    + Internet Service Providers (ISPs) tier 1 defined as ISPs that fully
      connect with each other
        + providers are at the top
    + regional ISPs
        + below the providers, customer
    + access ISPs
        + customers to the regional ISPs

#6-5 BGP
+ border gateway protocol to connect autonomous systems together
+ "path vector" protocol
    + BGP routers at boundaries of each AS advertise complete path
        + list of AS's to reach a destination
        + "network prefix can be reached via the following AS's: "
    + local policies pick preferred path among options
    + when a link/router fails, path is withdrawn
+ customer and provider relationship
    + provider AS and customer AS
    + customer will always pay the provider to carry its packets
        + money flows up
    + IP traffic can flow up or down
        + in general, don't flow through intermediate AS in same level
    + peering relationship
        + in general, a peer doesn't provide transit between other peers
        + horizontal path that between two peers that passes through a third
          peer not allowed
+ BGP messages
    + open: establish BGP sesion
    + keep alive: handshake at regular intervals
        + occasional message to check that BGP session is still alive between
          two routers
    + notification: shut down a peering session
    + update: announcing new routes or withdrawing previously announced routes
        + announcement: prefix + path attributes
        + path attributes
            + next hop, autonomous system, local preference, etc.
            + used to select among multiple options for paths
+ Route selection
    + highest local preference
        + customer > peer > provider
        + ideally, send downwards through customer AS's
    + if no local preference defined, shortest ASPATH
        + ASPATH
            + shortest autonomous system path to destination
        +other ways of choosing also exist for traffic engineering
        + lightly used paths, etc.
    + no other way to choose? lowest router
+ Summary
    + all AS's connect using BGP-4
    + path vector algorithm: list of AS's sent along with each advertised prefix
        + router can examine path to choose whether or not it wants to use it
        + as packets sent, chain visited AS's on to it

#6-6 Multicast
+ so far we've assumed packets were unicast -> single destination
+ sometimes we want packets duplicated to a set of hosts
+ Flooding
    + easily deliver packets to a large number of hosts
    + packets replicated at every router, sent out through every interface
      except where it came from
+ Reverse Path Broadcast (RPB)
    + widely used
    + builds on observation that before A has even started sending, network
      already built MST
    + broadcast
        + like flooding, but no looping packets lost forever
        + routers at each hop ask...
            + consider the interface the packet arrived from
            + if it were sending a unicast packet to origin, is this the
              interface it would send a packet out of if it were going to origin?
                + if so, send out of all the other ports
            + this effectively breaks looping packets
    + RPD + pruning
        + routers not directly connected to a host interested in receiving
        + send pruning message saying "don't give me any packets"
+ multiple trees
    + each router generates its own MST
+ In practice
    + used less than expected

#6-7 Spanning Tree
+ Spanning Tree protocol: used by ethernet switches
+ ethernet routes packest, forwarding from source to destination based on
  ethernet addresses
+ we know how addresses are learned, but how are loops prevented?
+ ethernet switches build a spanning tree over which packets are forwarded
    + single tree for entire network
    + not building a spanning tree per router like IP
+ Ethernet Switch forwarding
    + examine header of each arriving frame
    + if ethernet destination address in forwarding table, forward via correct
      port
    + if not, broadcast the frame to all ports (except the one where packet
      arrived in)
    + entries in table are learned by examining the Ethernet SA of arriving
      packets
        + learning could lead to loops due to flooding/broadcast
+ Preventing loops
    + topology of switches is a graph
    + spaning tree protocol: find subgraph that spans all vertices without loops
    + decide...
        + which switch is the root of the tree
        + which ports are allowed to forward packets along the tree
+ How it works
    + pick a single root and only forward packets on ports on the shortest
      hop-count to root
    + root port: port on switch closest to the root
    + designated port: port neighbors agree to use to reach the root
    + periodically all switches broadcast ID of sender, ID of root, distance
      from sender to root
        + BPDU
    + initially every switch claims to be root - distance 0
    + every switch broadcasts until it hears a better message
        + smaller ID
        + shorter distance
+ Ultimately
    + only root originates config messages (other retransmit them)
    + switch only forwards on ports

#6-8 IPv6
+ Goal of IP Addresses
    + stitch many different networks together
    + globally unique identifier
        + can only be mostly unique nowadays since only 2^32 bits
        + 255.255.255.255
        + 255 = 11111111 = 8 bits
        + NATS, etc.
+ new protocol started recently recognizing this shortage
+ address structure
    + 128 bits of address
        + 128 / 8 = 16, so 16 255's in a way
        + F in Hex = 15 = 1111
        + FF in Hex = 255
        + 16 255's is desired, so 16 FF's = 32 Hex digits
        + 32 Hex digits * 4 bits/hex digit = 128
    + separated into subnet and interface portions
    + write address in hexidecimal as 8 blocks in 16 bits
    + :: means chain of 0's (0000:)
    + FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF

#6-9 Routing
+ approaches to route packets
    + flooding: packet replicated at router to all interfaces except one it
      arrived on
        + used in times of uncertainty
    + source routing
        + source puts list of hops placed in header
        + end to end principle
        + rarely used due to security, exposing full topology, etc.
    + forwarding tables
        + all ethernet switches and internet routers use this
    + spanning tree
        + unicast routing algorithms build a minimum cost spanning tree (no
          loops, can reach all destinations)
        + populate forwarding tables
            + Bellman-Ford
            + Dijkstra's
+ Internet routing
    + Hierarchical routing used by internet
        + autonomous system chooses local routing protocol
    + BGP: used between AS's
    + multicast routing
    + Spanning Tree Protocol
        + ethernet networks, not necessarily internet protocol
        + set of switches, one at the root, create a spanning tree
