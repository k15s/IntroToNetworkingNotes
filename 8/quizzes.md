#PROBLEM 8-1A  (1/1 point)
Which security mechanism best defends against eavesdropping attacks?

+ Secrecy
Integrity
Authenticity

+ Secrecy hides your information, and so prevents an eavesdropper
  from understanding it.

#PROBLEM 8-1B  (1/1 point)
Which security mechanism best defends against Man-in-the-Middle attacks?

Secrecy
Integrity
+ Authenticity

+ With authenticity, you can be sure that the other party is whom they say
  they are. With authenticity, an adversary cannot interpose on communication
  and pretend to be the intended other participant.

#PROBLEM 8-2A  (2/2 points)
Which of the following attacks would cause all traffic destined for Host X to
be routed to the attacker’s machine Y on a switched network?

+ Spoof ARP replies (using Y’s MAC) for all ARP requests for X’s IP address.
Spoof ARP replies (using Y’s MAC) for all ARP requests from X.
+ Broadcast ARP requests (using Y’s MAC and X’s IP).

+ EXPLANATION
+ Spoofing ARP replies to requests for X's IP's address would allow Y to
  insert its own MAC address into other node's ARP tables. Spoofing replies
  for requests from X would allow Y to redirect traffic to other hosts, but
  not to X. Finally, broadcasting requests that seem to be from X would
  insert entries into other's ARP tables.

#PROBLEM 8-2B  (1/1 point)
You log into a public, wired desktop at Stanford. After typing facebook.com
and then enter in the navigation bar of your favorite browser, you find out
that Facebook has recently moved to a pay-per-month service and wants your
credit card number. You notice that your default gateway and DNS resolver have
the same address and that all of these IP addresses are from a subnet you've
never seen before. Which of these attacks is most likely?

+ Malicious DHCP server
MAC overflow
Man in the middle
ARP masquerade

+ EXPLANATION
+ Of these options, the most likely attack is a malicious DHCP server. The
  server is giving your host a configuration that allows an adversary
  to intercept all of your traffic through a malicious gateway and DNS server.
  MAC overflow is an eavesdropping attack, while this attack involves generating
  malicious packets. This isn't a man-in-the-middle attack because the malicious
  servers are not forwarding requests to the correct recipients. It is
  unlikely to be an ARP masquerade attack because you see a new subnet; if
  it were ARP masquerading then the addresses would most likely look normal.

#PROBLEM 8-3A  (1/1 point)
In the video, you learned about different kinds of layer 3 attacks. Which of
the following are true?

A “blackhole” layer 3 attack causes harm by flooding the network with packets
that never reach their destination.

+ A more-specific attack works because routers prefer longest-prefix matches to
  other routing entries.

ICMP cannot be used to carry out the blackhole attack. Layer 3 attacks can never
be detected.

+ EXPLANATION
+ A blackhole attack works by routing packets to an IP which doesn’t route any
  packet it receives. So, ICMP can be used to carry out a blackhole attack by
  sending a host an ICMP redirect message to an IP that does not route traffic.
  Note that the IP may in fact exist on the network -- it just does not forward
  any of the redirected packets. And of course, with appropriate monitoring,
  you can think of techniques to detect layer 3 attacks. Try thinking of some
  ways to detect blackhole attacks.

#PROBLEM 8-3B  (1/1 point)
Suppose a legitimate AS uses BGP to advertise a route for /32 IPv4 prefix.

A malicious AS cannot hijack the prefix because the legitimate AS is already
announcing the most specific prefix possible (i.e. /32).

+ A malicious AS can hijack the /32 prefix an announce a shorter path to the /32
  prefix.

A malicious AS can send an ICMP redirect message to one of the BGP routers to
redirect traffic to the malicious AS.

If the malicious AS is only peering with a small fraction of AS’s in a network,
the malicious AS is guaranteed to hijack *all* traffic to the /32 IPv4 prefix.

+ EXPLANATION
+ Recall that AS’s use multiple policies to select the next hop AS -- in
  general, the shorter the path, the more it is preferred. Also, BGP routers
  only use BGP routing protocol to determine next-hops. So, they should not
  process ICMP redirect messages and update their routing state. And finally,
  the last option is not always true -- the malicious AS’s collateral damage
  will be restricted to those AS’s that are closer to the malicious AS than the
  legitimate ones (unless the malicious AS manages to peer with ALL AS’s!)

#SECURITY V: DENIAL OF SERVICE ATTACKS  (1/1 point)
You notice that you are having a lot of trouble connecting to your server. Which
of one the following would strongly suggest a Syn-Bomb attack?

