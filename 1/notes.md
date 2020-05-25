# 1-1
+ reliable bidirectional byte stream, A -> B and B -> A
+ a NAT client makes it harder to connect to machine hiding behind it

# 1-2 4 layer model
+ Model
    + Application
        + bidirectional byte stream
        + semantics and data flowing between two points
        + GET URL request via http protocol
        + application doesn't care how the data got there, hands GET request to
          TCP layer, which makes sure data gets there reliably, which relies on
          network layer to send stuff, etc...
    + Transport
        + TCP/IP
        + Transmission control protocol
        + make sure data is delivered in right order end to end
        + TCP transmits datagrams multiple times if network drops some by
          accident
        + TCP can put data back into correct order if it's out of order
        + UDP (user datagram protocol) is also an option, no guarantees...
    + Network
        + for us, the most important
        + deliver packets end to end, source -> destination
        + packet is collection of data with header 
        + packets, link layer provides service to network
        + IP (internet protocol) like postal service...
            + not reliable, but makes best effort
            + no order in data or flow state, no guarantees
    + Link
        + links between routers that carries data
        + ethernet, wifi, etc.
+ layers communicate with corresponding layer on other end
+ IP is the thin waist

# 1-3 IP service
+ IP datagrams have header and data
+ Transport gives its segment to network layer, which places this into its IP
  data section with a separate IP header
+ IP sends this datagram to link layer, which is also re-packged internally then
  sent
+ simple so its faster and streamlined, other pieces can be built on top, works
  over any link layer
+ try to prevent packets from looping forever
+ fragment packets if theyre too long
+ header checksum for destination accuracy
+ Datagram
    + data
    + header
        + destination IP address
        + source IP address
        + protocol ID: allow destination to unzip packages and figure out how to
          parse
        + IP version
        + total packet length
        + time to live: prevents the looping by router decrementing
        + packet ID, flags, fragment offset: helps to split datagram into pieces

# 1-4 Day in packet life
+ TCP has client and server (listens)
+ 3-way handshake
    + client sends synchronized message (syn)
    + server responds (synack)
    + client responds (ack)
+ IP address is used by network layer for packet delivery
+ TCP port tells software which app to deliver to
+ each router has a forwarding table to figure out where to send packet to

# 1-5 Packet Switching
+ packet switching: independently, for each new packet, pick best outgoing link
    + if free send, otherweise hold
+ packet is a unit of self-contained data with info to get to its location
+ each router switch has a small state for appropriate next hop
    + all packet has to do is carry destination then!
+ switch makes independent simple decisions, no state cares or flow state
    + flow: collection of datagrams part of the same communication (like TCP)
+ statistical multiplexing
    + web traffic is burst, just make independent decisions based on who to send
      to
    + share single resource, without keeping wasteful link open

# 1-6 Layering
+ split system up into hierarchical layers
+ sequential communication, only talk above and below
    + generally the lower layer provides service for upper layer
+ separation of concerns, modularity is good!
+ analogy: the postal service! abstract services separately from each other,
  everyone does their own job

#1-7 Encapsulation
+ how to organize information in packets so that layering is possible
    + browser generates GET request
    + TCP encapsulates GET request
    + IP encapsulates TCP segment to make packet
+ layering protocols is now possible
    + Virtual private network (VPN) just adds single layer to keep things simple

# 1-8 Byte order and packet formats
+ big endian
    + standard format, MSB comes first, LSB at the end (far left)
    + just like a regular binary conversion
+ little endian
    + LSB comes first, MSB on the far right
    + we have to slide the number around before we do our standard hex
      conversion
    + 0x00000005 in 32 bits
        + 32 bits so split up by 2's then flip, so 00 00 00 05 -> 05 00 00 00
        + 5 * 16^6
    + 0x3500 in 16 bits
        + 16 bits so split up by 1's (one hex slot handles 16) then flip, so
          3 5 0 0 -> 0 0 3 5
        + 5 + 3 * 16
+ computers need to agree how to represent numbers, and different processors
  vary the representation
+ big endian for internet protocols and packets
+ what if your computer is little endian?
    + plenty of C networking library functions that can re-arrange these things
+ BE CAREFUL
    + when you handle network data you have to make sure you're careful with
      representations
    + pick strict approach for conversions

# 1-9 IPV4 Addresses and CIDR
+ routers decide which link to send a packet to next based on the destination
  address
+ IPV4 address id's device on internet
    + 32 bits long (4 octets)
    + a.b.c.d
    + netmask allows you to compare 2 IP addresses and check if they're in the
      same network
        + 255 decimal = 11111111 (8 bits)
        + AND IP with netmask, compare with other IP AND'd with netmask to check
          if on same network
+ CIDR (classless inter-domain routing)
    + \CIDR designates number of bits for same network
    + these bits are expressed via the IP address, similar to what we did with
      netmasks
    + \24 = 2^(32 - 24) hosts = 2^8 hosts = 256 addresses/hosts
    + \20 = 2^(32 - 20) hosts = 2^12 hosts = 4096 addresses/hosts
    + this makes sense - CIDR declares the range, we're calculating all possible
      permutations, either 1 or 0 so 2^pow

#1-10 Longest Prefix Match
+ routers can have many links/options, use LPM to figure out which link to route
  packet to
+ forwarding table has set of partial IP addresses (Wild cards in IP address)
+ 171.33.0.0/16
    + first 16 bits/2 octets of packet destination address matches 171.33 the
      router will send down this link
    + 16 bit prefix > 0, so this one is more specific than the default...
+ basically, the CIDR prefix determines how many bits from L -> R of the prefix
  must match. All bits after are wildcards that can take anything.

#1-11 Address Resolution Protocol (ARP)
+ how a device knows which link address to send to based on address
+ encapsulation
    + network layer destination for ultimate endpoint
    + link layer destination for middleman gateway
+ nodes cache links and corresponding addresses
+ if a node doesn't have the needed address, it can ping other nodes in a
  request for the right address via ARP
    + simple request reply protocol
    + who has network address I need?
    + gets response "I have network address"
