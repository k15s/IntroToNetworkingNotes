#PROBLEM 5-1A  (1/1 points)
Assume that all of the 32 bit IPv4 address space is available for public address
allocations, except for the 10.0.0.0/8 range which is reserved for private
addresses behind a NAT. (There are no other reserved, multicast, or special use
addresses.) With only one NAT, what is the maximum number of total hosts we can
have in the whole network (excluding the NAT)?

Hints: Think about how many addresses there are in the IPv4 address space
without 10.0.0.0/8. Don't forget to remove the public address for the NAT.

+ 4294967295
+ the whole network refers to an address space including both public and
  private addresses
+ total number of IP addresses possible: 4 octets, 8 bits in an octet, so 2^32
+ the number of IP addresses reserved for private block = number of IP addresses
  that can reside behind the NAT, so no change here
+ subtract the single, public address for the NAT
+ 2^32 - 1 = 4294967295

#PROBLEM 5-1B  (1/1 points)
(Same constraints as above)

We add a second NAT to the same network. What is the maximum number of total
hosts that we can have the network (excluding the NATs)? Hints: Don't forget to
remove the NATs from your count.

+ 4311744510
+ the second NAT
    + 10.0.0.0/8 means 10.x.x.x is the range of private addresses
    + this is 2^24 possibilities
+ 2^32 - 2 NAT addresses + 2^24 extra private addresses from the second NAT

#PROBLEM 5-1C  (1/1 points)
An external host wants to initiate a TCP connection to a node listening on port
5577 behind a NAT. What NAT port should the external host attempt to connect to?

It should send it to 5577, the NAT will always automatically maintain the map
between its receiving port and the internal node.
It can send it to any port on the NAT, but the first 16 bits of the TCP packet's
data field should contain 5577. If the internal host is behind a NAT, it always
reads these bits to determine which internal port to demultiplex a packet to.
+ In the general case, the external host cannot guarantee that the packet will
  be routed to port 5577 of the internal node.

+ Answer: c. In general, mappings are made when an internal host initiates a
  connection to an external host. Therefore, there will be no mapping to
  internal host port 5577.

#PROBLEM 5-1D  (1/1 points)
(True or false) A NAT could be configured to share 1 public IP address among
2^32 hosts (where each host is assigned 1 private IP). Note: Assume that we are
using IPv4, which supports 32 bit addresses.

+ False
+ There must be some part of the address range allocated as public and some part
  private.
+ The NAT would still only be able to support 2^16 TCP connection

#PROBLEM 5-2A  (1/1 points)
You have a host behind a NAT that is exchanging UDP traffic with another host
outside the NAT. This other host has a publicly routable IP address and is not
behind a NAT. The other host has IP address 171.2.1.6 and is using port 9999.
Your local UDP port number is 5555, which the NAT has mapped to 7777. There are
no other NAT mappings.

Your NAT receives a UDP datagram on its external interface. The datagram is from
182.11.5.9, port 7878 to the NAT's IP address, port 5555. Which of the following
NAT types will properly forward the datagram to your host?

Symmetric
Full cone
Restricted cone
Port restricted
+ None of the above

+ The external port for your UDP exchange is 7777, so the NAT does not have a
  mapping for the 5555 destination port

#PROBLEM 5-2B  (1/1 points)
(same setup as before) You have a host behind a NAT that is exchanging UDP
traffic with another host outside the NAT. This other host has a publicly
routable IP address and is not behind a NAT. The other host has IP address
171.2.1.6 and is using port 9999. Your local UDP port number is 5555, which the
NAT has mapped to 7777. There are no other NAT mappings.

Your NAT receives a UDP datagram on its external interface. The datagram is from
182.11.5.9, port 9999 to the NAT's IP address, port 7777. Which of the following
NAT types will properly forward the datagram to your host?

Symmetric
+ Full cone
Restricted cone
Port restricted
None of the above

+ Answer: full cone. The packet is coming from a different IP address, so
  neither restricted cone nor port restricted will translate the packet. Nor
  will the symmetric NAT. A full cone NAT, however, will map solely on the
  destination address and port.

#PROBLEM 5-2C  (1/1 points)
(same setup as before) You have a host behind a NAT that is exchanging UDP
traffic with another host outside the NAT. This other host has a publicly
routable IP address and is not behind a NAT. The other host has IP address
171.2.1.6 and is using port 9999. Your local UDP port number is 5555, which the
NAT has mapped to 7777. There are no other NAT mappings.

