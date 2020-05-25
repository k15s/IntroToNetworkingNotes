#4-0 Congestion Control
+ flow control: when we try to prevent the sender from overwhelming the receiver
+ congestion control: extend notion to entire network as a whole - don't
  overwhelem the links, routers, etc.
    + spread pain evenly
    + make sure all flows controlled in equal ways
+ TCP controls congestion
    + sender can control how many packets it puts into the network
    + end to end approach!
    + AIMD: additive increase multiplicative decrease

#4-1 Basics: what it is, time scales, fairness
+ a packet switched network will always deal with congestion - we have to keep
  the network from collapsing
+ control happens in TCP
+ what is congestion?
    + two packets collide at router
    + flows using up all link capacity - combined flow rates may go over the
      link rate, resulting in dropped packets and buffer overflow
    + too many users using a link during a high traffic time of day
+ what causes congestion?
    + say we have two senders, one switch, one outgoing link
    + senders and links have same rate
    + arrival rate > departure rate resulting in dropped packets
+ congestion is unavoidable
    + packet switching makes efficient use of links -> statistical multiplexing,
      so links aren't "divided up wastefully"
    + if packets are dropped, waste of upstream resources and re-sending may
      make things worse
    + bottlenecks make distribution complicated
+ fairness and throuput
    + tradeoff between fairness and throughput
    + want to maximize overall throughput while also being fair to ALL the links
      and their rates
    + max-min fairness
        + try to max rates of litte flows
        + you cannot increase rate of one flow without decreasing rate of
          another flow
        + links that share a bottleneck will have an equal share
        + single link
            + 3 incoming links with rates 0.5, 1, and 0.2
            + 1 outgoing link with rate 1
            + ideally, each links gets 1/3
            + 0.2 remains the same, doesn't want/need more
                + leaves 0.8 left for the other 2 links = 0.4 for both ideally
            + 0.5 > 0.4, so truncate it to 0.4
            + 1 > 0.4, so truncate it to 0.4
            + if we increase rate of any of them, it would be at the expense of
              a slower flow
+ congestion control goals
    + maximize throughput - keep links busy and fast flows
    + max-min fairness: balance between throughput and fair flow treatment
      through bottleneck
    + respond quickly to changing conditions
    + distributed control: no central arbiter, store things in distributed
      fashion

#4-2 Approaches: network vs end host, max min fairness, AIMD
+ example: fair queueing scheduler at every switch and per-flow queues
    + each flow gets its on queue at each router, FQ distributes from these
      queues
    + not responsive though - simply divides up links, but doesn't tell sources
      how many packets to send
    + packets can still drop from buffer overflow
+ we need a way to tell the sources what rate of packets they should send
+ Network-based
    + explicit feedback from routers to indicate congestion
    + Example
        + A -> B
        + imagine flows passing through switches between A and B
        + switch can tell A there is congestion in the networking, reduce
          sending rate
        + what would we send?
            + perhaps a few bits as a signal
        + how would it get there?
            + piggyback on packets going in one direction to be sent back the
              other way via acknowledgements
+ But do we need the network to provide congestion control? 
    + End-host based/end-to-end principle
    + observe behavior of network to determine rate to send at
        + doesn't depend on switches/routers, won't have to change network as we
          scale
    + if packet is dropped, A will observe this in 2 ways
        + timeout
        + sequence of acks coming back from B
+ TCP has to do this - implement congestion control at end host
    + react to observable events, particularly packet loss/drops
    + exploit sliding window
    + try to figure out how many packets it can savely have outstanding in the
      network
    + IP offers no indication of congestion
+ Sliding window
    + window size < round trip time of sent packet
        + sliding window has to pause to wait for ACKs to come back
    + window size = round trip time of sent packet
        + sliding window never has to pause
        + better use of network
+ TCP varies num packets by varying window size
    + window size = min(receiver, congestion window)
    + congestion window calculation
        + AIMD: additive increase, multiplicative decrease
        + if a packet is received correctly
            + window size = window size + 1/W
            + slowly increases if things are going well
            + therefore, window size = window size + 1 for every complete window
              of packets received
            + small "staircase" - step length based on round trip time of packet
        + if packet dropped
            + window size = window size / 2
            + uses this as congestion signal
            + window size more extremely than increase
    + varying sliding window size varies number of packets in network from
      source

