#8-00 Chat with Professors
+ up until now we've assumed no malicious activity
+ how can the network be used against us?
+ how can we defend against them? how can we communicate in an entrusted way?

#8-0 Security
+ types of attack
    + eavesdrop
    + masquerade/behave as though a piece of infrastructure proving information
        + forge responses causing client to send data to somewhere else
    + denial of service
        + overwhelm piece of infrastructure or sender so they can't send
+ 3 principles to secure your network
    + confidentiality: communicate such that others can't read it
    + integrity: prevnt tampering
    + authenticity: make sure other party communicating with is the desired
      target
+ cryptography

#8-1 Introduction to Network Attacks
+ botnets are computers owned by a single entity that are used to send spam
+ How can communication be compromised?
    + attacker eavesdrops on private communications
        + sniff and record network data
        + listen to metadeta for connection establishments
        + physical layer
            + tap a cable
            + listen to wifi using Wireshark(packets broadcast for everyone to
              hear wirelessly)
            + compromise router to duplicate and forward data
    + attacker modifies, deletes, or inserts data
        + tampering
        + changing contents of packets
        + redirecting packets to another server
        + take control of end host
    + prevent communication
        + denial of service
            + swamp servers/networks by generating billions of messages
+ Eavesdropping example
    + person making purchase from Amazon - wirelessly connected
    + makes credit card purchase via vanilla HTTP
    + attacker can eavesdrop...
        + by listening/sniffing wifi packets broadcast through air
            + if packets aren't encrypted, contents open
        + passive detector on wire to pick up signals leaking from cable
        + tap into optic fiber to break into public internet
        + subvert routers to trick it to re-route packets somewhere else
        + eavesdrop on HTTP data, can easily get private data sent
    + Man-in-the-middle
        + can terminate HTTP connection in middle, cutting off connection
        + pretend to be Amazon, access data
        + can pass data along to original, correct end host to fool both ends
    + Routing redirection
        + attack can fool router or Alice's DNS server to redirect packets to
          attacker
        + forced to browse attacker's website
+ What we want
    + secrecy/confidentiality: no one can listen-in to our communication
    + integrity: messages not altered in transit
        + attach MAC (message authentication code) code to make sure they aren't
          tampered with
    + authentication: confirm identity of other party communicating with
        + digital signatures and certificates
    + uninterrupted communication
        + don't want someone preventing us from communicating
        + combat denial of service...

#8-2 Layer 2 Attacks
+ eavesdropping with an interface in promiscuous mode
    + interface captures all packets passing by
    + computers allow this so they can act like ethernet switch
    + easy on wifi network
+ force packets to be broadcast in any ethernet network
    + overload forwarding tables
    + prevnt switch from learning addresses correctly
    + we can then eavesdrop using wireshark
+ masquerade as DHCP or ARP server
    + redirect packets to a different host
+ Original ethernet - shared cable
    + easy when packets broadcast onto shared medium
        + wifi, early ethernet, etc.
+ Ethernet switches
    + switches all for simultaneous communications
    + bad news for attacker, good for performance
    + packet only passes over links involved in a connection, so it's harder to
      overhear packets
    + attack tables?
        + switch learns addresses by watching packets in network
        + switch broadcasts packets when it receives packet for destination not
          in its forwarding table
        + attacker can then keep filling up forwarding table with other
          addresses
            + send packets with new destinations repeatedly
            + send at a high enough rate, switch will evict entries used in the
              actual (LRU protocol)
            + this is a MAC overflow attack
+ DHCP and DNS Masquerade
    + attacker tries to persuade you to use rogue DHCP server
    + DHCP service offered by network to help your computer configure
        + helps computer find "good" DNS server in network
    + computer broadcasts discovery packets to find the DHCPs server
    + if rogue DHCP server can respond faster than legit one, it can respond to
      consumer first
        + can give bad router address, sending traffic to attacker
            + makes it even easier to set up man-in-the-middle
        + can give address to rogue DNSserver
            + can return IP address of a different server to intercept traffic
