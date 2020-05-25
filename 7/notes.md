#7-0 Lower Layers
+ dig below IP layer
+ what is a link? How do they work?
+ fundamental principles of computer communications
    + signal to noise ratio
+ how are these links filled?
    + wired links: ethernet
    + wireless links
        + signal can vary - not fixed over time
        + broadcasted - everyone can hear
        + hidden clients problem - no direct signal contact with each other
+ different links can cary packets up to different sizes
+ IP fragmentation is necessary
    + large max transmission wire -> smaller one
+ when 2 hosts are communicating with each other, they'll have different clocks
    + somehow have to indicate what clock rate and frequency we're using for
      correct decoding and decoding.

#7-1 Shannon Capacity and Modulation
+ Capacity: how much information can a given channel/link carry?
    + wire, wireless, sound waves, channel is whatever we want
    + turns out there's a limit -> limit is called a Shannon limit
+ Shannon Limit
    + Capacity = bandwith * log base 2 (1 + Signal Strength/Noise)
    + higher S/N: lower noise is expensive due to better hardware, or stronger
      signal (louder)
    + amount we can send is proportional to bandwith and signal to noise ratio
+ Analog Signal
    + generally how we represent a signal
    + sin wave
    + amplitude = signal strength/loudness, height above the "x axis"
    + wavelength = distance between peaks/depths
        + speed of light c = 1 foot/nanosecond = 1 billion feet / second
        + if wavelength is 1 foot, wave travels 1 billion feet per second
            + therefore, 1 billion waves per second is the frequency
    + frequency = 1/wavelength
    + bandwidth: size of frequency ranges used
    + phase: timing of wavelength
        + phase can be offset if entire wave is slid over by some certain "time"
+ waves
    + amplitude shift keying (ASK): adjust amplitude of waves to send different
      bits (0 or 1)
        + commonly used in wired networks (Ethernet today)
        + wires don't have a lot of resistance
        + stable signal strength
            + harder in wireless networks since we have to identify possible
              amplitudes
    + frequency shift keying (FSK): adjust frequency/distance between waves
        + rarer
    + phase shift keying (PSk): adjust phase of waves
        + useful when channel can have major changes in signal strength
        + binary phase shift keying
            + 2 phases: 0 or 180 degrees ("1/2" shift)
        + quadrature phase shift keying
            + four phases: 0, 90, 180, 270
    + shifting phase
        + it's easy to shift phase
        + add separate
    + I/Q Modulation
        + I: in-phase component
        + Q: quadrature component
        + symbol is linear combination of I and Q
            + binary
                + (1,0) and (-1,0)
            + quadrature
                + (1,0) (0,1) (-1,0) (0,-1)
        + change scaling factors of I and Q to generate various phases
+ Symbols vs Bits
    + symbol: unit of transfer at the physical layer
        + we might not be able to transfer an entire byte at the physical layer
        + so, we use symbols when talking about the physical layer
    + BPSK has 1 bit per symbol
    + QPSK has 2 bits per symbol
    + although the link layer transfers bits/bytes
        + transfer sequence of bits into sequence of symbols
+ QAM (quadrature amplitude keying)
    + phase AND amplitude keying at the same time
    + common approach
    + 16-QAM: 16 symbols, 4 bits / symbol = log2(16)
    + 32-QAM: 256 symbols, 5 bits / symbol = log2(32)
    + can be represented on coordinate plane
+ many ways to represent bits in terms of analog signals

#7-2 Bit Errors
+ Bit errors and decoding to protect against bit errors
+ Shannon limit
    + possible data rate bounded by signal noise to ratio
    + stronger signal or lower circuit noise, send more
    + but this is a theoretical limit
    + bandwidth is typically fixed
+ noise can interfere with analog signal and throw it off, fuzzier
+ Low noise reception
    + visualizing the coordinate plane, signal still concentrated around a
      particular phase and amplitude
+ High noise reception
    + visualizing the coordinate plane, signal overlaps over multiple phases
      and amplitude
+ this is how bit errors are introduced - signal to noise ration such that some
  of the symbols are misunderstood
    + magnified by density of constellation/coordinate plane
+ sending packets just as raw bits, directly translating bits in packets to bits
  in symbols are extremely inefficient because of bit error probability
+ in practice
    + transform packet data of bits/bytes into symbols with some amount of
      redundancy/error correcting codes
    + packet gets a bit longer at physical layer because of this
    + so, if a couple of bits are wrong, we can still get the original packet
      data just fine
    + this is coding