Wireshark showing tcp packets from many sources

+ Wireshark showing tcp syn packets from many different sources (ie ip
  address-port pairs) over a short period of time

Increased memory consumption

Increased cpu utilization

+ EXPLANATION
+ the synbomb attack works by half-opening many TCP connections. The server has
  to keep state for each of these connections.

#SECURITY V: DENIAL OF SERVICE ATTACKS 2  (1/1 point)
You notice that you are receiving unsolicited ICMP echo replies from all of the
hosts on your subnet. You are most likely the victim of...

+ SMURF attack
ICMP flooding attack
Syn-Bomb attack
MS Blaster Worm

#SECURITY V: DENIAL OF SERVICE ATTACKS 3  (1/1 point)
The smurf attack is an example of an attack using amplification. T/F?

+ True
+ EXPLANATION
+ the attacker sends one forged packet to the broadcast address and the victim
  must process replies from all hosts on that subnet.

#IN VIDEO QUIZ1  (1/1 point)
You generate a perfectly random key K. You send two messages, M1 and M2,
encrypted with K (C1 = K XOR M1 and C2 = K XOR M2). Does sending C2 leak
information about M1 and M2?

+ Yes
+ If an attacker hears both C1 and C2, can reconstruct M1 XOR M2
+ C1 = K XOR M1
+ C2 = K XOR M2
+ C1 XOR C2 = (K XOR M1) XOR (K XOR M2) = M1 XOR M2
+ e.g. if M1 = M2, can figure out that C1 and C2 are the same
+ reason why it's called a one-time pad: only use it once

#IN VIDEO QUIZ 3  (1/1 point)
Given a shared secret key, can you transmit files secured from adversaries over
a network solely by encrypting them in CBC mode?

+ No
+ encryption provides confidentiality but not integrity
    + adversary can still modify/tamper with messages

#PROBLEM 8-6A  (1/1 point)
Suppose you are using the Blowfish block cipher in CBC mode. It turns out,
however, that there's a bug in the implementation you're using such that it
always chooses 0 as its initialization vector. How might this make your traffic
vulnerable?

An adversary will be able to decrypt the first cipher text block of every
message you send.

An adversary will be able to decrypt every ciphertext block of every message you
send.

An adversary will be able to generate the cipher text block for any desired
value of the first clear text block.

An adversary will be able to generate the cipher text block for any desired
value of any clear text block.

+ An adversary will be able to detect when you send duplicate messages.

+ EXPLANATION
+ The data is still encrypted, so confidential: an adversary cannot read it.
  However, an adversary can see if two messages are the same, as two identical
  messages will produce identical cipher texts.

#PROBLEM 8-6B  (1/1 point)
To deal with this buggy Blowfish implementation, a friend suggests adding 64
bits of random data to the beginning of each message. Will this work-around
address the broken initialization vector code?

+ Yes
+ This will fix the problem by reintroducing randomness in the first block
  passed to the cipher.

#PROBLEM 8-7A  (1/1 point)
Your friend is building a transaction processing system and wants to
prevent adversaries from issuing new transactions. Your friend defines a
message structure:

struct transaction {
    uint32_t timestamp; // in seconds
    uint64_t source;
    uint64_t destination;
    uint64_t amount;
    char message[64];
    uint8_t sha256[32];
}

where the sha256 field is a SHA-256 hash of all prior fields and the message
field is an optional text message that is logged but not processed. The
timestamp field is similarly logged but not processed. Your friend tells you the
SHA-256 hash will provide integrity and prevent an adversary from issuing new
transactions. Is this true?

+ No, the has will not provide the protections desired.
+ Anyone can generate such a transaction and compute the hash. An adversary
  could easily generate a new transaction with a correct hash.

#PROBLEM 8-7B  (1/1 point)
Your friend extends the system to use a message authentication code with a
shared key rather than a hash, such that the message structure is

struct transaction {
    uint32_t timestamp; // in seconds
    uint64_t source;
    uint64_t destination;
    uint64_t amount;
    char message[64];
    uint8_t mac[32];
}

The MAC is computed with HMAC using SHA-256 as the hash function. Will this
scheme prevent an adversary from generating new transactions and/or replaying
transactions?

+ No, it will not prevent an adversary from issuing transitions.
+ EXPLANATION
+ The MAC will prevent an adversary from generating new transactions, but
  the adversary can replay existing ones. For example, convince you to
  transfer something to their account then launch a replay attack to have
  you transfer it 100 times.

