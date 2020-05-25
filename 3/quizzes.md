#PROBLEM 3-2A  (1/1 points)
Packet switching versus circuit switching. Which of the following are true?

+ Packet switching is more efficient than circuit switching for traffic that
  arrives in "bursts."

Packet switching leads to a lower end-to-end delay than circuit switching.

+ Circuit switching leads to a lower variation in end-to-end delay.

+ Circuit switching is used by the telephone network

#PROBLEM 3-2B  (1/1 points)
Which of the following statements are true about packet switching?

+ In packet switching, packets are routed individually, hop-by-hop toward their
  destination.

Packet switching was originally designed as a replacement for the phone backbone.

The Internet chose to use packet switching because it leads to stronger delay
guarantees than the alternatives.

+ In packet switching, packets are held in a buffer during times of congestion.

#PROBLEM 3-2C
In a circuit switched network with 10 hosts, if each pair of hosts can establish
at most one circuit between them, what is the maximum number of simultaneous
circuits?

+ 9 + 8 + 7 + 6 + 5+ 4+ 3+ 2+ 1 = 45

#PROBLEM 3-2D  (1/1 points)
Suppose you have a packet switched network that has ten hosts. You want these
switches to be able to deliver packets to all ten hosts no matter what (working)
topology they are put in: trees, lines, etc. How many entries must each switch's
forwarding table have for packets to be able to reach every end host?

+ 10

#PROBLEM 3-2E  (1/1 points)
Statistical multiplexing is more desirable than fixed-sharing of a link because:

+ Data traffic is bursty and the instantaneous rate of each flow sharing a link
  varies.

+ It allows more efficient use of a link.

guarantees a share of the link for each flow.

Prevents non-statistical uses of a link.

#PROBLEM 3-2F  (1/1 points)
When a voice call (e.g. Skype) is carried over the Internet, the human voice is
digitized at a variable rate. Which of the following are true about packet
switched voice:

+ If the network is congested, voice packets can be delayed in the network while
  packets wait in router buffers for the outgoing link to be free. This can add
  extra delay to the voice call, and can even cause the voice packets to be
  dropped altogether.

Voice calls in the Internet are always lower quality than in the telephone
network.

+ If the network is lightly loaded, then one voice call is unlikely to affect
  another.

The Internet cannot simultaneously carry high quality voice calls and high
quality video

#PROBLEM 3-3A
We wish to send a 150,000 byte message over a network with four hops, each of
length 20km and running at 100 Mb/s. What is the end-to-end delay of the message?
Use speed of light in copper = c = 2 * 10^8 m/s, and round your answer to the
nearest integer millisecond.

+ 4 * [ (1200000 bits / (100 * 10^6) bits/s) + (20 km * 1000 m) / (2 * 10^8 m/s) ]
+ = 0.0484 = 48 ms

#PROBLEM 3-3B
We wish to send a message of size 150,000 bytes over the network. There are four
hops, each of length 20km and running at 100 Mb/s. However, before sending we
split the message into 1500 byte packets. What is the end-to-end delay of the
message? Use speed of light in copper = c = 2 * 10^8 m/s, and round your answer
to the nearest integer millisecond.

+ Think about this delay in terms of the LAST packet reaching the end:
+ Time for this single, final packet to reach destination:
+ 4 * [ (12000 bits / (100 * 10^6) bits/s) + (20 km * 1000 m) / (2 * 10^8 m/s) ]
+ = 8.8 * 10^-4

+ Time passed until last packet is placed on the wire:
+ 99 * (12000 bits / (100 * 10^6) bits/s) = 0.01188

+ since 99 other packets were placed on the wire ahead of it, we have to add them together
+ 0.01188 + 8.8 * 10^-4 = 13 ms

#PROBLEM 3-4A
Consider a video service streaming video over the Internet to a client. If the
time taken for packets to reach the destination varies between 12ms and 128ms
and if no packets are retransmitted, then which of the following are true:
+ To ensure packets arrive before they are needed, the client needs a playback
  buffer that holds at least 128ms.
+ To ensure packets arrive before they are needed, the client needs a playback
  buffer that holds at least 12+128 = 140ms.
+ To ensure packets arrive before they are needed, the client needs a playback
  buffer that holds at least 128-12 = 116ms.
+ Because the network delay is bounded, we don't need a playback buffer.

Hint: Remember the purpose of the playback buffer is to absorb variation in
packet delay.

+ 128 - 12 = 116ms

#PROBLEM 3-4B
Consider two users communicating via a voice-over-IP service over a path with 10
routers and links all running at 100Mb/s. If each router has a 1MByte packet
buffer, and if no packet is lost and retransmitted, what is an upper bound on
the playback buffer needed at the receiver? Express your answer in milliseconds
(rounded to the nearest integer). Assume 1MByte = 2^20 bytes. Consider the
maximum queuing delay introduced by each router.

