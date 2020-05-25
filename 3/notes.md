#3-0 Packet Switching
+ each packet is a self-contained unit of data
+ efficient: keeps link busy whenever work is being done
+ packets traveling between the same two hosts may encounter delays
    + different paths, different queueing delays
+ 3 main delays
    + packetization delay, propagation delay, queueing delay

#3-1 History of Networks and the Internet
+ look it up...

#3-2 What is Packet Switching
+ Circuit Switching
    + dedicated wires for phone calls, circuit switches set up circuits from our
      phone to the other
    + each call has a private, isolated data rate from end to end
    + establish, communicate, close
    + virtual private wire with dedicated circuit
    + problems
        + inefficient: computers are bursty, times of silence result in
          dedicated circuit that no one else can use
        + diverse rates: computers communicate at many rates
        + state management: we have to maintain state for every communication
          going on, if one fails, change state to re-route calls, then clean it
          up
+ packet switching
    + no dedicated circuit - just send packet when ready (header contains
      destination)
    + routed one hop at a time from source through switches to destination
    + each packet looks up address in forwarding table
        + see address, go to next switch
    + lots of different types in internet
        + routers: deal with internet addresses
        + ethernet switches
    + multiple packets
        + each switch routes one packet at a time, regardless of how many
          packets are in the network
        + packets share full capacity of link
    + switches have buffers
        + if two packets arrive at the same time, the switch has to hold both
        + hold one, send the other
        + this buffer has to be able to hold many packets
    + routers don't maintain per-communicate state, just the forwarding table

#3-3 Terminology, End to End Delay and Queueing Delay
+ Useful definitions
    + propagation delay: time it takes for single bit to travel a link at
      propagation speed c
        + time = l / c, where l is length and c is speed
    + packetization delay: time from when first bit of packet is put on link to
      the last bit of packet is put on link
        + time = p / r, where p is num bits and r is number of bits/s placed on
          wire
        + determined by how close together we can pack bits together and how
          fast the data rate is
        + 64 byte packet, 5.12 microseconds, 100 Mb/s link
            + (64 * 8) / 100 * 10^6 = 5.12
        + 1 kbit takes 1.024 seconds over 1 kb/s link
            + 1024 bits / 1000 bits per second = 1.024 seconds
        + only a function of length of packet and rante at which we can put bits
          on the link
            + length of link doesn't matter, nor does bit speed
+ End to end delay
    + time it takes for packet to go from source to destination, first bit
      leaves and last bit arrives
        + so FOCUS ON WHEN THE LAST PACKET/BIT ARRIVES, especially when sending
          many consecutive packets
    + sum(p/r + l/c) = sum(packetization delay + propagation delay)
        + sum up all these pairings for all the switches
    + packet buffers also change the delay
    + sum(p/r + l/c + Qi)
        + sum(packetization delay + propagation delay + queueing delay)
+ Queing delay
    + unpredictable, random variable - depends on other users in the network
    + the other two types of delays are fixed...

#3-4 Playback Buffers
+ real-time applications have to handle this
    + can't be sure if they'll have video/voice sample from packets to deliver
      to viewer
    + playback buffer
        + gray line to the right of the current point in video
        + packets that have been buffered but not yet played to user
    + client tries to build up this playback buffer in case packets are delayed
    + how far ahead do we want to let the buffer run?
        + longer, more variability and things can change/go wrong
        + shorter, may run out of packets
+ as video packets are passed through switches/routers, queues collect packets
    + focus on queueing delay in each switch
    + variable delay between when byte is sent by server and when byte arrives
      to laptop
        + depends on queueing delay encountered by each individual packet as it
          passes through the switches
    + lower bound of delay
        + end to end delay cannot be lower than packetization + propagation delay
    + upper bound of delay can be huge and not useful, varies depending on 
      queueing delay
    + playback rate
        + separate from rate at which bytes are received by laptop
        + sent -> received -> played back
        + so from received -> playback = buffer size/time
            + how long the packet sat in the playback buffer before finally
              playing back
        + playback rate is constant, unlike the rate at which bytes are
          received
        + if the playback rate intersects with the rate at which bytes are
          received
            + client freezes screen and waits for bytes to accumulate
            + rebuffering: make the buffer bigger!
        + playback buffer
            + abstract queue that contains packets 
            + queue size based on amount of time from variance in delays