+ Coding
    + add redundancy at the physical layer to improve throughput
    + protect against bit errors
    + aggregate improves throughput of system
    + coding gain
        + ratio of bits at link layer / bits at physical layer
        + 1/2 code: each link layer bit is 2 physical layer bits
        + 3/4 code: each 3 link layer bits are 4 physical layer bits
+ data rate or bit rate = bits/symbol * symbol rate * coding rate
+ Overview
    + bits (link layer) become chips (physical layer)
    + physical layer has to handle noise which can create chip errors
        + dense modulation - higher throughput since more bits per symbol, but
          vulnerable to noise
        + sparser modulation has fewer errors
    + Translating between link and physical layer bits
        + 1:1 mapping is a bad idea because of bit errors
        + coding gain: L2/L1 ratio, so each bit can be multiple chips
    + using 64 QAM, so

#7-3 Clocks
+ clocks and clock recovery
+ when we send over link, we assume a data rate
+ clock must be used by transmitter to achieve a data rate
    + receiver must also know this rate so it can decode the data sent
    + problem is that the receiver has to figure this out so it knows where
      one bit ends and another begins
+ Outline
    + data transmitted via a clock
    + received knows when to sample incoming bits
    + asynchronous communications for short messages
    + synchronous communications for longer messages
        + encode clock with data
        + recover clock
+ data transmitted with clock
    + this allows it to have a frequency
    + ideally a wire would connect sender's clock to the receiver
+ if we don't know the sender's clock
    + receiver's clock is on a different oscillation and frequency
    + receiver can miss bits of data and get the wrong result
    + eventually this could result in a complete bit shift
    + generally speaking, the clock frequency is constant
+ asynchronous communication
    + not used by networks in general, sometimes for links with short packets
    + receiving clock happens to correctly sample bits just by its oscillation
      on its own
+ synchronous communications
    + different hosts of slightly different frequencies
    + clock recovery units take the incoming signal and determine clock of
      sender
        + frequency and phase are examined
        + look at bits, detect when there's a transition and where one bit
          starts, another one ends
        + phase lock loop
    + receive clock domain
        + before this, we use the transmitters clock domain/sender's clock
        + receiver's clock domain
        + transition into receiver's clock domain
            + flip flop passes bit data into FIFO queue leading to receiver's
               domain
            + bit written into FIFO using transmitter's clock
            + bit read out of FIFO using receiver's lock
            + FIFO is called an elasticity buffer
                + picking up slack between 2 clocks
    + encoding for clock recovery
        + if clock were sent separately, like on a machine, simple
        + if clock is not sent separately, data streem must have sufficient
          transitions so receiver can determine clock
            + encode the data
            + Manchester coding (only used occasionally nowadays, but it's
              simple)
                + downward transition when we see a 0
                + upward transition when we see a 1
                + insert extra transition between repeated bits
                + advantages
                    + guarantees one transition per bit period
                    + ensures d.c. balance
                        + equal numbers of hi and lo
                        + spending equal mounts of time above and below 0 line
                        + this helps differentiate between 0's and 1's
                    + disadvantages
                        + double bandwidth in worst case
                        + all 1's, twice as many transitions as we need
            + 4b5b encoding (4 bits original data -> 5 bit encoding)
                + 0000 = 1110
                + 0001 = 01001
                + 0010 = 10100
                + advantages
                    + more bandwidth efficient (25% overhead)
                    + extra codes can be used to control information
                + disadvantages
                    + fewer transitions, clock discovery is harder
        + Summary
            + encoded data sent out over network link
            + clock recovery unit examines transmitions to determine clock
            + placed into elasticity buffer that receiver can pull from

#7-4 FEC and Reed-Solomon
+ forward error correction
+ for a given modulation scheme and signal to noise ratio
    + can calculate expected bit error rate
    + this bit error rate will never be 0
+ coding: add some redundancy to accomodate bit errors
    + cost < benefit
    + increase throughput since now all packets get through correctly
    + forward error correction: proactively add extra redundancy to correct
      potential errors in the future
+ Coding algorithms
    + Reed-Solomon (simpler and extremely common)
        + ax^2 + bx + c
            + any polynomial of degree k
        + take k chunks of data to encode
        + k chunks become coefficients of k - 1 degree polynomial
            + 3 chunks become ax^2 + bx + c
            + with any 3 points on the parabola, can determine the entire
              parabola
        + send coded data