+ 1 Mb of data builds up at each router, so 1 Mb = 2^20 bytes queue in router
+ 2^20 bytes * 8 = number of bits queued at router
+ 10 routers in path
+ 100 Mb/second links = 100,000,000 bits/second

+ [ [(2^20 bytes * 8) bits / router] * 10 routers ] / 100,000,000 bits/second ]
+ = 0.8388 seconds = 839 milliseconds

#PROBLEM 3-4C
If a voice call has missing data it makes it hard to understand the speaker.
Therefore, we commonly allow time for a limited number of retransmissions before
playing the sound to the listener. If the network path (in each direction) has
total packetization delay of 15ms, total propagation delay of 25ms, and queuing
delay varying between 0ms and 10ms, how large (in milliseconds) does the playback
buffer need to be if we want to allow for one retransmission?

Hint: The maximum RTT of the network is 100ms, so the packet can be retransmitted
if a response has not been heard within 100ms

+ max delay in path: 15 + 25 + 10 = 50 ms
+ min delay in path: 15 + 25 + 0 = 40 ms
+ Variance: 10 ms, so if there were no re-transmissions or packet losses,
  buffer size = 10 ms
+ We have to allow for one re-transmission, so accommodate for that amount of
  time in the buffer size, so 100ms + 10ms = 110 ms buffer size handles RTT's

# PROBLEM 3-4D
Consider a network where there are two paths between your computer and a video
stream server (in practice might be many more). The total distance for the first
path is 2000km and the total distance for the second is 1500km, and the path that
each packet takes is randomly chosen. Assuming that there is no queueing delay,
all the links run at the same data rate, and no packets are lost, how big should
the playback buffer be? (Assume c = 2 * 10^8 m/s)

+ Remember that the playback buffer's size does not depend on the total
  end-to-end delay, but the variance in the delay.
+ Only worry about propagation delays, since assuming no queueing delay, etc.
+ Path 1: 2000 km = 2000,000 m / (2 * 10^8 m/s) = 10 milliseconds
+ Path 2: 1500,000m / (2 * 10^8 m/s) = 7.5 milliseconds
+ variance: 10 – 7.5 = 2.5 millseconds

# PROBLEM 3-5B
Consider a queue with a continuous time arrival process that is constrained so
that in any time interval t1 to t2 (where t2 – t1 = t > 0) no more than (s + rt)
bits can arrive, where s and r are non-negative constants. Any arrival process
is acceptable so long as it meets the constraint above. The output link operates
at a constant rate of 2r. What is an upper bound on the long-term average rate
that bits can arrive to the FIFO?

+ s + rt arrive in total
+ rate = amount arrived / time = (s + rt) / t = s/t + r, but considering when t
  gets huge, r is the upper bound

# PROBLEM 3-5C
Imagine a queue has a continuous time arrival process that is constrained so
that in any interval from time t1 to t2 (where t2 – t1 = t > 0) no more than
(s + rt) bits can arrive, where s and r are non-negative constants. Any arrival
process is acceptable so long as it meets the constraint above. The output link
operates at a constant rate of 2r. What is the maximum occupancy of the FIFO?

+ queue occupancy = arrived - departed
+ (s + rt) - (2rt) = s - rt
+ max ocupancy is s, when t gets larger the occupancy only decreases

#PROBLEM 3-6A  (1/1 points)
Assume 5 packets arrive in a burst to a router at time t=0 seconds, and another
burst of packets arrive at t=10s, and so on every 10 seconds; and if one packet
departs from the router at t=1s, t=2s, t=3s, t=4s, t=5s, and then again at
t=11s, t=12s, … and so on.

What is the maximum number of packets in the router buffer?

+ 5, since the bursts are cleared between each other

#PROBLEM 3-6B  (1/1 points)
(Same as question Q1). What is the minimum number of packets in the router buffer?

+ 0

#PROBLEM 3-6C  (1/1 points)
(Same as question Q1) What is the time average number of packets in the router
buffer?

+ L = avg(arrival rate) * avg(delay)
+ avg(delay) = (1 + 2 + 3 + 4 + 5) / 5 = 15 / 5 = 3
    + since all the packets arrive "at once"
+ L = (5/10) * avg(delay) = 15 / 10 = 1.5

#PROBLEM 3-6D  (1/1 points)
(Same as question Q1) What would be the average number of packets in the router
buffer if the arrival rate was the same (i.e. 5 packets per 10 seconds), but
there was at least one second between each packet arrival?

+ so the packest arrive in a "burst", but each packet arrival is separated by 1
  second
+ so, 1 arrives, 1 removed right as 2 arrives, 2 removed right as 3 arrives, and
  so on
    + removals/arrivals separated by 1 second, so each experiences a one second
      delay
+ L = avg(arrival rate) * avg(delay)
+ L = (5/10) * (1 + 1 + 1 + 1 + 1) / 5 = 0.5