+ playback buffer -> video decoder -> screen
    + client picks playback point within the buffer
+ applications have to estimate delay, set the playback buffer, and resize it if
  there are delays

#3-5 Simplistic Deterministic Queue Model
+ router queue
    + routers have to have queues in their interface to hold queues during
      congestion
    + at any one time, Q(t) is number of bytes in the router
    + Q(t) = A(t) - D(t)
        + arrived - departed
    + delay through the queue = horizontal difference between A(t) and D(t)
        + so basically, amount of time spent in queue by particular byte
+ small packets can shorten delay
    + why not send the entire message in one packet?
    + multiple packets means parallelism, sort of like pipelining

#3-6 Queueing Model Properties
+ useful queue properties
    + can think of a network as a set of queues connected by links
    + when arrival processes are RANDOM events
    + don't worry about the math or details, but intuition is important
+ burstiness/bursty arrivals increase delay
    + assume evenly spaced/paced departures
    + evenly spaced arrivals, 1 packet every second
        + 10101010...
        + long periods of 0, short periods of size 1
        + so Q(T) occupancy <= 1
    + bursts of arrivals, n arrivals in a burst every n seconds
        + Q(t) = after first burst [0, n]
        + size decreases to 0 with removals right before next burst
        + Q(t) <= 5, higher average and variance queue size
+ determinism minimizes delay
    + random arrivals wait longer on average than simpler, periodic arrivals
+ Little's result
    + L = lambda * d
    + average size = average arrival rate * average delay of customer in queue
    + if none lost or dropped and regardless of arrival process
+ Poisson process: an arrival process is Poisson if...
    + Expected[num arrivals in interval t] = arrival rate * t seconds
    + successive interarrival times are independent
        + next arrival is independent of prior one
    + Why?
        + model aggregation of independent, random events very well
    + Be warned
        + network traffic is bursty
        + PACKET ARRIVALS ARE NOT POISSON
        + but it models arrival of new flows quite well (many users, not
          individually)
+ the M/M/1 queue
    + simplest type of queue
    + M = Markovian arrival process, in our case Poisson
    + M = Markovian service process
    + exponentially distributed, independent time to serve packet

#3-7 Switching and Forwarding
+ packet switch
    1. lookup address
        + figure out where it's going next via forwarding table
    2. update header
        + decrement TTL, update checksum, etc.
    3. queue packet
        + into buffer memory
    + can have multiple inputs and outputs, but multiple packets passing through
      a single output results in queueing in the buffer
+ ethernet switch (specific packet switch)
    + examine header of each arriving frame
    + if destination address is in forwarding table, forward frame to right
      output port
    + if destination address is not in table, broadcast frame to all ports
      except the one that packet came through in
        + flood it to everyone in hopes it'll reach destination
    + entires in table are learned by examining the source address of arriving
      packets
        + first come through, DA is not in table, is broadcast to everyone
        + if it gets a response, it "remembers the destination" and puts it in
          the table
+ internet router
    + examine IP version and length of datagram
    + decrement TTL, update IP header checksum
    + check if TTL is 0
    + check forwarding table
    + create new ethernet frame then send it
+ Operations
    + lookup address
        + store addresses in hashtable
        + ethernet addresses based in Hex
            + store addresses in hash table
            + look for EXACT match
        + longest prefix matching based on IP addresses
            + implementation: binary trie
            + left branch if 0, right branch if 1
#3-8 Switching and Forwarding (2)
+ output queued packet switch (simplest and slowest)
    + queues that hold packets are on the output half of the switch
    + it's possible that all incoming packets want to go to the same port
    + worse case: all incoming packets want to go to same queue/output, so 
      output rate of (N + 1) * R
        + N is number of packets, R has to do with rate of writing into queue,
          sending it out...
        + this is BAD! N can be huge!
    + output queued switches are limited by this problem - they need memory that
      can run very quickly
    + work conserving: never idle or internal blocking
    + throughput maximized, lines always busy
    + expected delay is minized since we're always doing work on outgoing lines
