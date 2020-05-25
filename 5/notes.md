#5-0 Applications and NATS
+ 3 cornerstones of the internet
    + DNS (UDP): ask for names in client server relationship
    + HTTP: client server
    + Bittorrent: peer to peer model

#5-1 Network Address Translation
+ strong end to end: all the network should do is forward data
+ NAT
    + box between you and the internet
    + has its own IP address
    + has an internal interface and an external interface
    + NAT will rewrite your packet so that it appears as though its coming from
      its external interface
    + destination sees its from address X, sends back to X
    + NAT will re-translate received packet back to you
+ almost all home routers are NATS
    + ISP gives you a single IP address
    + NAT can give machines private local IP addresses
    + translate all these back to a single IP address
    + so, multiple machines can share a single IP address
    + extra security to limit the connections opened into your machine
+ How a NAT works
    + internal interface for local machine/node interaction
    + external interface
    + when a NAT sees packets destined for the internet from internet interface,
      generates a mapping
        + when a response returns, NAT checks for a matching mapping

#5-2 NATS - Types
+ Basic Mappings
    + <private IP, Port> <-> <External Interface NAT IP, Port>
    + NAT rewrites host request such that packet is from external IP and port
    + NAT rewrites received request back to private IP and Port
+ Full Cone NAT (plays the nicest)
    + least restrictive in terms of what packets it allows to traverse a mapping
    + any packet that comes into NAT for <NAT external IP, Port> pair will be
      translated to a certain <Internal IP, Port> pair
    + regardless of source address and source port
+ Restricted Cone NAT
    + filters packets based on source IP address
    + translate packets that comes from same source address
    + mappings are created between <node IP, Port> to
      <External NAT IP, Port>, it includes address of other end point to form
      <External NAT IP, Endhost IP, Port>
+ Port Restricted NAT
    + when a packet comes in from external host to external NAT IP address
    + NAT stores expected Port as well as endhost
    + so <Internal node IP, Port> to <External NAT IP, Endhost IP, Source Port, Port>
+ Symmetric NAT
    + port restricted
    + packets going from same source address, port to different destination
      addresses are given different external address-port matchings

#5-3 NAT Implications
+ NATS allow you to share an IP address among many hosts
+ NATS can help firewalling
+ Incoming Connections
    + can't have an incoming connection
    + no mapping to translate incoming packet to go to host, packets will be
      dropped
    + NAT allows connections out, not in
+ Connection Reversal
    + assume host A is behind a NAT
    + rendevous service
    + both hosts connected to this service
    + B sends request to A saying it wants connection, service forwards this
      request, then A opens the connection to B
+ Relays
    + assume both hosts are behind a NAT
    + additional host in the center relays data back and forth
+ NAT Hole Punching
    + assume two hosts behind NATS, want to connect directly to each other
      without relay
    + they first both talk to an external server to figure out external NAT
      interfaces/IP
    + clients can now send traffic to each others NATS
    + What if the NATS aren't full cones?
        + the server still gives the hosts the NAT external interface to send to
        + by sending, the NATS will be forced to set up mappings
        + hosts simultaneously send traffic to external NAT interfaces
        + this works for full cone NATS, restricted NATS, port restricted NATS
        + won't work for symmetric NAT
            + just because the server saw one port, doesn't mean the NAT will
              re-use the port for other traffic
            + THIS IS WHY SYMMETRIC NATS ARE FROWNED UPON - THEY COMPLICATE
              THINGS
+ Transport: No New Transport
    + NAT needs to know transport protocol for setting up mapping
    + creating a new transport protocol would result in NATS not recognizing
      them and dropping the packet
    + can't develop new transport protocols, stuck with TCP, UDP, ICMP

#5-5 HTTP - hypertext transport protocol
+ hypertext is a document format that lets you contain formatting info
+ client opens a TCP connection to a server and sends commands to it
    + GET: requests a page
    + server receives request, checks if its valid, sends response with
      necessary data
        + numeric code associated with response
        + 200 OK: accepted
        + 400 BAD REQUEST
+ HTTP Request
    + request line: method, URL, version
    + headers: field name, value
    + blank line
    + body of message
        + in a GET method, body is method
        + other types of requests store data in the body
+ HTTP Response
    + status line: version, status code, phrase
    + headers
    + blank line
    + body

#5-6 HTTP/1.1 Keep-alive
+ prior approach takes too much time opening windows
+ connection header for requests
    + keep-alive: tells server to keep connection open for more requests
    + close: tells server to close connection
    + server can always ignore header and do what it wants

#5-7 BitTorrent
+ clients request docs from other clients
+ break data into chunks of pieces with SHA1 encrptions, clients tell others who
  has what pieces
    + clients join swarms by downloading a torrent flie
    + try to download rarest piece first
    + at the end of the torrent with only a few pieces left, request for pieces
      from multiple peers
    + clients use TCP/IP connections