+ Details
    + two kinds of errors
        + erasures: location of error known ("erased" values)
        + errors: location unknown (bit error)
    + take k chunks of data, code into n chunks (n >= k)
    + can correct up to (n - k) erasures (need k points)
    + can correct up to (n - k)/2 errors
    + example
        + R = (255, 223) = take 223 bytes of data, turn into 255 coded bytes
        + 255 - 233 = 32
        + this RS code can protect agaisnt 32 erasures or 16 errors
+ Reed-Solomon Example
    + R(7,5)
    + each initial data word is 3 bits long, 7 = 2^3 - 1
        + break data up into 3 bit chunks
    + 5 data words = 15 bits
        + each 15 bits is turned into 7 3-bit codewords
    + pad original data with 0's if it's not evenly divisible by the data word
      length
+ Conceptual Reed-Solomon Interleaving
    + errors in physical layer are random are easy to handle
        + even if multiple errors, as long as they're in separate code blocks
    + bursts are harder to handle
        + errors in same block
    + interleaving is sending code blocks interleaved with each other
        + A0 D0 C0 D0
        + A1 D1 C1...
        + so bursts of errors are spread out through code blocks

#7-5 MAC and CSMA/CD
+ wifi for wireless, ethernet for wired
    + ethernet is most widely used link layer mechanism
+ CSMA/CD at heart of how ethernet works
+ why is Ethernet called layer 2?
    + in the 4 layer model (application, transport, network, link)
    + ties to 7 layer OSI model - Ethernet wraps Link and Physical layer
+ sharing a medium
    + ethernet: multiple hosts share a common cable
    + have to decide who gets to send and when
        + only one packet allowed on wire at a time
        + MAC protocols (medium access control protocols)
            + ethernet doesn't use it very much anymore
+ examples of MAC Protocols
    + Aloha
        + simple and random
        + any host can try to send at any time - no waiting
    + Ethernet (main one)
        + every host randomly sends
    + Token Passing
        + Complex and deterministic
        + explicitly control which host sends next packet
        + special packet sent from one host to the next
            + this special token determines who gets to send next
        + but tokens can be lost easily, out of order, etc.
+ Goals of MAC Protocols
    + high utilization of shared channel
    + fair among end hosts
        + everyone gets equal opportunity to send
    + simple and low cost to implement
        + can be widely deployed
    + robust to errors
        + one end host can fail without breaking entire network
+ Aloha Protocol Example
    + all hosts transmit on single frequency
        + send to "main" host
    + main host packet data retransmitted over separate frequency to end hosts
    + if you have data to send, send it
        + if what comes back on a different frequency isn't what you send, you
          know there was a collision
    + if your transmission collides, retry later
    + properties
        + simple, somehwat robust against failure of a host
        + distributed: hosts control protocol
        + low-load effectiveness
        + high-load can result in wasted packets
    + improve performance
        + listen for activity before sending
        + after collision, wait time based on load
+ CSMA/CD Protocol
    + all hosts can transmit and receive on shared channel
    + sending
        + carrier sense: check that line is quiet before transmitting
        + collision detection: ASAP, if detected stop transmitting, wait
          transmission time, then reset sending process
            + garbled messages
            + voltage level of wire increased, busy, etc.
            + collisions propagate down the line, so all hosts will hear it
        + make sure packet size >= 2L/C
            + transmissions have to take long enough such that a collision will
              always propagate back to the sender before the sender finishes
              sending the packet

#7-6 Ethernet
+ Ethernet Frame format
    + Preamble
        + sequence of 1's and 0's to help clock recovery
    + Start of frame delimeter
        + symbol that packet about to start
    + Destination Address
        + globally unique address
    + Type
        + protocol of encapsulated data
        + IP
    + Pad
        + zeroes to ensure mininimum frame length
        + allows for collision detection
    + Cyclic Redundancy check
        + check sequence to detect bit errors
+ Data rate
    + 10 Gb/second
    + when we increase the ethernet speed, have to maintain P/R > 2L/c for
      collision detection
        + make P larger
        + make L smaller
        + solution
            + keep packet size the same
            + limit L
+ faster and faster
    + MAC protocol: framing structure and how we decide to send packets stayed
      the same
