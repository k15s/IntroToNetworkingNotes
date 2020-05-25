#7-1A  (1/1 point)
Given a signal-to-noise ratio of 30 dB, what is the maximum capacity of a
telephone line with bandwidth 3 kHz (in kbps)?

Note that a given signal-to-noise ratio S/N can be converted to db (decibels)
using: S/N(in db)=10*log10(S/N). Answer in kbps.

+ 30
+ EXPLANATION
+ Signal strength / noise = 10^(30 / 10)
+ 3000 * log2(1+1000) = 29.9 kbps.

#PROBLEM 7-1B  (1/1 point)
Which of the following statements are true?

Higher signal-to-noise ratio results in lower channel capacity.

The Shannon limit gives a lower bound on the capacity of a channel.

+ Bandwidth is the range of frequencies on which a signal can be transmitted.

+ A phase difference of 180 degrees means that one wave's peak occurs at
  the same time as another wave's trough.

#PROBLEM 7-1C  (1/1 point)
It is more suitable to use PSK instead of ASK when:

+ The signal can experience significant amplitude fluctuations in the
  physical layer medium.

The signal has to be sent at higher data rate.

The signal experiences little attenuation in the physical layer medium.

+ The signal attenuates over distance in the physical layer medium.

#PROBLEM 7-2A  (1/1 point)
Which one of the following best describes the relationship of symbols to bits?

+ Symbols are what the physical layer puts on the medium to encode some sequence
  of bits.

The physical layer passes symbols to the link layer when it receives bits from
the medium.

The physical layer appends error-correcting codes to the bits received from the
link layer: the original bits plus the error-correcting codes are called symbols

The terms "symbols" and "bits" are used interchangeably: they mean the same
thing

#PROBLEM 7-2B  (1/1 point)
On a noisy wireless link, which of the following are more likely to increase
link layer throughput? (Note: the term "coding gain" is as defined in the
lecture)

+ Using a sparser modulation
Using a denser modulation
+ Using lower coding gain
    + the smaller the ratio, the more physical layer bits that are mapped to a
      link layer bit
+ Using high-power transmitters
Using amplitude shift keying

#PROBLEM 7-2C  (1/1 point)
Consider a simplified form of 64-QAM modulation which supports a bit rate of
150 Mbps. Assuming it uses a coding gain of 5/6, how many symbols does it put on
the wire per second (in millions)?

+ 30
+ bit rate = bits/symbol * symbol rate * coding rate
+ using 64 QAM, so log2(64) = 6, so 6 bits/symbol
+ 150 Mbps = 6 * symbol rate * 5/6
+ symbol rate = 30

#PROBLEM 7-3A  (1/1 point)
An infrared remote control uses asynchronous communications to send 64 bits of
data using a 1MHz clock. The clocks at the two ends differ by at most 10kHz.
Which of the following statements are true?

If the receiver’s clock is 10kHz slower than the transmitter’s clock, then we
will count a bit twice.

+ If the receiver’s clock is 10kHz faster than the transmitter’s clock, then we
  will count a bit twice.

There is no risk that we will miss a bit, or count a bit twice.

If we double the amount of data sent, we are guaranteed that it will be
delivered reliably.

+ EXPLANATION
+ b is true because we will double count a bit after 0.5/1% = 50bit times.

#PROBLEM 7-3B  (1/1 point)
Which of the following are true about Manchester coding?

+ Used to introduce transitions between 0 and 1 to aid clock recovery
+ Guarantees a transition during every bit time
Doubles the amount of data that can be sent across the link
Avoids d.c. balance

#PROBLEM 7-3C  (1/1 point)
You friend discovers another form of block coding: 4B/6B, and she asks you to
compare it Manchester coding and 4B/5B. Which of the following might you
discover in your analysis?

Using 4B/6B, it will be harder to recover the clock than 4B/5B, but easier
than Manchester coding.

+ The overhead of 4B/6B will be less than Manchester coding but more than 4B/5B.

+ 4B/6B will allow for more control codes than 4B/5B and Manchester coding.

4B/6B will have more transitions than 4B/5B and Manchester coding