#PROBLEM 8-8A  (3/3 points)
You want to buy a new wireless router and your neighbor, whose network is
close enough that your computers can see it, tells you her new one is great. You
buy the same model and install it. You find out a few weeks later that every one
of the routers in this product line has the same RSA private/public key pair
installed for making secure web connections, which you can easily find out. The
router uses this key for authentication and for encrypting the pre-master secret
in its TLS sessions with clients (e.g., for configuration). What attacks could
your neighbor launch on your network with this knowledge? Mark all that apply.

Man-in-the middle to impersonate any web server.

+ Man-in-the-middle to impersonate your router.

+ Read all of the traffic encrypted by the public key.

+ Sign documents that impersonate being from your router.

Read all traffic between your router and devices attached to it.

+ EXPLANATION
+ The vulnerable private key only jeopardizes traffic with the router itself. So
  the first attack isn't possible. Knowing the router's private key does not
  allow someone to impersonate eBay, for example. However, the attacker could
  impersonate the router itself. Furthermore, because the attacker has the
  private key, she could read all traffic encrypted with the public key as well
  as sign documents that pass verification with the public key. However, this
  doesn't mean that she could read *all* traffic. If RSA is used to establish a
  symmetric secure session, then there are ways in which this can be done that
  doesn't reveal the symmetric key (e.g., reuse a prior negotiated key).

#PROBLEM 8-8B  (1/1 point)
You wish to send a message confidentially using RSA. You generate your
public/private key pair and encrypt your message m as m^k mod n following the
algorithm on slide 5. Will the resulting message be confidential?

+ No
+ Recall that public key encryption needs randomness, otherwise an attacker
  can observe message repetitions. If you just take the plaintext message and
  raise it to the kth power, this is a deterministic transformation.

#PROBLEM 8-9A  (1/1 point)
Suppose a network attacker block responses from Google’s certificate catalog.
You receive an application certificate signed by Google and want to check the
certificate whose key signed this application certificate. What should the
browser do if it receives no response to its query to Google?

Accept the application certificate as valid.

+ Reject the application certificate as invalid and close the connection to
  the application.

Ask the user if the application certificate should be accepted

Flip a coin to decide if application certificate should be accepted or rejected

+ EXPLANATION
+ If you cannot verify the signing certificate, you can't authenticate the
  other party.

#PROBLEM 8-9B  (1/1 point)
Using public key cryptography, X adds a digital signature σ to message M,
encrypts (M, σ) and sends it to Y, where it is decrypted. Which one of the
following sequences of keys is used for the operations?

Encryption: X’s private key followed by Y’s private key; Decryption: X’s public
key followed by Y’s public key

Encryption: X’s private key followed by Y’s public key; Decryption: X’s public
key followed by Y’s private key

Encryption: X’s public key followed by Y’s private key; Decryption: Y’s public
key followed by X’s private key

+ Encryption: X’s private key followed by Y’s public key; Decryption: Y’s
  private key followed by X’s public key

#PROBLEM 8-9C  (1/1 point)
Suppose the private key for a root certificate authority is compromised. Which
of the following actions is necessary?

There is no way to recover from this

Browsers must not trust certificates signed directly by this certificate
authority

+ Browsers must not trust certificates where this certificate authority is
  anywhere on the chain of signatures.

No change is necessary

+ EXPLANATION
+ When the private key of the certificate authority is compomised, someone can
  sign forged certificates. They can also use those certificates to sign other
  forged certificates. Thus, no certificate that has been signed directly or
  indirectly by the compromised private key can be trusted.

#PROBLEM 8-10A  (1/1 point)
Suppose that an adversary is able to reverse-engineer the random number
generator used by TLS such that it can guess with 1% accuracy the next random
number a TLS implementation will generate. Each random number has a uniform,
independent, 1% chance of being guessed. The adversary then observes a TLS
session setup as described in slide 9. What fraction of TLS sessions would such
an adversary be able to compromise?

100%
99%
+ 1%
0.1%
0.0001%
0%

+ EXPLANATION
+ The only random number that is sent secretly is the pre-master secret. If
  the adversary can guess this value it can compromise the session. Since
  the adversary has a 1% chance of guessing a random value, the probability is
  1%.

#PROBLEM 8-10B  (1/1 point)
Which of these reasons could be invoked by a web server admin for not using
HTTPS:

+ TLS incurs a performance overhead that will increase the load on the server.

+ We need to buy a certificate and not just self-sign one, otherwise the
  clients' browser will complain.

+ It will break content caching by middleboxes, which may increase the load on
  the server.

TLS is not good enough for us, because even if it prevents an attacker from
reading the messages exchanged, it won't stop this attacker from modifying them.

+ It will break virtual hosting.