Your NAT receives a UDP datagram on its external interface. The datagram is from
171.2.1.6, port 3535 to the NAT's IP address, port 7777. Which of the following
NAT types will properly forward the datagram to your host?

Symmetric
+ Full cone
+ Restricted cone
Port restricted
None of the above

+ Answer: b and c. A port restricted NAT will not translate the packet because
  the source port 3535 does not match 9999. A restricted code will, however,
  because the source address matches.

#PROBLEM 5-2D  (1/1 points)
(same setup as before) You have a host behind a NAT that is exchanging UDP
traffic with another host outside the NAT. This other host has a publicly
routable IP address and is not behind a NAT. The other host has IP address
171.2.1.6 and is using port 9999. Your local UDP port number is 5555, which the
NAT has mapped to 7777. There are no other NAT mappings.

Your host sends a datagram from local port 5555 to 171.55.1.2 port 9999. What
NAT types will allocate a new external port for these datagrams?

+ Symmetric
Full cone
Restricted cone
Port restricted
None of the above

+ Answer: symmetric. A symmetric NAT creates a unique source IP, source port
  pair for each destination IP, destination port pair. All other types will
  reuse the port and potentially create a new mapping for it.

#PROBLEM 5-5
Latency: 50 ms. Request size: 1 segment. Response size: 2 segments.
Packetization delay: 10 ms (request and response).

+ single page
+ 50 ms SYN
+ 50 ms SYN/ACK
+ 50 ms + 10 ms ACK + segment
+ 50 ms + 20 ms response with 2 segments
+ 230 ms

#PROBLEM 5-5A  (1/1 points)
Suppose you type http://sing.stanford.edu/fullduplex/index.html into your
browser bar. Your browser starts a request for this page. Whatdoes http://
specify? Check all that apply.

The link layer to use.
A network layer address.
+ A transport layer port.
+ An application layer protocol.
A name for an application layer resource.

+ HTTP implies both the usage of TCP port 80 and the HTTP application-layer
  protocol.

#PROBLEM 5-5B  (1/1 points)
Suppose you type http://sing.stanford.edu/fullduplex/index.html into your
browser bar. Your browser starts a request for this page. What does
/fullduplex/index.html specify? Check all that apply.

The link layer to use.
A network layer name.
A transport layer port.
An application layer protocol.
+ A name for an application layer resource.

+ The path is one part of the HTTP protocol, which is the application layer.

#PROBLEM 5-5C  (1/1 points)
Suppose you type http://sing.stanford.edu/fullduplex/index.html into your
browser bar. Your browser starts a request for this page. What does
sing.stanford.edu specify? Check all that apply.

The link layer to use.
+ A network layer name.
A transport layer port.
An application layer protocol.
A name for an application layer resource.

+ The domain is a name that maps to an IP address. IP is the network layer.

#PROBLEM 5-5D  (1/1 points)
Suppose you have a network with an RTT of 100ms and a packetization delay of 1ms
for a maximum sized TCP segment of 1.4kB. You are using TCP Tahoe. The initial
congestion window is 2 packets and the network does not drop packets. You
request a 1.4kB file. Approximately what *percentage* (an integer from 0 to 100)
of the time it takes to receive the file is spent in connection setup?
"Connection setup" is the time before the first data byte is sent. Do not
consider teardown.

+ latency = RTT / 2 = 100 / 2 = 50ms for a one way trip
+ connection set up is before the first data byte is sent
+ SYN + SYN/ACK + ACK/GET request
    + the get request needs a 1 ms packetization delay before it is sent/the first
      data byte is sent
    + SYN and SYN/ACK are 50 ms each
+ total time
    + we're now accounting for the bytes sent, so for the ACK/GET request, 1 ms
      packetization + 50 ms latency
    + assume like in the examples from the lecture, response consists of 2 TCP
      segments (2 packet congestion window)
    + this means 2 packets are sent back in response to the GET request
        + 2 ms packetization + 50 ms latency = 52 ms response
    + (50 ms + 50 ms + 51 ms + 52 ms)
+ (50 ms + 50 ms + 1 ms) / (50 ms + 50 ms + 51 ms + 52 ms) = 50%