+ Ethernet Switching
    + hubs to switches
    + every time a packet is sent, all links attached to hub will be busy
        + can't have simultaneous sends even if packet's required links are
          separate
    + this became a switch
        + packet flows to switch
        + switch checks ethernet address, compares to table
        + forwards packet
+ Ethernet Switch
    + forward based on header of ethernet frame
        + if destination address isn in table, forward frame to correct output
          port
        + if not, broadcast to all ports except the port packet arrived from
    + table entries are learned based on source addresses of arriving packets
    + Spanning Tree Protocol creates loop free topology
+ Summary
    + MAC(medium access protocols)
        + random: simple
        + deterministic
        + CSMA/CD has became the most popular protocol
    + CSMA/CD eventually replaced by Ethernet switching
        + switches pretty much emulate routers

#7-7 Wireless is Different
+ using electromagnetic spectrum, shared medium unlike a wire
+ Access Point Networks
    + computer with wifi, connects to some access point which connects to the
      internet
+ Not a wire
    + radiates over space, not through a copper wire
    + outward in a sphere
    + signal strength degrades with distance
        + r^2 or faster
        + twice as far from a transmitter, at a quarter of the strength
    + uncontrolled medium
        + signal strength changes over time
        + interference from other transmitters
+ Signal Strength
    + obstructions (metal plate, etc.) can weaken signal
    + wireless signals reflect
        + multipath: echoes in a canyon
            + receive signal in multiple paths/reflections with different delays
    + no perfectly uniform antenna
        + move to the left, suddenly signal strength drops
    + world is constantly changing
+ Overview
    + wireless networks are the last hop for personal communications, but don't
      work as well
    + signasl weaken over distance, environment, etc.
    + different behavior results in different protocols and algorithms

#7-8 MAC
+ Wireless media access control
+ Basic MAC(media access control)
    + arbitrate control of channel
    + one node should use 100%
    + multiple nodes should receive fair share
    + one send at a time
    + high utilization under contention - medium used heavily with lots of nodes
+ Ethernet CSMA/CD
    + on transmission
        + n = 0
            + if idle, transmit
            + if busy, wait until its idle for 96 bit times
    + during transmission
        + if no collision detected, wait 96 bit times, accept next frame for
          transmission
        + if collision detected
            + send jam signal so everyone knows of collision
            + back off for certain amount of time
            + check channel again later
+ THIS APPROACH DOESN'T WORK WITH WIRELESS
    + transmitter will hear its own signal at a really high signal strength
        + can't necessarily hear what's happening at receiver
        + signal strength varies
        + with wires, we know sender and receiver will hear
    + collision detection doesn't work the same way
        + A -> B <- C
        + this is a "collision"
        + A can't hear about C's signal, since A's own signal is "shouting"
          because it's so strong

#7-9 CSMA/CA
+ carrier sense multiple access/collision avoidance
    + used in wireless networks
+ Link Layer Acknowledgements
    + can't detect if collision at receiver in wireless network
        + need feedback
    + if A -> B, if B receives successfully, sends acknoledgement back
    + so if A -> B <- C and there is a collision
        + B will not send an ACK, didn't successfully receive either packet
    + sender needs feedback from receiver
+ CSMA/CA
    + pick random backoff
    + sense local channel, transmit after backoff
        + listen, if channel idle, transmit
    + if packet not ack'd, back off again, retry
    + otherwise, accept next packet for transmission
+ 802.11 CSMA/CA
    + pick random initial wait period t
    + periodically check channel, if idle, decrement t
        + t = amount of idle time transmitter wants to hear before transmission
    + when t = 0, transmit
        + if packet ack'd, move on to next packet
        + if packet not ack'd, double t
        + if t >= T, drop packet
            + don't block, try to move on to the next one
+ Problem 1: Hidden Terminals
    + say node B is in middle, access point
    + nodes A and B want to transmit to B
    + transmitter is listening to where channel is idle to it
        + can only sense its own state
    + hidden terminal
        + both A and C try to transmit to receiver
        + receiver can hear both
        + A and C cannot hear each other
            + A transmits, B receives
            + C can't hear anything, transmits
            + collision occurs, both lost
+ Problem 2: Exposed Terminals
    + A <- B    C -> D
    + A can't hear C
    + B transmits to A
    + C wants to transmit to D
    + D can't hear B
    + C senses local channal, hears B is transmitting, doesn't transmit
        + D can however, receive something from C since it's not receiving
          anything
    + C is exposed to B
        + someone doesn't transmit when it could