#PROBLEM 7-4A  (1/1 point)
There are 10 link layer frames from one host to another, each is 500 bytes in
size. Your buddy is upset with the additional overhead introduced by forward
error correction, so he decides to implement a system where the raw bits are
sent along the wire. You decide to implement a system using a FEC algorithm with
a coding gain of 3/4. The two of you decide to test your systems to see which
performs better. Assume that one bit is deterministically flipped every 10000
bits, and if either system cannot recover the error, the entire frame will need
to be resent. How many bits does your buddy’s system send for every bit yours
sends? (You can ignore any frames sent by the recipient back to the sender)

+ 1.1
+ Your buddy's system first sends 10 frames (40000 bits), out of which 4 bits
  in four different frames are corrupted. Then, he sends a fresh copy of these
  4 frames (16000 bits), out of which only 1 bit is corrupted (the 50000th bit
  from the start of transmission). Then, he resends that 1 frame (4000 bits)
  which also gets corrupted as it contains the 60000th bit. Then he resends
  this last frame (4000 bits) to complete the transmission without an error.
  Thus, in total, your buddy sends 64000 bits.
+ your system: sends 10 frames (53333 bits), all recovered.
    + 10 frames * 500 bytes * 8 bits/byte = 40,000 bits total
    + 40,000 bits / 3 bits per link layer * 4 bits per physical layer = 53,333

#PROBLEM 7-4B  (1/1 point)
We decide to use a (15, 11) Reed-Solomon code without interleaving. What is
the minimum number of bit errors that can occur to prevent decoding?

+ 3
+ EXPLANATION: 2 bit errors can be decoded.

#PROBLEM 7-4C  (1/1 point)
Same setup as B. What is the maximum burst of bit errors that we can recover
from?

+ 16
+ EXPLANATION
+ 2^4 - 1 = 15, so each data word is 4 bits long
+ 11 data words = 44 bits
+ Each of the code words is 4 bits and we can have up to 2 in error per chunk.
  If the burst alters the last two code words from one chunk and the first two
  from the next, we can recover.

#PROBLEM 7-5A  (1/1 point)
You are considering two retransmission strategies for all hosts in a network
using the ALOHA protocol. The first is a deterministic strategy where the first
time a collision is detected, the sender waits one second and retransmits. The
second time a collision is detected, the sender waits two seconds and
retransmits, and so on. The second strategy is a random strategy where when a
collision is detected, the sender waits a random amount of time and retransmits.
After the first collision it randomly chooses between waiting 0 or 1 seconds;
after the second collision it randomly waits between 0 and 2 seconds, and so on.

The deterministic strategy will perform better under low-load
+ The random strategy will perform better under high-load
+ The deterministic strategy will not recover from collisions
The average expected delay for these strategies is the same

+ In the deterministic strategy, when a collision occurs both senders will
  backoff for 1 second. Then they will try to resend and collide again, so they
  will backoff for 2 seconds. They will continue to collide like this forever.
  This strategy will not work, but the random one will.

#PROBLEM 7-5B  (1/1 point)
In a CSMA/CD network, the end hosts are upgraded from 10Mb/s to 100Mb/s. The
size and layout of the physical network otherwise remains the same. Which of the
following are true?

+ The minimum packet size should be made ten times larger.

The network will continue to work without change because the packets will arrive
ten times sooner.

The first bit of each packet reaches the destination 10 times sooner.

+ The amount of time a packet spends on the wire is decreased by 10 times.

+ EXPLANATION
+ For CSMA/CD to work, a packet needs to be on the wire for long enough to
  detect collisions. Because the amount of time the packet spends on the wire
  is decreased by 10 times, the minimum packet size also needs to be increased
  by 10x to ensure that it is long enough to reliably detect collisions. The
  speed of propagation in the wire is unaffected.

#PROBLEM 7-5C  (1/1 point)
A CSMA/CD network operates at 10Mb/s. The maximum distance between any pair of
end hosts is 2km. What is the minimum size packet a host should send in order
to detect collisions before it finishes transmitting a packet? Assume a
propagation speed of 2 x 10^8m/s.

2 bits
3 bytes
100 bits
+ 200 bits

+ Answer: Packet length >= 2LR / c
+ 2 * 2000 m * 10^7 / 2e8 = 200 bits

#PROBLEM 7-5D  (1/1 point)
Which of the following statements are true about a correctly sized CSMA/CD
network?

+ All collisions will be seen by every end host within the collision domain (not
  just the colliding hosts).

If there is a collision between two packets, the end hosts that are not
transmitting packets will only see one of the colliding packets, not both.