#PROBLEM 5-5E  (1 point possible)
Suppose you have a network with an RTT of 100ms and a packetization delay of 1ms
for a maximum sized TCP segment of 1.4kB. You are using TCP Tahoe. The initial
congestion window is 2.8kB and the network does not drop packets. You request a
15.4kB file. Approximately what *percentage* (an integer from 0 to 100) of the
time it takes to receive the file is spent in connection setup? "Connection
setup" is the time before the first data byte is sent. Do not consider teardown.

+ RTT = 100 ms, so one way trip time = 50ms
+ maximum sized TCP segment = 1 segment = 1.4 kB
+ initial congestion window = 2.8 kB = 2 segments
    + 2 + 4 + 8 + ...
    + (2 * 1.4 kB) + (4 * 1.4 kB) + (8 * 1.4 kB) = 19.6 kB, we don't fully send
      3 congestion windows
    + (2 * 1.4 kB) + (4 * 1.4 kB) + (5 * 1.4 kB) = 15.4 kB, so we only send a
      little over half of the last window in the Tahoe increase
+ conection setup = SYN + SYN/ACK + ACK/GET request without sending a byte
    + 50 ms SYN + 50 ms ACK + 1 ms packetization before sending anything
+ total time
    + connection setup along with...
    + 50 ms GET request right after the ACK and packetization
    + for the actual brunt of the data, recall that the congestion window
      results in sending of <window size> packets rapid fire/almost
      simultaneously, so the packetization delay builds up
        + GET request leads to...
        + 2 segments = 50 ms trip (time for all 2) + 2 ms packetization = 52 ms
        + confirmation acks after receiving packets = 50 ms
        + 4 segments = 50 ms trip (time for all 4) + 4 ms packetization = 54 ms
        + confirmation acks after receiving packets = 50 ms
        + 5 segments = 50 ms trip (time for all 5) + 5 ms packetization = 55 ms
+ (50 ms + 50 ms + 1 ms) / (50 ms syn + 50 ms + 51 ms GET request + brunt data)
+ 25

#PROBLEM 5-5F  (1 point possible)
Suppose you have a connection with an RTT of 30ms and negligible packetization
delay. Your maximum TCP segment size is 1.4kB. You want to download a web page
that is 29.4kB in size. Approximately how much longer will a connection with an
initial window of 2.8kB take than one with an initial congestion window of 14kB?
For example, if the large window takes 10 RTTs and the small window takes 25
RTTs, it takes 2.5 times as long, and write 2.5.

+ 29.4 kB total
+ 1.4 kB max segment size = 1 segment is 1.4 kB
+ initial window of 2.8 kB = 2 segments
    + 2 + 4 + 8 + 16...
    + 2 segments = 2.8 kB
    + 4 segments = 5.6 kB
    + 8 segments = 11.2 kB
    + we don't have to send the entire 16 segments
        + 7 segments (of max possible 16) = 8.4
    + now consider the RTTs
        + SYN + SYN/ACK + ACK/REQ = 3 one way trips
        + 2 segments sent = 1 one way trip
        + ACKs of 2 segments = 1 one way trip
        + 4 segments sent = 1 one way trip
        + ACKs of 4 segments = 1 one way trip
        + 8 segments sent = 1 one way trip
        + ACKs of 8 segments = 1 one way trip
        + 7 last segments sent = 1 one way trip
        + ACKS of 7 segments = 1 one way trip
        + 11 RTTs/trips total
+ initial window of 14 kB = 10 segments
    + 10 + 20 + 40...
    + 10 segments + 20 segments = 42 kB, don't send two full windows
    + 10 segments + 11 segments = 29.4 kB, done with half of second window
    + now consider the RTTs
        + SYN + SYN/ACK + ACK/REQ = 3 one way trips
        + 10 segments sent in burst = 1 one way trip
        + ACKs of 10 segments sent in burst = 1 one way trip
        + 11 last segments sent in burst = 1 one way trip
        + ACKs of 11 segments in burst = 1 one way trip
        + 7 RTTs/trips total
+ 11 / 7 = 1.6

#PROBLEM 5-6A  (1 point possible)
Suppose the route between your host and a server has a fixed RTT of 110ms. The
bottleneck link is 8 Mb/s. Ignore queueing delay and assume that the bottleneck
simply drops once a sender's window is larger than 110,000 bytes. You may assume
packets are only lost to drops on the bottleneck link. The TCP initial
congestion window is 10kB (10,240 bytes). The server can process requests
instantly. You are using TCP Reno. Assume this setup in all questions in this
quiz.

