#PROBLEM 1-2A  (1/1 points)
What are the names of the four layers of the Internet model?

+ Transport, link, application, and network

#PROBLEM 1-2B  (1/1 points)
Which of the following statements are true? Choose all correct answers.

+ A consequence of layering is that a layer at the source host communicates with
  its peer layer at the destination, without concerning itself with the
  implementation details of the layers above and below.

The network layer relies on the connection state maintained by the TCP layer
in order to keep track of who it is communicating with.

The 7-layer OSI model is superior to the Internet 4-layer model because it
allows a network to be more reliable.

+ The socket API provides computer applications with a reusable means to
  communicate with remote applications.

Typically, the network layer is implemented on modern computer systems by
copying the code from a reference implementation into the application source code.

#PROBLEM 1-2C  (1/1 points)
Which layer is responsible for delivering data across a single hop?

+ Link layer

#PROBLEM 1-3A  (1/1 points)
Which of the following statements are true about the IP service model?

The Internet protocol is an example of a transport layer, and provides 
reliable end to end communication.

+ The Internet protocol is an example of a network layer, and is required for
  all communications in the Internet.

IP ensures packets are delivered in order.

#PROBLEM 1-3B  (1/1 points)

Which of the following statements are true about the Internet Protocol?

+ IP datagrams contain a field so that if a packet is routed in a perpetual loop,
  it will eventually be dropped.

IP datagrams contain a hop-count field that is incremented each time a
datagram visits a router. This allows us to find out how many routers the
datagram visited along the path.

IP datagrams contain a time-to-live field which, at any point in the network,
carries a timestamp indicating how many seconds since the packet was first
created.

+ There are currently two main versions of the IP protocol used in the Internet:
  IP Version 4, and IP Version 6.

#PROBLEM 1-4D  (1/1 points)
The purpose of a default route in a routing table specifies where to forward
packets whose destination addresses match no other rules in the routing table.

+ True

#PROBLEM 1-6A  (1/1 points)
What are the advantages of the network layers abstraction?

+ Break a complex task of communication into smaller pieces.

+ Lower layers hide the implementation details from higher layers.

Upper layers can provide a service to the layers below.

+ Lower layers can change implementation without affecting upper layers as long
  as the interface between layers remains the same.

#PROBLEM 1-6B  (1/1 points)
Your web browser uses HTTP transported over TCP. The webserver has one network
layer IPv4 address and is connected to an Ethernet network. After learning about
layering, your friend decides to change your laptop to use an alternative
protocol at each of the layers. Which of the following changes will still allow
you to access a webpage on the server without any changes to the server?

+ Unplug your laptop and use 802.11n WiFi to access the network

Update the client to use IPv6 addressing

Change the transport to UDP

Use FTP instead of HTTP in the web browser

+ only the first is right. All changes are made to distinct layers. And yes,
  layering enables you to replace each protocol. But notice that in this case he
  is only changing the protocols at the source, not at the destination.
+ And for any protocol to work, both the source and the dest have to know about
  it. However he is able to change the link layer protocol because for the link
  layer the destination is just the next hop(or in this case your home router)!

#PROBLEM 1-6C  (1/1 points)
You update your link layer from 10Mb/s Ethernet to 10Gb/s Ethernet. Which of the
following changes also need to be made?

Update from IPv4 to IPv6

Upgrade from half-duplex TCP to full-duplex TCP

Update the UDP header to support 10Gb/s mode

+ None of the above

+ no changes needed! no protocol change!

#PROBLEM 1-7A  (1/1 points)
Suppose you send a web request (HTTP) over TCP over IP over Ethernet. The first
byte of the Ethernet payload is:

Part of an HTTP request

A TCP transport segment header

+ An IP packet header

An Ethernet frame header

#MULTIPLE CHOICE  (5/5 points)
+ 16-bit 53 as 0x3500:
    + little endian check: 3 5 0 0 -> 0 0 3 5 = 53, flip works so little endian
+ 16-bit 4116 as 0x1014:
    + little endian check: 1 0 1 4 -> 4 1 0 1 != 4116, flip doesn't work
    + big endian
+ 32-bit 5 as 0x00000005:
    + little endian check: 00 00 00 05 -> 05 00 00 00 != 5, flip doesn't work
    + big endian
+ 32-bit 83,886,080 as 0x00000005:
    + little endian check: 00 00 00 05 -> 05 00 00 00 = 83886080, flip works so
      little endian
+ 32-bit 305,414,945 as 0x21433412:
    + little endian check: 21 43 34 12 -> 12 23 43 21 = 304300833, flip works so
      little endian

#PROBLEM 1-8A  (1/1 points)
Network order is

+ Big endian

#PROBLEM 1-8B  (1/1 points)
Host order (the byte ordering used on a generic computer)

+ depends on the laptop