#5-8 DNS 1
+ URL
    + http:// is the application protocol and port
        + http protocol, port 80 by default for TCP
    + cs144.scs.stanford.edu is the host
        + how do we translate this to an IP address?
        + typing in the IP address would work too
    + /labs/sc.html is the file
+ Domain Name System
    + task it's trying to solve: Map names to addresses/values
    + handle a huge number of records
    + distributed control - people can control their own names
    + robust
    + read-only/mostly: look up names much more often than update them
    + loose consistency: changes can take a while to propagate
        + if a mapping changes, a delay is fine
+ these properties allow for extensive caching
    + look up a name, store result for a long time
+ DNS Servers
    + hierarchical zones: root zone, edu, stanford, scs, etc.
        + each of these zones can be separately administered
        + each domain can be by from multiple servers
    + root zone
        + 13 servers that are highly replicated (a, b, c, ..., m) and scattered
          around the world
        + bootstrap: root server IPs are stored in a file on name server
            + computer knows nothing, has to talk to a root server
            + ask for stanford.edu, ask root servers for the edu servers,
              contact edu servers asking for Stanford, etc.
        + DDOS attacks against root servers
            + attack DNS root servers to cause the DNS system to crash
+ DNS Queries
    + Recursive
        + ask server to resolve entire query
    + Non-recursive
        + server answers one step of query
    + usually UDP port 53
    + steps...
        + client wants to go to www.stanford.edu
        + asks resolver to recursively get the IP address of www.stanford.edu
        + resolver sends non-recursive query to root server asking about "edu"
        + root responds
        + resolver caches entry for edu, then contacts edu server
        + edu server responds with info about stanford
        + resolver caches result, asks stanford domain server
        + stanford server responds
        + resolver caches result, returns IP address to client

#5-9 DNS 2
+ queries and resources
+ query
    + starts from a client
    + asks resolver recursive query
    + resolver checks cache entries, if missing, can ask external servers
        + root for edu
        + Top level domain .edu server for stanford
        + ask stanford server...
        + each of these are nonrecursive queries and are cached by resolver
+ details
    + all DNS info stored in resource records
    + name: domain name
    + time to live: in seconds
    + class: for extensibility, usually IN/Internet
    + type: type of record
    + rdata: resource data dependent on type
+ Resource Records
+ RR Type A Records
    + IPv4 address
    + tells you address associated with name
+ RR Type NS Records
    + tells address of name server associated with name
+ DNS Message Structure
    + header: describes overall message
    + question: query or response
    + answer
    + authority
    + additional 
+ DNS message header
    + ID: pair queries and responses
    + flags: authoritative, recursive, etc.
+ DNS name compression
    + pack everything into a small number of bytes
    + names repeated through a packet are referenced
    + a name is broken up into labels based on hierarchy
        + www
        + stanford
        + edu

#5-11 DHCP
+ host needs
    + its own IP address
    + subnet mask: who is in the local network
    + gateway router: next hop
    + DNS server IP address is also useful
+ how do we get these values?
+ DHCP: dynamic host configuration protocol
    + machine can request configuration from DHCP server
    + move location? just request again
    + configuration duration has a lease, which can be renewed
        + only good for so long
    + garbage collection: when lease expires, reclaim that IP address
+ 4 step exchange
    + discover
        + discover what configurations are out there
        + this request is sent to ALL DHCP servers on the network, so that other
          DHCP servers can see their offer was not chosen
    + offer
        + server responds with offer
        + multiple servers can make multiple offers
    + request
        + client makes request for a configuration
        + request step repeats when the lease is nearing its end
    + ack
        + server acknowledges validity of configuration
    + release is optional
+ we're trying to bootstrap the IP configuration without any IP info
    + client sends UDP packets from port 68 to port 67 (DHCP port)
    + broadcasts to 255.255.255.255 since it doesn't know exact DHCP server
      address
    + use relays to forward across links

#5-12 Applications (layer) and NATS
+ NATS: network address translators
    + maps internal address/port pair with an external pair
    + manages subnet of private internal addresses
    + multiple hosts behind a NAT can share one external IP
    + no arbitrary incoming connections allowed
        + nuisance, generally the more "open" the better
        + NATs make things more complicated to create new connections or
          protocols
+ DNS: domain name system
    + app that uses UDP
    + maps URLs to records
        + A records: IPv4 address
        + NS records: name server
        + AAAA records: IPv6 address
        + MX records: mail server
        + etc.
    + hierarchy of servers
        + first ask root server to find out about "edu"
        + ask "edu" for stanford
        + ask "stanford" for full url
    + resolver: computer who queries DNS for you
        + makes caching more efficient, can share amongst clients
+ HTTP: hypertext transport protocol
    + uses TCP
    + request response protocol, ascii text
    + 1.0: each request based on separate TCP connection
    + keep-alive with 1.1: client can request many resources with only starting
      handshakes