Using HTTP 1.0, you request a 20kB file. How long will it take to download the
last byte of the file in ms?

+ 20 kB file desired, 10 kB congestion window initially
+ one way trip, 110 ms / 2 = 55 ms
+ steps...
    + SYN + SYN/ACK = 55 * 2 = 110 ms
    + ACK/request = 55 ms
    + server's initial send of 10 kB window, 55 ms
    + host ACK, 55 ms
    + server sends remaining 10 kB of now 20 kB window, 55 ms
+ 110 ms for connection setup (SYN/SYN-ACK), 110ms for first 10kB
  (request/data), 110ms for second 20kB (ack/data), for a total of 330ms.

#PROBLEM 5-6B  (1 point possible)
Approximately what percentage (integer 0 to 100) of the bottleneck link capacity
does this use?

+ initial window of 10 kB = 10,240 bytes
+ we send a total of 20 kB, 2 * 10,240 bytes = 20,480 bytes
+ 20,480 bytes / 0.33 seconds = 62,060 B/s, so 6% of 8 Mb/s

#PROBLEM 5-6C  (1/1 points)
Suppose you use HTTP/1.1 to request 5 20kB files. You request all 5 as soon as
the connection opens. Will this transfer cause a packet drop?

+ No. The congestion window will be 10kB, then 20kB, then 40kB, then 80kB. In
  the final 80kB window, only 30kB is transmitted, since that completes the
  100 kB desired.

#PROBLEM 5-6D  (1 point possible)
Approximately what percentage (integer 0 to 100) of the bottleneck link capacity
does this use?

+ 100 kB / 0.55s = 186 kB/s = 18.6%

#PROBLEM 5-6E  (1/1 points)
Suppose you use HTTP/1.1 to request 30 20kB files. You request all 30 as soon as
the connection opens. Will this transfer cause a packet drop?

+ Yes
+ The total transmission is 600kB.
+ 110,000 bytes = 110 kB
+ The congestion window will start as 10kB, then grow to be 20kB, 40kB, 80kB,
  160kB, and 320kB.
    + 10 + 20 + 40 + 80 + 160 + 290/320 = 600
+ This final window will transmit 290kB, more than the router buffer can hold.
+ Note that depending on the segment timing, 160kB will likely not overflow the
  buffer. Because acks are self-clocking data transmissions, a window of 160kB
  will only fill 50kB or so of the router buffer and so will probably not
  overflow the queue. But this is a detail -- 290kB will definitely overflow the
  buffer.

#PROBLEM 5-6F  (1 point possible)
How long will it take to download all 30 files, in ms?

+ 1100

+ Once you drop a packet, you are operating at link capacity.
+ packet drop at 5th sending window (160 kB congestion window)
    + 110,000 bytes for drop = 110 kB
    + once we drop, we're operating at max capacity for this link
+ 10kB + 20kB + 40kB + 80kB + 110kB + 110kB + 110kB + 110kB + 10kB = 600 kB
+ 1 RTT to create the connection, then 9 RTTs to request the data and
  receive then ack
+ 9 RTTs + creation RTT = 10 RTTs = 1100ms

#PROBLEM 5-7A  (1/1 points)
Suppose you have a reasonably large BitTorrent swarm where half of all nodes are
behind NATs for which no traversal techniques work. What percentage (integer 0
to 100) of peer pairs cannot share data?

+ 25
+ Two peers cannot open a connection if and only if both are behind a NAT.
+ number of pairs where no NATS are present
    + say there are 6 hosts
    + H1 connects to H2, H3, H4, H5, H6
    + H2 connects to H3, H4, H5, H6
    + ...
    + in total, (n - 1) + (n - 2) + (n - 3) + ... + 1
    + 5 + 4 + 3 + 2 + 1 = 15
+ numbers of pairs where 50% of hosts are behind NATS
    + say there are 6 hosts
    + num connected pairs
        + H1 connects to H2, H3
        + H2 connects to H3
    + num pairs behind NATs
        + H4 connects to H5 H6
        + H5 connects to H6
+ number of pairs behind nats / number of possible pairs that can connect
+ [(n/2 - 1) + (n/2 - 2) + ... + 1] / [(n - 1) + (n - 2) + ... + 1] = 25%

#PROBLEM 5-8A  (1/1 points)
Which statement(s) are correct about DNS?