#PROBLEM 3-6E  (1/1 points)
You own a deli which experiences a burst of customers in a 10 minute period. In
the first minutes, 1 arrives; in the second, 2 arrive; in the 3rd, 3 arrive; in
the 4th, 4 arrive; in the 5th, 5 arrive. Then customers stop arriving. The
average number of customers waiting over the 10 minute period is 3. What is the
average delay experienced by each customer, in minutes?

+ L = avg(arrival rate) * avg(delay)
+ 3 = 15 customers/10 minutes total * avg(delay)
+ avg(Delay) = 2

#PROBLEM 3-7B  (1/1 points)
Using longest prefix matching, the IP address 34.21.13.8 will match which entry?

34.0.0.0/8
34.21.11.0/20
34.21.13.0/32
+ 34.21.13.0/24

#PROBLEM 3-7C  (1/1 points)
Which of the following statements are true about a Ternary Content Addressable
Memory (TCAM)? Choose all correct answers.

It is very fast because there are multiple processes running in parallel in a CPU
+ When presented with search data, it examines all the entries in its table
+ It uses a mask to see if an address matches a TCAM entry
It uses specialized hardware
+ EXPLANATION
+ TCAM is a special kind of memory and doesn’t use processors to do the comparisons.

#PROBLEM 3-8D  (1/1 points)
Virtual Output Queues solve which problems:

Ensures that all flows have an equal share of the output link
+ Eliminate head-of-line blocking
+ Only require memory bandwidth of 2 * R instead of (N+1) * R for an output
  queued switch
+ Allows a switch to achieve maximum throughput

#PROBLEM 3-9A  (1/1 points)
Given a FIFO queue of size B Kbits and output rate of R Kbits/s, what is the
maximum delay of a packet passing through the queue?

(2B)/R
B * R
+ B/R = max queue size / output rate
R/B

+ The maximum delay will be B/R; this occurs when a packet arrives and the queue
  is full. The packet will need to wait for the contents of a full queue to
  depart the egress link.

#PROBLEM 3-9B  (1/1 points)
Weighted fair queuing has which of the following properties?

+ allows some packet flows to receive a higher service rate than others.

ensures that control traffic is always delivered sent onto the egress link first.
can be used to retransmit dropped packets.

can provide packet flows with a guaranteed minimum rate of service.

+ By increasing the weight of a queue for a particular set of packet flows, the
  rate of service is increased relative to packet flows in other queues.
  Furthermore, a packet flow can be guaranteed a minimum rate of service by
  creating a dedicated queue.

#PROBLEM 3-9C  (1 point possible)
A network operator decides to use strict priority queuing in their switches. The
policy is that video traffic will use the high priority queue and HTTP web
traffic will use the low priority queue. What are some potential pitfalls to
this strategy?

The video traffic may suffer more delay than a single FIFO queue
+ Users may not be able to load web pages during peak video-watching hours
All video flows will receive an equal portion of the egress link
Video packets will never be dropped

+ The packets in the high priority queue have strict priority over the low
  priority queue, so video traffic cannot suffer more delay than a single FIFO
  queue because it is high priority. On the other hand, if the egress link is
  saturated with video traffic, web traffic will be starved i.e. it will not be
  delivered.

  And giving strict priority to video traffic doesn't mean packets will never be
  dropped (unless the switch buffers are infinite in size, which is not the
  case!)

#PROBLEM 3-9D  (1/1 points)
If a switch maintains two strict priority queues for each output (high and low)
each of size B Kbits what is the maximum delay of a packet in the high priority
queue if the output link runs at R b/s?

(2B)/R
+ B/R
B/(2R)
B * R

+ B/R. The high priority traffic doesn’t see the low priority traffic, so the
  calculation is the same as if the high priority path was a FIFO.

#PROBLEM 3-10A  (1/1 points)
We can control the maximum queueing delay a packet experiences in a switch by
controlling which of these properties?

number of queues, but you don't know the rate of each queue or its depth

+ size of a queue

+ the rate at which a queue is served

+ Decreasing the size of the queue or increasing the rate that the queue is
  served will both serve to decrease the maximum delay. Changing the number of
  queues won’t directly impact the maximum queueing delay.

#PROBLEM 3-10B  (1/1 points)
Where is a leaky bucket used?

+ at the source that is sending packets
at the destination only
at both the source and destination

+ A leaky bucket regulator is used to limit the arrival process to routers so as
  to prevent buffers from overflowing and packets being dropped. This is done at
  the source.

#PROBLEM 3-10C  (1/1 points)
A switch is allowed to delay packets in a given queue by at most 1.8ms. If the
queue is served at 10Mb/s, how big should the queue be (in bits)?

180
1800
+ 18000
180000
Hint: Consider the formula for the maximum delay of a queue

+ The maximum delay of a queue: delay = size / rate. To find the size given a
  maximum rate: size = rate * delay, or 10Mb/s * 1.8 ms = 18000b.