+ If an end host has a new packet to send, it can send its packet immediately
  after determining if the network is idle.

If the network uses Manchester encoding and there are two transitions every bit
time we can safely conclude there is a collision.

+ If the network uses Manchester encoding and there are three transitions every
  bit time we can safely conclude there is a collision.

+ EXPLANATION
+ A is true because simultaneously transmitted packets are guaranteed to
  “overlap” at every point in the network (although the overlap will be
  different in different places).
+ C is true by design of CSMA/CD.
+ D is not true because a correct sequence of all 0’s or all 1’s will have
  two transitions per bit time.
+ E is true because a single packet cannot generate more than two transitions
  per bit time.

#PROBLEM 7-6A  (1/1 point)
What is the theoretical maximum length for cable segments using the original
10Mb/s Ethernet standard (in meters)? You should assume the minimum length
packet is 72 bytes, and a propagation speed of 2 * 10^8 m/s.

+ 5760
+ P/R == 2x(L/c). Thus:
+ L = P * c / (2R) = 72 B * 8 b/B * (2 * 10^8) m/s / (2 * 10^7 b/s) = 5760 m

#PROBLEM 7-6B  (1/1 point)
Network A consists of a hub with twenty ports, each connected to exactly one
end host. Network B is identical, except the hub is replaced by a switch. Which
of the following are true?

If the end hosts in both networks try to send the same set of packets, Network
A will have fewer collisions than Network B.

+ Network A has one collision domain while Network B has twenty collision
  domains.

+ If the links in Network B are full duplex, twenty packets can be transferred
  successfully simultaneously between the end hosts.

In Network A up to nine packets can be successfully transferred simultaneously
between the end hosts.

+ The nodes in Network B can be further apart than the nodes in Network A.

+ EXPLANATION
+ A hub is just a repeater and amplifier, so it is logically equivalent to a
  single wire. There is one collision domain and only one host can transmit at
  a time. On the other hand, a switch segments the network, so each port will
  have its own collision domain. This also means hosts can be the maximum
  distance from the switch, while with a hub, the maximum distance must be
  between any two hosts. (i.e. nodes could be twice as far with a switch than a
  hub.)

#PROBLEM 7-6C  (1/1 point)
You just turned on a four port Ethernet switch (it hasn’t learned any
addresses yet), and connected a host to each port. You send packet #1 from
00:11:22:33:44:55 to 66:77:88:99:00:11 which arrives at port 1 of your switch.
Next, packet #2 is sent from 22:33:44:55:66 to 00:11:22:33:44:55 which arrives
at port 3 of your switch. Which of the following are true?

Packet #1 is broadcast on all ports

After the first packet is received, the switch associates 66:77:88:99:00:11
with port 1

+ Packet #2 is only sent out on port 1

Packet #2 is sent out on ports 1, 2, 4

+ The first packet is sent out of all unblocked ports except the one through
  which it arrived. The second packet will only be sent to port one, because
  the switch associates 00:11:22:33:44:55 with port 1 after packet #1
  arrives.

#PROBLEM 7-7A  (1/1 point)
Under ideal conditions, you can faintly detect a WiFi signal. In order to use
the signal for communication, it needs to be 2 times stronger. You are currently
16m from the access point. What is the furthest you can stand from the AP to
receive the required signal strength?

4m
8m
+ 11m
23m

+ Under ideal conditions wireless signal strength is reduced by the square of
  the distance.

#PROBLEM 7-7B  (1/1 point)
One of the large unlicensed bands used for 802.11b WiFi is 2.4GHz. What is the
length of these waves? Answer in feet.

+ 0.41
+ EXPLANATION
+ wavelength = c / f
+ c is the speed of light in feet per second (983,571,056). f is the frequency
  in cycles/second.
    + c/f is therefore feet/cycle, or the wavelength.
+ Using the formula, we have: 1ft/ns / 2.4 GHz = 0.42 ft
+ Or, 983,571,056 / 2400000000 cycles/sec

#PROBLEM 7-7C  (1/1 point)
Which of the following events could result in a weaker WiFi signal?

+ The relative humidity increases
+ Your neighbor installs an access point next door
+ You move from a room in your house to one that is much closer to your access
  point
+ You rotate your cellphone by 90 degrees

#PROBLEM 7-7D  (1/1 point)
What are some of the biggest differences between wired and wireless networks?