If a DNS resolver does not have a cached answer, it returns to the querying host
a list of DNS servers that do.
+ A host can use TCP to query a DNS resolver.
In the final level of the recursive query, a resolver queries an actual host
bound to the targeted domain name. This host responds with its IP address.
If the binding between a name and an IP address changes on one DNS server, all
DNS queries for the name will immediately return the new IP address.

+ The default protocol for DNS queries is UDP, but TCP can also be used. The
  resolver will respond with a cached answer if it has one, or it will perform
  the recursive queries, caching the intermediate results, and reply to the
  client with the result.

#PROBLEM 5-8B  (1/1 points)
DNS is used to translate names into IP addresses. ARP is used to translate IP
addresses into Level 2 (usually Ethernet) addresses. One optimization might be
to resolve Level 2 addresses using DNS. Why wouldn't this work?

+ We need to know the Level 2 address of the gateway to reach the DNS server
Ethernet addresses are not assigned by the network administrator, so they cannot
be returned by DNS
+ Level 2 addresses are not addressable outside of the broadcast domain that
  contains the receiver
It would require writing a merge-RFC which can take several years to process.
This would work.

+ The client, the DNS server, and every router along the path in between will
  use ARP to resolve the level 2 address for the next hop of the DNS packet.
  Clients need the level 2 addresses to reach the DNS server, so the DNS server
  cannot provide them. Furthermore, level 2 addresses are only addressable
  inside the broadcast domain of the client, so a DNS server that returned level
  2 addresses would not allow a client to send packets across the network.

#PROBLEM 5-8C  (1/1 points)
The root name servers have the following names: [a-m].root-servers.net. How can
a client resolve this name?

The root servers are no different; use the standard DNS recursive query
mechanism
+  This mapping is hard coded in the client
+  The client can use a DNS resolver, assigned by the DHCP server, to get the
  mapping
The client must cache these values

+ Answer: b,c. The mappings for the root servers are hard-coded into DNS clients
  and DNS resolvers. So the mapping is hard coded, or the client can request it
  from the DNS resolver.

#PROBLEM 5-9A  (1/1 points)
If a DNS response packet contains the name cheese.sandwich.foodie.com, which of
the following can be benefit from compression in the rest of the packet?

+ sandwich.foodie.com
+ foodie.com
+ cheese.sandwich.foodie.com
+ www.cheese.sandwich.foodie.com

+ EXPLANATION
+ The first three can be compressed fully using a pointer to the appropriate
  point in cheese.sandwich.foodie.com. The last one can be partially compressed:
  it will need to include www and then can contain a pointer to
  cheese.sandwich.foodie.com.
+ all choices have a suffix that matches the original packet name

#PROBLEM 5-9B  (1/1 points)
If a DNS response packet contains the name www.cs.berkeley.edu, which of the
following can benefit from compression in the rest of the packet?

+ www.cs.berkeley.edu
+ www.eecs.berkeley.edu
+ www.cs.stanford.edu
www.cs.berkeley.com

+ Answer: a,b,c. Answer (a) can be fully compressed. Answer (b) can compress
  berkeley.edu.
+ Answer (c) can compress edu.
+ Answer (d) cannot be compressed because there is no common suffix
+ suffix is based on a substring that ends at the end of the original string
+ the first 3 choices all have a suffix in common with www.cs.berkeley.edu

#PROBLEM 5-9C  (1 point possible)
The following is an raw RR (resource record) in hexadecimal.

c0 2e 00 01 00 01 00 00 02 6d 00 04 ab 43 d7 c8

Which of the following statements are true?

+ Compression is being used on the name field
The name can be found at offset 0xc02e in the packet.
+ This RR is an A record with 171.67.215.200
The TTL for this RR is 158976 seconds

+ Answer: a, c. Here is the decoded RR:
+ www-v6.stanford.edu. 621 IN A 171.67.215.200

+ The length byte of the name field has the first two bits set, so this
  indicates that the next 14 bits will be a pointer to the offset in the DNS
  packet that contains the name: 0x002e or 46 in decimal. The next two bytes are
  the type (A), and the next two for class (IN). The TTL is the next four
  bytes: 00 00 02 6d or 621 in decimal. The next byte is the data length (4), so
  the next four bytes are the data field. In this case, the data field is the IP 
  address 171.67.215.200.

#PROBLEM 5-10A  (1/1 points)
Choose the best filler. A query for the MX record for a domain name ****** as a
query for the A record for the same domain name.

must always return the same IP address
must always return the same IP address if the IP address exists
can never return the same IP address
+ may or may not return the same IP address Status: correct