#PROBLEM 1-8D  (1/1 points)
The layout of a character string in memory is the same on big-endian and
little-endian architectures.

+ True

#MULTIPLE CHOICE  (5/5 points)
For each source, destination, and netmask, mark whether the destination is in
the same network as the source.

+ recall that 255 = 11111111
+ source: 128.34.1.15, destination: 128.35.1.15, netmask: 255.255.0.0
    + different networks, the first two octets don't match
+ source: 10.0.1.4, destination: 10.0.1.5, netmask: 255.255.255.0
    + same networks, first 3 octets match
+ source: 10.0.1.4, destination: 10.0.2.5, netmask: 255.255.255.0
    + different networks, the first two octets don't match
+ source: 171.64.15.33, destination: 171.64.15.5, netmask: 255.255.255.224
    + first 3 octets match
    + 224 = 11100000
    + 33 = 00100001
    + 5 = 00000101
    + 224 AND 33 != 224 AND 5
    + different networks
+ source: 171.64.15.33, destination: 171.19.201.2, netmask: 255.0.0.0
    + same network, first octet matches

#PROBLEM 1-9A  (1/1 points)
Suppose you have IPv4 address 192.13.128.0 and your subnet mask is 255.255.128.0.
Which of the following IPv4 addresses are in your network?

1. 192.13.128.0
2. 192.13.255.1
3. 192.13.255.64
4. 192.13.0.0
5. 192.13.64.2
6. 192.13.192.255

+ 255.255.128.0 = 11111111.11111111.10000000.00000000
+ so first 2 octets have to match perfectly
+ the first bit in the third octet has to = 1, since our original address AND
  mask in third octet = 10000000
+ 1, 2, 3, and 5

#PROBLEM 1-9B  (1/1 points)
You bring your laptop to Stanford. You connect to the network with a wire through
your Ethernet port and you connect to the network via WiFi. True or False:
Stanford will give the same IP address to your wireless card and your Ethernet
card.

+ False

#PROBLEM 1-9C  (1/1 points)
Suppose that an ISP owns a subnet 18.24.64.0/18. If the ISP wants to create 4
disjoint, equally-sized subnetworks from this, what will the CIDR addresses of
these subnets be?

1. 18.24.80.0/4, 18.24.112.0/4, 18.24.96.0/4, and 18.24.64.0/4.
2. 18.24.64.0/4, 18.24.64.0/8, 18.24.64.0/12, and 18.24.64.0/16.
3. 18.24.80.0/16, 18.24.112.0/16, 18.24.96.0/16, and 18.24.64.0/16.
4. 18.24.80.0/20, 18.24.112.0/20, 18.24.96.0/20, and 18.24.64.0/20

+ Netmasks are always easier to understand if you convert everything to binary.

+ 18.24.64.0/18 -> this means that you own all ip addresses that start with the
  first 18 bits of 18.24.64.0 or 00010010 00011000 01000000 00000000.

+ Now you can divided these into four equal-sized subnets with 20-bit netmasks
  by permuting bits 19 and 20.

1) 00010010 00011000 01000000 00000000 -> 18.24.64.0/20
2) 00010010 00011000 01010000 00000000 -> 18.24.80.0/20
3) 00010010 00011000 01100000 00000000 -> 18.24.96.0/20
4) 00010010 00011000 01110000 00000000 -> 18.24.112.0/20

+ Only 4

#TEXT INPUT  (5/5 points)
With this forwarding table, over which link will a router using longest prefix
match send packets with the following IP destination address?

dest: 0.0.0.0/0, link: 1

dest: 18.0.0.0/8, link: 5

dest: 171.0.0.0/8, link 2

dest: 171.0.0.0/10, link: 4

dest: 171.0.15.0/24, link: 1

dest: 55.128.0.0/10, link: 6

dest: 63.19.5.0/30, link: 3

+ Packets to 63.19.5.3 will be sent on link
    + looks like the last one, \30 means first 30 bits have to match
    + they do, since the 3 only takes up the very last 2 bits in the final octet
    + 3
+ Packets to 171.15.15.0 will be sent on link
    + between 2 and 4, both pass
    + 4 requires the first 10 bits to match, which is more than the first 8, so
      it's more specific
    + 4
+ Packets to 63.19.5.32 will be sent on link
    + 32 = 00100000
    + looks like the last one is promising, first 30 bits have to match
    + first 3 octets are perfect, so first 6 bits of final octet have to = 0
    + there is a 1 in 32, nothing else matches, so default
    + 1
+ Packets to 44.199.230.1 will be sent on link
    + no matches...
    + 1
+ Packets to 171.128.16.0 will be sent on link
    + only the 3rd destination/link 2 matches
    + first 8 bits, so only the first octet
    + the 4th dest doesn't, since it needs the first 10 bits to match, and
      128 = 10000000 has a 1 in the MSB, != 0, no match
    + 2