+ ARP Masquerade
    + rogue ARP server
    + consumer first sends ARPaddress to find ethernet address to next hope
        + request to ARP server, which replies with address
        + rogue ARP server can respond faster, give wrong information
            + ethernet address to rogue server, redirecting consumer's traffic
            + man-in-the-middle attack setup

#8-3: Layer 3 Attacks
+ IP Network attacks
+ Common types
    + use ICMP to tell source end-host to redirect traffic
        + ICMP is supposed to tell end host how network is doing
            + e.g. TTL decremented to 0, or destination unreachable
    + BGP hijacking/IP hijacking
        + neighboring IP's trust each other to give accurate info about path
        + BGP's create TCP session with each other to pass true information
        + quite easy to redirect traffic
            + ISP can advertise prefix belonging to someone else, capture
              traffic
            + ISP advertises invalid ISPpath, creating black hole for traffic
        + ISP badly behaved is the strongest indicator of BGP hijacking
    + More specific prefix
        + ISPinserts more specific prefix to divert portion of address space
        + attacker once again has to take over BGP/TCP session or masquerade as
          ISP
+ ICMP Redirect
    + tell host, this packet went down the wrong path, send it down this other
      path next time you have to send to destination
        + ICMP redirect
    + attacker can send ICMP redirect message to host, telling it to send
      messages to attacker instead
        + can drop packet to prevent service
        + can look at packet before forwarding to destination
            + man in the middle
+ BGP Attacks
    + autonomous system can advertise IP addresses it doesn't own
    + autonomous system can't verify if given AS path is correct - leads to
      right destination or not
    + ISPs exchange BGPmessages over TCP session
        + if attacker can take over connection or inject packets, can create
          incorrect path advertisements
+ Almost any ISP can bring down the internet by advertising bad routes
+ BGP Hijacking
    + attacker can take over BGP router to compromise an AS
    + attacker can then re-route packets to attacker's AS
    + this AS can implement a man in the middle attack
    + AS can also broadcast packets to other AS's to create false paths

#8-4 Denial of Service
+ DDoS attacks: distributed denial of service
+ basic denial of service attack
    + something that prevents a service from being available
    + overload server/network with too many packets
    + can't function propertly and servce clients
    + maximize cost of each packet to server in CPU and memory
+ DDoS
    + attack comes from many places - hard to filter out
    + penetrate many machines
    + then, make hosts into zombies that attack on command via malware
    + later start simultaneous attack
+ DoS attack
    + target availability of service
    + why is this useful?
        + extortion/financial "blackmail"
        + revenge
        + bragging rights
    + can attack at many levels
+ simple DoS attacks
    + jam physical layer
        + make network inoperable with hardare
    + exploit link layer
        + 802.11 has NAV (net allocation vector) to suggest when network is free
    + flooding attack with pings
        + ICMP echo requests flood as fast as possible
    + amplifying server resources per packet helps attack too
+ EDNS attack
    + has some queries that can result in huge responses
    + large number of DNS resolvers on internet
        + can flood victim with DNS responses via these open resolvers
        + thus, also hard to kick bad guy off network
+ SMURF attack
    + ICMP echo supports pinging IP broadcast address
        + broadcast gets reply from every machine connected to network
    + big amplification
        + compromose one machine on net
        + ping broadcast address from victim
        + all machines reply, compromising victim
+ SYN-bomb attack
    + can attack transport layer
    + TCP handshake: syn, syn-ack, ack
    + send stream of syn packets from bogus addresses
        + server fills up with these bogus addresses, sends responses into
          nowhere
        + few 100 packets/second can disable servers
        + legitimate clients can't connect anymore since memory is stuffed
+ Other attacks
    + IP fragment flooding
        + kernel has to keep IP fragments around for partial packets
        + flood with bogus fragments to server, never bother to send remaining
          fragments
        + server can never re-construct fragments, stuffed