#4-3 Dynamics of a single AIMD flow
+ varying sliding window size varies the number of outstanding bytes, 
  controlling congestion
    + end host only - no explicit network support
    + probe how many bytes the network can hold
    + really, changing the window size manages the buffer use
        + fewer outstanding packets -> lesser buffer use
    + regardless of window size, the bottleneck link is ALWAYS busy
    + sawtooth TCP flow is normal!
+ AIMD keeps RATE CONSTANT
    + number of packets outstanding in network != rate
    + rate = window size / packet round trip time
    + window is just probing how many more bytes it can put into the network
      without buffer overflow
+ utlization can drop when the window size shrinks
    + what if we want to maximize utliazation even when this happens?
    + we can't let the buffer occupancy of switch drop to 0
+ streaming HD vid at 10 Mb/sec
    + details
        + packets are 250 bytes
        + min ping time is 5 ms
        + AIM reaches steady state and sawtooth oscillates between min and max vals
        + buffer is perfect - never goes empty
    + smallest AIMD window value
        + (10 Mb/sec)(0.005 sec) = 0.05 Mb
        + (0.05 Mb)(1,000,000 bits / Mb) = 50,000 bits
        + (50,000 bits) / (8 bits / byte) = 50000 / 8 = 6250 bytes
    + largest AIMD window value
        + buffer and bottleneck link are full, so doubled RTT = 10 ms
        + (10 Mb/sec)(0.01 sec) = (0.1 Mb) (1,000,000 bits/Mb) = 100,000 bits
        + 100,000 bits / 8 = 12,500 bytes
    + how big is the packet buffer in router 
        + when full, buffer holds minimum RTT
        + therefore, 50,000 bits or 6250 bytes
    + after a packet is dropped, how long does it take for the window to reach
      the max value
        + packets are 250 bytes = 2000 bits each
        + window size increases by 1/W each each ACK
        + window size increases by 1 when entire sliding window W is acknowledged
        + so each additive increase every RTT time is by 1 packet = 2000 bits
        + 50,000 bits full buffer / 2000 bits = 25 increases or W++'s
        + so 25 RTTs will do the trick
        + 25 RTTs * avg(RTT) = 25 * 7.5 ms = 187.5 ms
+ what if ping/RTT time is now 250 ms
    + network will runs at 10 Mb/s
    + how big should router buffer be?
        + min RTT = 250 ms = 0.250 seconds
        + (0.250 seconds) (10 Mb / s) = 2.50 Mb = 2,500,000 bits
        + so buffer must at least be 2,500,000 bits
    + if a packet is dropped, how long does it take for window to reach max
      value?
        + as before, window increases by 2,000 bits with each ack
        + min RTT = 250 ms
        + max RTT therefore is 500 ms, since it's twice as long
        + average RTT = 375 ms
        + filling the buffer takes 2,500,000 bits / 2,000 bits = 1,250 single
          steps, meaning an entire sliding window was ack'ed
        + 1250 steps * 357 ms/RTT = 468750 ms = 468.75 seconds

#4-4 Multiple AIMD Flow
+ multiple flows passing through a single router
    + RTT is quite constant with many, many flows
    + aggregation of multiple flows results in much smoother oscillations
    + drops happen at more random times depending on which flow encounters
      congestion
+ sending rate varies with window size
+ AIMD sensitive to loss rate
+ AIMD penalizes flows with long RTT's
    + can take a very long time to climb back up additive increase for long
      RTT's
    + after a dropped packet, takes a long time to regain rate

#4-5 TCP Tahoe
+ how is AIMD achieved?
    + maintain congestion window - estimate of number of unacknowledged segments
      that connection can have outstanding in network
    + every RTT, TCP increases congestion window by 1, AI
    + detects a loss, halves window
+ when should TCP send new data? use a congestion window to limit transmissions
    + TCP has a window field in its header to communicate flow control sizes
      between end hosts
    + originally, TCP would send a full window of segments
        + set retransmit timer for each packet
    + but what if a window > network support?
        + if a bottleneck can only queue a few packets, packets will be dropped