+ Wireless networks are much more susceptible to interference
+ Wireless networks are much more dynamic than wired ones
Wireless networks are better suited for handling TCP traffic than wired ones

+ EXPLANATION
+ TCP performs better when flucatuations don't occur as frequently they do with
  WiFi.

#PROBLEM 7-8A  (1/1 point)
Suppose that a wireless link layer using a CSMA-like protocol backs off 1ms
on average. A packet’s link and physical layer headers are always set at the
same bitrate and take a total of 125us to transmit. If a packet is sent with a
link layer payload of 1000 bytes at a bitrate of 1Mbps, what overhead do
the physical and link layers introduce? Calculate overhead as the fraction of
the complete packet time taken up by backoff and link/physical layer headers.

+ 0.123
+ EXPLANATION
+ (backoff + header) / complete time
    + header = 125 us
    + backoff = 1 ms = 1000 us
+ complete time = header + backoff + transmission time
    + 1000 bytes = 8000 bits sent total
    + 1 Mb/s = 1,000,000 bits/second
    + 8000 bits / 1,000,000 bits/second = 0.008 seconds = 8000 us
    + transmission time = 8000 us
+ 1125 us / (1125 us + 8000 us)

#PROBLEM 7-8B  (1/1 point)
What is the overhead if the packet sent at 1 Mbps is 30 bytes long?

+ 0.824
+ EXPLANATION
+ same process, overhead = (backoff + header) / complete time
    + 30 bytes = 240 bits / 1,000,000 bits/sec = 2.4 * 10^-4 seconds
    + 2.4 * 10^-4 seconds = 240 us transmission time
+ 1125 us / (1125 us + 240)

#PROBLEM 7-8C  (1/1 point)
What is the overhead if a 1500 byte payload is sent at 48Mbps?

+ 0.818
+ EXPLANATION
+ same process, overhead = (backoff + header) / complete time
    + 1500 bytes = 12,000 bits
    + 48 MBps = 48,000,000 bits / second
    + 12,000 bits / 48,000,000 bits / second = 2.5 * 10^-4 seconds
    + 2.5 * 10^-4 seconds = 250 us transmission time
+ 1125 us / (1125 us + 250 us)

#PROBLEM 7-9A  (1/1 point)
Suppose that you have two nodes communicating with an AP that are hidden
terminals to one another. The AP, except for acknowledgments, remains silent
and there are no other transmitters in the network. The two nodes are
both transmitting packets whose active airtime (time spent sending bits,
not waiting) is 1020 microseconds. A node waits 100us for an acknowledgment.
Therefore, after transmitting a packet, if no acknowledgment is received, the
idle time before a retransmission will be the 100us + the backoff. The network
is configured to have an initial CSMA/CA backoff of 0-100 microseconds.

This backoff value doubles for each retransmission (0-100us, then 0-200us, then
0-400us). Assuming both nodes start transmitting at exactly the same time, what
is the minimum number of backoffs before a packet is delivered successfully?

+ 5
+ EXPLANATION
+ The idle time between two transmissions must be at least 1020 microseconds.
+ The idle time is $$100{\rm us} + (0-100\cdot 2^{n-1})$$. When n=5, the
  backoff time will be in the range of 0-1600us, so an idle time of 100-1700us.

#PROBLEM 7-10A  (1/1 point)
A company wants to setup a WiFi network that will be used solely for instant
message conversations on the local subnet. Each IM packet is at most 1000 bytes
as the protocol limits the packet size. A packet larger than 1200 bytes will
trigger a RTS/CTS exchange. Would RTS/CTS introduce an overhead to this network?

+ Answer: No. As RTS/CTS would never be used, its control packets will never
  be transmitted and it won’t introduce any overhead.

#PROBLEM 7-10B  (1/1 point)
We have a laptop user transmitting at 1 Mbps with no loss. The packets,
including payload and all headers, are 100 bytes long and the average CSMA
backoff is 32 microseconds. Assuming RTS/CTS induces an extra amortized load of
160 microseconds per data packet, how much faster (in percent) is the CSMA/CA
protocol in this situation?

+ 19
+ EXPLANATION
+ At 832us per packet, CSMA/CA can support 1201 packets/s. At 992us, RTS/CTS can
  support 1008 packet/s. That's 19% faster.