+ Application level DoS
    + DNS supports both TCP and UDP
        + many DNS server implementations block during TCP connections
        + just take out DNS server by writing long protcol message and keeping
          TCP connection open
    + SSL requires public key decryption at server
        + can use up server's CPU time by opening up many connections
        + cheap too for client

#8-5 Security Principles
+ core concepts
    + confidentiality
    + integrity
    + availablility
+ Basic problem
    + attacker can snoop on all traffic, suppress any packets, replay or
      generate new packets
    + to defend...
        + cryptography
            + build end to end secure communications despite how adversary may
              own the entire network
        + scalable system design
            + prevent communications from being blocked
            + e.g. DNS has so many root servers, very hard to block
+ Cryptography
    + mathematical principles and ideas for securing communication
    + often misused - little mistakes can cost you
    + safer to use existing, tested codes
    + confidentiality: communicate privately so no one else can read it
        + encryption
    + integrity: protect from tampering, ability to know if messages have been
      tampered with
        + hashes, siganturesl, and MACs
    + authenticity: ability for party to prove they are who they say they are
        + certificates, MACs, and signatures
+ Confidentiality
    + two parties should be able to send information secretly, even if others
      can read the messages
    + one-time pad: perfectly confidentiality
        + two can share perfectly random key of zeroes and ones
        + no one else has this key
        + can XOR message M with key, producing new message to send
        + XOR'ing again can reproduce it
    + disadvantage: key has to be as long as message, can take up huge amounts
      of memory
+ Integrity
    + make sure you receive unchanged message
    + cryptographic hash
        + function turns arbitrary length data into fixed length hash
        + additional property of collision resistant
        + if M x has hash H x, intractable for someone to find different message
          that has same hash
    + MAC
        + use key to generate MAC and check MAC
        + only someone with K can compute the correct MAC
            + intractable to generate MAC unless you have key
+ Authenticity
    + ability to verify someone's identity
    + exchange a "secret" beforehand, MAC
    + if no scecret, chain of trust to get secret
        + trust one party which can vouch for another it trusts, and so one
+ High Availability Design
    + DoS and DDoS
    + scale out system to distribute load to handle it, or filter traffic
      upstream

#8-6 Confidentiality
+ It's just between you and me
+ Symmetric Encryption
    + basic way to achieve confidentiality
    + two parties share some secret key K
    + message M and key K
        + M is called plaintext/cleartext - trying to keep it secret
        + Encrypt: E(K, M) -> C ciphertext
            + if encryption alg is secure, intractable to figure out cleartext
              from ciphertex unless they have the key
        + adversaries can look at it but can't decrypt it
        + Decrypt: D(K, C) -> original M
    + only decrypt message if you have the right key K
        + of course someone can test every K, so make K big enough
    + Encryption and Decryption take same key K, hence name of symmetric
      encryption
    + Examples: AES, Blowfish, DES, RC4
+ One-Time Pad
    + perfectly secret and impractical encryption algorithm
    + genreate perfectly random stream of bits K
    + sender and receiver exchange key beforehand
    + XOR to encrypt and decrypt
    + perfectly secure
        + if you have the ciphertext but not the key, any M is equally likely
    + fast: XOR'ing is quick
    + totally impractical: need a huge key the same size as all data
        + GB of data requires GB key
+ Ciphers
    + we want a cryptosystem where we can distribute a small key
        + share beforehand
        + use K to encrypt much larger message M
        + Encrypt(key, M) = C ciphertext
    + computationally intractable to find this M
        + could try all possible keys, but 2^128 is a lot of work
    + 2 Kinds
        + stream ciphers
            + psuedo-random sequence of bits based on key
            + XOR this stream
            + NOT A ONE-TIME PAD
            + repetition, has a lot of trouble in practice
                + if you re-use same psuedo-random sequence of bits
                    + same key in 2 different messages
                + attackers can launch attacks
        + block ciphers
            + operate on fixed sized blocks (64 bits, 128 bits, etc.)
            + maps plaintext block to ciphertext block
            + AES is the most popular one