+ TCP Tahoe: 3 improvements
    + Congestion window
        + flow control is all about what the endpoint can handle
            + upper bound for what data can be sent if endpoint can handle more
              than the network can support
        + TCP Tahoe estimates congestion window of network
        + Sender window = min(flow window, congestion window)
            + don't send more than what the receiver can handle
            + don't send more than what the network can handle
        + managing the congestion window, create 2 states
            + slow start state: timeouts, packet drops, etc.
            + congestion avoidance: AIMD policy
    + timeout estimation
    + self-clocking
+ slow start
    + congestion window is max segment size
    + with each single ACK, increase congestion window by 1 max segment size
    + congestion window grows exponentially: 1 -> 2 -> 4 -> 8...
        + much faster than additive increase
    + called this since it's slower than the old approach of sending everything
+ congestion avoidance
    + increase by max segment size^2 / congestion window for each ack
    + linear/additive increase
    + basically, increase by 1 with an entire segment worth of ACKs
+ State Transitions
    + two goals
        + slow start rapidly finds network capacity  by sending a lot of packets
          very quickly
        + congestion avoidance carefully probes capacity
    + 3 signals for transitions
        + increasing acknowledgements: trasnfer going well
        + duplicate acknoledgements: delays/losses
        + timeout: we screwed up big time
    + connection starts in slow start state
    + with each new ack, increase the congestion window
    + when the congestion window size passes a threshhold, change states
    + on a timeout or triple duplicate ack, go back to slow start
        + decrease size of congestion window
        + value is tracked for future threshhold calculation
+ Sender maintains the congestion window

#4-6 TCP RTT Estimation (and self clocking)
+ When should you send data retransmissions?
+ Timeout Estimation
    + timeouts too long results in stalls waiting for ACKs
    + timeouts too short result in quick dropoffs
+ RTT is highly dynamic and can vary based on load
    + as a flow sends packets faster, queues can fill faster, increasing RTTs
    + other traffic can also increase RTT
    + track variance and weighted moving average
+ When should you send acknowledgements? with as little delay as possible
+ Self-clocking
    + send acks aggressively as soon as it receives data segments
    + bottlenecks will space them out by itself
    + TCP only puts new packets into the network as it receives acks
    + this keeps the number of outstanding packets in the network stable

#4-7 TCP Reno
+ improvements to Tahoe
+ fast retransmit
    + soften response to packet loss
    + if a Tahoe receives 3 duplicate ACKs, it assumes the next segment was lost
    + retransmit immediately without a timer
+ fast recovery
    + when a loss is detected by 3 duplicate ACKs, don't drop into slow start
      state which reduces congestion window to 1
    + instead, half the congestion window size
    + this doesn't exit the congestion avoidance state
    + this follows the AIMD policy
+ fast recovery 2
    + inflate congestion window by 1 for each duplicate ACK
    + in theory, the ACK means packets left the network, so more data may be
      sent
+ TCP Tahoe: on timeout or triple duplicate ACK
    + threshhold = congestion window / 2
    + congestion window drops to 1
    + retransmit missing segment
+ TCP Reno: on timeout 
    + behaves identically, congestion window drops to 1, etc.
+ TCP Reno: on triple duplicate ACK
    + set threshhold to congestion window / 2, staying in congestion avoidance
      state
    + inflate congestion window by 1 for each duplicate packet
    + fast retransmit

#4-10 Congestion Control
+ Flow control is about the end hosts not overwhelming each other
+ congestion control is about preventing source hosts from overwhelming the
  network
+ TCP will always lead to some packets being dropped
    + feedback signal to determine how to maximize network use
    + keeps packet drop rate low, links full, and high throughput
+ AIMD
    + we want the network to be fair - max min fairness
    + TCP can control number of outstanding packets in network
    + increase congestion window size by 1 per RTT
    + one drop = halved number of bytes that can be outstanding in network
+ TCP endpoint maintains congestion window
    + don't put more packets than what the receiver and the network can handle
+ slow start and congestion avoidance
    + maintain size of congestion window
    + quickly find right congestion window size
    + AIMD to probe
+ RTT running average and variance to check how long it takes to receive ACK
+ self clocking
    + only send a new packet when it receives an ACK or timeout
    + only send when a packet has let
+ fast retransmit: keep making progress even with duplicate acks
+ fast recover: cut congestion window in half, stay in congestion avoidance/AIMD
+ window inflation: don't waste an RTT waiting for ACKs