+ Input Queued Packet Switch (most scaleable)
    + queues that hold packets are on the input/lookup/header change half of switch
    + good news: seems to work the same
    + better news: buffer memory now only has to accept one packet from input
      at a time
    + PROBLEM
        + head of line blocking
        + if you have multiple queues blocked by packets that all want to go to
          the same output
        + can only send one packet out at a time, so other packets behind the
          queue are stuck
    + virtual output queues can solve head line blocking
        + each input maintains separate queue for each output

#3-9 Rate Guarantees
+ shortcomings of queues and strict priorities and guaranteed flow rates
+ output queued packet switch
    + lookups and header update on left, queues are with outputs on the right
    + output queue is FIFO/free for all - whoever sends the most packets will
      receive the highest usage of output link
    + no way of distinguishing priorities besides order
    + can't say anything meaningful about flow
+ alternatives to simple FIFO?
    + strict priorities
        + give each flow a priority
        + high priority queue and low priority queue
        + as each packet arrives, route it to one or the other queue based on
          header
        + scheduler sits at output - always takes packets from high priority
          queue first before low priority traffic
        + high priority traffic unaffected by low priority traffic - like a
          private network
        + this can starve low priority network - only use if small amount of
          high priority traffic
    + rate guarantees
        + give guarantee rate/fraction of outgoing link to each flow
        + weighted fair queueing
        + calculate finishing time of packets bit by bit for balanced fairness

#3-10 Delay Guarantees
+ special techniques that use rate guarantees
+ delay guarantees
    + control delay of packets
    + we know rate at which a queue is served, size of each queue
    + classify packets, place them in queue
    + how do we make sure no packets are dropped from an overflowing queue?
    + limit number of bits that can arrive in a period of time so that the
      buffer never overflows
    + leaky-bucket constrained/regulator
        + packets arrive and go into the packet buffer
        + routers run WFQ
        + only take packets out of buffer only if there are enough tokens in the
          bucket
        + tokens made available at a special rate
        + bucket constrains the traffic in sender
        + once a packet is sent, tokens are used up and must be replenished by
          rate
        + we're only allowing for bursts up to a certain value, giving us a long
          term guarantee
    + RSVP 
        + this protocol populates these values in the first place
        + end to end control system intsalls these rates and values in the path
        + Buffer size > rate * time for packet through buffer
+ this isn't used often
    + requires tons of coordination
    + in most cases, over-provisioning and priorities work well

#PROBLEM 3-10D  (1/1 points)
A video conference application wants to keep the end-to-end packet delay below
200ms, and at a rate of at least 1Mb/s. The packets are all 10,000bits long and
pass through 9 routers (and therefore across 10 links), and all the links are
200km long and run at 10Mb/s. At each router along the path, the video packets
are placed into their own queue, which is served at a guaranteed minimum rate.
Considering all the packetization, propagation and queueing delays in the system,
how much buffering should be in each router, in terms of packets? You can assume
each router has the same amount of buffering, and the speed of propagation is
2 x 10^8m/s.

+ packetization delay
    + 10,000 bits / (10 * 10^6 bits / second) = 0.001 second = 1 ms
    + 1 ms * 10 links = 10 ms over all links
+ propagation delay
    + 200,000 m / (2 * 10^8 m / s) = 0.001 seconds = 1 ms
    + 1 ms * 10 links = 10 ms over all links
+ end to end = sum(packetization delay + propagation delay + queueing delay)
+ 200 end to end delay = sum(10 ms + 10 ms + queueing delay)
    + 180 ms = total queueing delay
    + 180 ms / 9 routers = 20 ms delay per router to meet end to end requirement
    + 20 ms / (1000 ms / s) = 0.020 seconds
    + 0.020 seconds * (1 MB / second) = 20,000 bits held in router can be served
      at proper rate to meet end to end requirement
    + (20,000 bits) / (10,000 bit packet) = 2 packets in router

#3-11: Packet Switching
+ simple in that each packet is self-contained to reach its destination
+ lazy: keeps link busy if work is done, rather than dedicating capacities to
  apps/users
+ simple forwarding time with no flow state
+ delay
    + write bits in packet to link - packetization delay
    + bits have to propagate down cable or through air, based on distance /
      speed of propagation
    + variable queueing delay
        + routers have to have buffers to hold packets during congestion
        + depends on how busy the network is
        + wireless links are NOISY
+ playback buffers for streaming data