+ Problem 3: Collision or low SNR?
    + A -> B but no acknowledgement
    + is this because of a collision? Other node transmitting at the same time
        + back off, re-transmit again later
    + or is this because the channel lost strength?
        + transmit at slower bit rate
    + still struggling to solve this

#7-10 RTS/CTS
+ request-to-send and clear-to-send
+ use short sequence of control packets to determine if safe to send data
+ RTS: can I successfully send this packet to you?
+ CTS: you are cleared to send to me
    + other nodes can also hear this, know not to send data
+ pushing losses and collisions to control packet exchange
+ Problems with CSMA/CA
    + hidden terminals
        + RTS can't solve this
        + always possible for other ends to not hear CTS
    + exposed terminals
        + RTS won't help with this
    + collision or low SNR?
        + helps
+ Primary problem: overhead
    + RTS AND CTS
    + how long do these packets take?
    + they take a significant amount of time
        + have to be heard by everyone, sent at low speeds

#7-11 WiFi
+ 802.11 Format and Overhead
+ BPSK to 64-QAM are all encoding methods usedBPSK to 64-QAM are all encoding
  methods used
    + can adapt to a huge number of speeds
    + in wired system, signal/noise fixed, so fixed speed is fastest that other
      side can operate at
+ basic challenge
    + suport huge range of extensible bitrates
+ virtual carrier sense
    + duration field tells listeners how long this packet exchange will take
    + so receiver and other nodes know how long it will take
    + use this for virtual carrier sense
        + node will count down while channel is busy
        + so CTS packet can use this
+ virtualize a link
    + given 3 addresses
    + access point act more like a switch
    + send from A1 -> A2 via A3
    + tell access point to send to other link layer address through you
        + give nodes connected wirelessly to AP virtual access to wired network
          sitting behind the access point
+ Overhead
    + control headers take a long time to send vs. data
    + control sequence is 92% of sending time, data is fast
+ Summary
     + basic MAC format
     + backwards compatibility
        + talks about how long packet will last
        + even if you don't know modulation scheme, knows how long packet will
          be
    + MAC control specified in terms of durations
    + virtualize a link - embed additional addresses
    + don't be fooled by 600 Mbps
        + control headers needed for backwards compatibility are a bottleneck
          for spending speed

#7-12 IP Fragmentation
+ break packet that's too big into fragments
    + requester has to take the fragments and re-assemble them
+ Fragmentation and Assembly
    + occur when higher layer's data unit is too large for lower layer
    + fragmentation: take large data unit, break up into smaller units
    + assembly: combine chunks into original data unit
    + examples
        + TCP breaks stream of data up, transmits, then re-assembled
            + end to end - given TCP segment arrives at the other end without
              being split
        + Network
            + IP operates on host to host
            + possible some intermediate node breaks up a packet into IP
              fragments
        + Link
            + take IPv6 packets and breaks them into link fragments
+ IP Fragmentation
    + one node can break fragment IP
    + next node just forwards those fragments along
    + destination re-assembles
+ IP fragmentation
    + IP addresses + indent field identify fragments of a packet
    + more fragments bit = indicate if another fragment is coming
        + MFB 1 in all chunks except last one
    + offset = say where data begins in 8 byte chunks
        + offset * 8
    + avoid IP fragmentation if possible
        + multiple packets results in chances of a dropped packet
+ TCP Fragmentation
    + choose packet sizes so they fit inside IP packets

#7-13 Recap of Lower Layers
+ IP is the thin waist
+ IP runs over many link layers
    + Ethernet, WiFi, DSL, 3G, etc.
+ IP runs under
    + TCP, UDP, RTP
+ Ethernet and CSMA/CD
    + almost universally used for wired networks
    + simple and reliable
    + network learns addresses, send whenever
    + ethernet switches allow for simultaneous connections in a network
+ IP Fragmentation
    + all link layers have a max capacity
    + 1500 default for ethernet
    + MTU: max transmission unit
    + fragmentation may be required for links with different MTU's
        + fragment field to create self-contained fragment datagrams
        + destination hosts re-assemble
        + less common
            + most wired networks use Ethernet today, so no need to fragment
              since 1500 is plenty
+ Wifi has constantly changing link speeds
+ Bit errors and coding: easier error detection
+ Error correcting codes
+ Shannon Capacity: fundamental limit to rate of information transfer