+ Answer: d. There is no restriction that the MX and A records of a particular
  domain share an IP address.

#PROBLEM 5-10B  (1/1 points)
You want to switch the domain name for your website. However, you still want
users that use the old name to be able to access it transparently. Which of the
following solutions below would work?

+ Create an A record for the new name that contains the IP address of the
  machine that hosts your site. Leave the existing A record for the old name
  unchanged.

Create an A record for the new name that contains the IP address of the machine
that hosts your site. Configure the old name to return an NX response
(for not found).

+ Create a CNAME record for the new name that contains the old name.

+ Create an A record for the new name that contains the IP address of the
  machine that hosts your site. Configure the old name to return a CNAME record
  with the new name.

+ Answer: a,c,d. We want the old and new domains to both resolve to the IP
  address of the machine that hosts the site. We can either have an A record or
  a CNAME record for one domain and an A record for the either.

#PROBLEM 5-10C  (1/1 points)
Email systems today use the following format for email addresses:
user@domain.com where there is one MX record per domain. Alternatively, we could
have email addresses of the form: user.domain.com, whose MX record might differ
from the A record for user.domain.com. Assuming there are no wildcards allowed
in MX records (an extra feature we haven't covered), how would this change
impact the way email works today?

+ We would require an MX record for each user

+ Each user could have their own mail server, while today many users in a domain
  must share servers

+ This could tremendously increase the load on DNS servers for domains with many
  email addresses.

The new change would allow email addresses to benefit from DNS caching, while
email doesn't leverage DNS caching today

+ Answer: a, b, c. The proposal creates a separate subdomain for eachuser, which
  would require that an MX record be created for each user. Each user could
  therefore have an individual server, althoughmany records could point to the
  same server. The change would require that each individual user's email
  address were cached separately and result in many, many more queries that
  reach the authoritative name servers for MX records. Email servers today do
  benefit from DNS caching because a resolver can cache the MX record.

#PROBLEM 5-11A  (1/1 points)
Which of the following is/are true? Choose all correct answers.

It is impossible to get two DHCP responses for a DHCP request because there is
only one DHCP server per network.

+ A DHCP client does not need to explicitly release its IP address if it leaves
  a network. A DHCP server can garbage collect the address when the client's
  license times out.

+ A client that connects using a DHCP server does not have to use the DNS server
  that the DHCP server responds with. It can use a hardcoded DNS server and
  still receive results.

#PROBLEM 5-11B  (1/1 points)

A client receives a DHCP offer from a DHCP server in response to its discover
message. After receiving this offer, it issues a request to the server and the
server responds with an acknowledgment of that offer. If the DHCP server crashes
after this point, but before the lease expires, the client will lose its network
connection because the DHCP server will not be able to route the client-F¢s
packets out of the subnet.

+ Answer: False. The DHCP server is responsible for assigning IP addresses. Once
  assigned, the client can send packets to the gateway, which is responsible for
  sending them out of the subnet. The client need not talk to the DHCP server
  until its lease expires.

#PROBLEM 5-11C  (1/1 points)
You may have noticed that the DHCP request that Phil-F¢s iMac sends is a
broadcast packet, sent to Ethernet address: ff:ff:ff:ff:ff:ff, and IP address:
255.255.255.255. But when his iMac sends the DHCP request it has already
selected an IP address and it knows which server the selected offer came from.

Why does it send the request as a broadcast packet?

+ This way all DHCP servers on the network will get a copy of it, so they can 
  withdraw other offers that were not selected.

All DHCP servers must get a copy of the request so they can update their
mappings

DHCP is a distributed protocol and the broadcast packets ensures that servers
are synchronized

There is a mistake in the implementation; the DHCP request should be a unicast
packet.

+ Answer: a. The packet is sent as a broadcast so that other DHCP servers see
  that their offer was not chosen and can be withdrawn. There is no general
  requirement for synchronization or shared state between DHCP servers.

#PROBLEM 5-11D  (1/1 points)
You open a brand new laptop and try to join a wireless network. Which will your
computer send first, an ARP packet or a DHCP packet?

It will send an ARP packet first.
+ It will send a DHCP packet first. 

+ Answer: b. Before a laptop would need the IP address of any other clients, it
  needs to know a little more about the network. So, it will first send a DHCP
  discover packet to get an IP address of its own, as well as the subnet mask
  and gateway address.