+ Using Block Ciphers
    + Messages typically longer than one block
    + ECB (electronic code book) mode
        + break message M into blocks
        + encrypt each one individually
            + fast, can do it in parallel
        + adversary can't decrypt any block
        + NOTSECURE
            + attacker can learn repeated plaintext blocks
            + if M1 = M2, C1 = C2
+ Cipher Block Chaining Mode
    + choose initialization vector (IV) for each message, same size of block
    + vector XOR message block, encrypt to produce C1
    + take C1, XOR message block, encrypt to produce C2
    + and so on...
    + don't want to re-use IV
        + if you did....
            + if plain text regions are identical, cyphers will be identical
            + this leaks information to the adversary about overall structure

#8-7 Integrity
+ encryption provides confidentiality, but we need integrity too
    + for a system to be secure, need to know message hasn't been modified
+ Secrecy is not enough
    + encryption only prevents someone from reading
    + integrity algorithms let you know for sure that message was generated by
      specific person with key
        + e.g. can know routing vector came from an actual, legit node
    + confidentiality without integrity is rare and poor design
    + integrity without confidentiality is common
        + cases where integrity comes first, confidentiality isn't that
          important
+ Cryptographic hashes
    + anyone can generate a hash, no key required
    + hash function with special properties
    + can be sure no one has tampered with it, no key required
        + anyone can check it
+ MACs
    + all integrity properties of cryptographic hashes
    + also ensure person who generated MAC has key
    + only someone else with same key can check if MAC is correct
    + authentication
        + receiver with corresponding key can be sure that received MAC has
          integrity
        + only sender has the right key to have generated it
+ Cryptographic Hash
    + like ordinary hash, produces fixed length output from arbitrary length
      input
        + cheap to compute, faster than network
    + single scan of data, mathematical ops on each bit
    + collision-resistant
        + if I have data x, intractable to generate data y such that H(y) = H(x)
            + so basically given hash, intractable to generate data that has
              that hash
        + there are many collisions though
            + but they are very hard to find
    + SHA-256 or SHA-512
+ Using Cryptographic Hashes
    + small hash uniquely describes large piece of data
        + hash file, store as h1
        + later, compute hash h2 on same file
        + if h1 = h2, file hasn't been tampered with
+ HMAC
    + anyone can generate a cryptographic hash
    + MACs want to provide authenticity too, though
        + program that generated MAC has a key
    + simple
        + incorporate secret of hash
        + HMAC(key, message) = prepend key to mesage then produce hash
        + send just message and MAC
        + to check MAC, receiver prepends key, compute MAC
            + know only someone with key could produce MAC
    + VULNERABLE
+ HMAC Revisited
    + more right way with multiple hashes
+ always MAC encrypted data

#8-8 Public Key Cryptography
+ hidden in plain site - key doesn't have to be secret
+ distribute public key freely, still communicate securely
+ confidentiality
    + three algorithms: generate, encrypt, decrypt
    + generate generates 2 keys: public key k, private key k inverse
        + public key to encrypt passed to obtain ciphertext
        + private key to decrypt
    + give out public key freely, know only someone with private key can
      decrypt something sent
    + but since public key sent out so often, encryption algorithm must be
      randomized
+ Integrity: Signatures
    + three functions
        + generate (pair of keys)
        + sign (takes private key)
        + verify (takes public key)

#8-9 Certificates
+ how do we know what the right public key is?
+ how do we know if a site has a public key
+ problem
    + want to securely communicate with server
    + if we know server's public key, can communicate securely
    + key management
        + how do we get the server's public key
        + how can we be sure it's the server's public key
+ Attack man-in-the-middle
    + attacker pretends to be server, give own public key
    + no way of knowing whose key is right
    + attacker can open up connection to server, pretend to be you
        + can re-write, forward, insert new data, etc.
+ Certficate
    + digital document that binds name to public key
        + signed by private key inverse

#8-10 TLS
+ used by HTTPS, above transport layer
+ cipher negotation sent after TCP connection established