#PROBLEM 7-11A  (1/1 point)
Is the duration field of an 802.11 frame described in terms of time or bits?

+ Time
Bits

+ It uses time for backwards compatibility: if a new version of WiFi has a
  new speed, older devices might not know how long a packet will take. Time
  fixes this.

#PROBLEM 7-11B  (1/1 point)
Suppose you are transmitting a 1500 byte WiFi frame at 600Mbps. If the
physical and link layer overhead is 92% (only 8% of packet time is
spent transmitting the 1500-byte WiFi frame), how long is this overhead,
in microseconds?

+ 230
+ EXPLANATION
+ 230 microseconds. Let x be the length of the overhead in microseconds.
  The length of the 1500 byte WiFi frame at 600 Mbps will be
  (1500*8)/(600*10^6) = 20 microseconds. So x/(x+20)=92%. Solving for x, we get
  x=230 microseconds.
    + 1500 bytes * 8 bits/byte = 12,000 bits/frame
    + 600 MBps = 600,000,000 bits/second

#PROBLEM 7-12A  (1/1 point)
Assume the configuration below:

A ---------------- B --------------- C

where A-B has an MTU of 1500 bytes and B-C has an MTU of 400 bytes.

Assume that link layer frames require 30 bytes of overhead, an IP header is
20 bytes, there is no loss in the network, and A always tries to send full
frames to C. For every packet that A sends, how many IP packets will cross the
link from B to C?

3 packets
+ 4 packets
5 packets
6 packets

+ EXPLANATION
+ For every packet that A sends, 1480 bytes will be IP data and 20 bytes will
  be IP headers (by definition, the ethernet MTU refers to the maximum amount
  of information that can be stored in the link layer's payload -- i.e., link
  layer headers are excluded) . Similarly, the maximum amount of data per
  packet that is transmitted from B to C is 380 bytes because 20 bytes is used
  for IP headers. 1480 bytes / 380 bytes = 3.89 packets, or 4 packets.

#PROBLEM 7-12B  (1/1 point)
(Same setup as A) For every 1000 bytes of IP data (not including the IP
header) that A sends, how many bytes must B send to C (including link layer and
IP header overhead)?

1050 bytes
1133 bytes
+ 1090 bytes
1450 bytes

+ EXPLANATION
+ For every 1500 bytes A sends B, 20 bytes will be the IP header and the
  remaining 1480 bytes will be IP data. B must fragment the entire 1500 bytes
  into 4 IP packets (see problem 7-12A). Therefore, the total bandwidth used
  between B and C per packet is:

  total = (initial_ip_data_bytes) + num_packets * (ip_overhead)

  total = 1480 + 5 * (20)

  total = 1580

  To send 1450 bytes of IP data, there must be 1580 bytes sent between B and C.
  This gives a ratio of total-B-C-bytes / a-b-data-bytes of 1580/1450 = 1.08966.
  Ie, for every 1000 ip data bytes that we send, B-C must send 1090 bytes over
  the network.

#PROBLEM 7-12C  (2/2 points)
You are setting up a network for a system you are building in which A
sends messages to C through B:

A ------- B ------- C

Assume that:
* A sends frames to B that must be fragmented into two on the layer between B
  and C
* C does not send messages back to A.

You have two cables, a pristine cable and a frayed cable. The pristine
cable causes no data loss. However, the frayed cable causes half of packets
sent over it to be lost (independently). There are no other causes of data loss
in the system.

What is the expected number of transmissions per packet for A to send to C if
the frayed cable is between A and B?

+ 2
+ Frayed between A and B:
+ E = 1+ (prob dropped between A and B)*E
+ E * (1 - prob dropped between A and B) = 1
+ E = 1/ (1 - prob dropped between A and B)
+ E = 2

What is the expected number of transmissions per packet for A to send to C if
the frayed cable is between B and C?

+ 4
+ Frayed between B and C:
+ E = 1 + (prob dropped either or both of two fragments)*E
+ E = 1 + (1 - prob did not drop either fragment)*E
+ E = 1 + (1 - p_no_drop*p_no_drop)*E
+ E * ( 1 - 1 + p_no_drop*p_no_drop) = 1
+ E * (p_no_drop*p_no_drop) = 1
+ E = 1/(p_no_drop*p_no_drop)
+ E = 1/(.5*.5)
+ E = 4
