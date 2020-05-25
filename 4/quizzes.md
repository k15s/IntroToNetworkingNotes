#PROBLEM 4-1A  (1/1 points)
Which of the following statements about congestion control are true?

+ A high packet drop rate can cause TCP throughput to collapse.
Congestion control prevents packets from being corrupted.
Congestion control ensures packets are delivered to the correct destination.
+ One of the goals of a congestion control algorithm is to share the bandwidth of
  a bottleneck link fairly across competing flows.

#PROBLEM 4-1B  (1/1 points)
Which of the following statements are true? Choose all correct answers.

+ Congestion collapse is undesirable because it leads to many packets being
  dropped in the network.
+ Congestion control is built into TCP.
Congestion control is built into IP.
Congestion can only happen at timescales of a few hundred round trip times.

#PROBLEM 4-2C  (2.0/2.0 points)
Which of the following statements are true? Choose all correct answers.

Max-min fairness is the only notion of fairness.
Max-min fairness always divides link bandwidth equally among flows traversing it.
+ An allocation of rates to flows that is max-min fair may not maximize the total
  throughput of flows in the network.
    + truncation...
+ In a network with only one router and for which every flow can saturate the
  bottleneck link on its own, max-min fairness divides link bandwidth equally
  among flows traversing the bottleneck link.

+ EXPLANATION

+ Answer: C,D. (A) is not true. Max-min fairness is not the only notion of
  fairness. As discussed in the video, it may be desirable to reduce the rates
  of flows that traverse multiple links. (B) is also not true. For example,
  consider two flows on a 10Mb/s, one of which is a video stream that never
  exceeds 1 Mb/s. In this case, the other flow gets 9Mb/s.

#PROBLEM 4-2D  (1/1 points)
Consider 10 flows passing through an fair queuing router with an outgoing link
running at 100Mb/s. Five of the flows are part of a file backup service and can
each fill the link if they are allowed to. The other five are video streams
running at 2Mb/s. If this router is the bottleneck router for all the flows, how
fast do the flows operate?

All 10 run at 2Mb/s.
All 10 run at 10Mb/s.
The five video flows run at 1Mb/s, the other five run at 10Mb/s.
+ The five video flows run at 2Mb/s, the other five run at 18Mb/s.

+ ideally, 100 MB/s / 10 flows = 10 MB/s per each incoming flow
+ the 2 MB/s video flows don't ask for any more and can easily get max
  throughput for their streaming rates
    + each leaves 8 MB/s on the table for re-distribution
+ video takes up 10 MB/s out of the 100 MB/s outgoing flow, leaving 90 MB/s for
  the 5 backup flows
+ 90 MB/s / 5 flows = 18 MB/s per each backup flow

#PROBLEM 4-3A  (1/1 points)
Which of the following statements are true about AIMD and sliding windows?

In the congestion avoidance phase, every time a packet is successfully
acknowledged, the congestion window is increased by one.
+ In the congestion avoidance phase, every time a complete window of packets is
  successfully acknowledged, the congestion window is increased by one.
AIMD avoids congestion collapse by never varying the window size.

#PROBLEM 4-3B  (1/1 points)
Which of the following statements are true about AIMD for a network with a
single flow?

The AIMD window size is constant.
+ If the bottleneck router has sufficient buffers, the sending rate is constant
  (W/RTT).
+ The sawtooth is the stable operating point at which the sending rate is
  constant.
The sawtooth of AIMD means the sending rate is varying constantly

#PROBLEM 4-3C  (3.0/3.0 points)
Consider a long flow being transmitted in 1 kB packets by a transport protocol
that (only) uses AIMD to control the window size. Initially, the window size is
10 kB. Every time the window opens to 20 kB, the last packet in the window is
dropped, which is detected exactly after one round-trip time, and retransmitted.

How many packets are sent up to and including the first packet that is dropped?
Hint: 10 packets can be sent immediately. Then the window size will increase by
one. Start by calculating the window size of each round up until the packet is
dropped.

+ initial window: 10 packets sent
    + new window = 11 packets wide
+ 11 packets sent
    + new window = 12 packets wide
+ 12 packets sent
    + new window = 13 packets wide
+ ...
+ 19 packets sent
    + new window = 20 packets wide
+ 20 packets sent
    + last packet dropped, but 20 were still sent!
+ 10 + 11 + 12 + ... + 19 + 20 = 165 sent packets

How many packets are sent in total up to and including the second packet that is
dropped? Hint: The second round will be the same. Don’t forget about the
retransmitted packet.

+ 165 sent before first drop
+ drop means window size = window / 2 = 20 -> 10
+ 165 sent before second drop
+ 165 * 2 = 330

If the round trip time is a constant 10ms, how long does it take until the last
packet is transmitted of a 16,500 kB file? Express your answer in seconds.

+ 16,500 kB file = 16,500 kB packets to send
+ we know 165 packets are sent until a drop, call this a "round"
    + window size: 10 -> 20
    + 10, 1 RTT, now 11
    + 11, 1 RTT, now 12
    + ...
    + 18, 1 RTT, now 19
    + 19, 1 RTT, now 20
    + 20, 1 RTT, drop, back to 10
    + 11 RTT's in one round of 165 packets
    + 11 RTT's * 10 ms = 110 ms per each "round"
+ 16500 total packets / 165 sent packets in a round = 100 "rounds"
+ 100 rounds * (110 ms / round) = 11,000 ms
+ 11 seconds

#PROBLEM 4-4A  (4.0/4.0 points)
Source A communicates to destination B using a flow controlled by AIMD. The RTT
is 10ms and the bottleneck link operates at 100Mb/s. The queue in the bottleneck
router can hold up to 5Mbits of packet data. Assume all data packets are 1Kbytes
long.

If it is the only flow in the network, what is the sending rate (in Mb/s)?

+ 100
+ EXPLANATION
+ If there is only one flow, and if the buffer is at least RTT*C (in this case
  it is: RTT*C = 10ms * 1e8 = 1e6 bits), then it will operate at 100Mb/s
  regardless of RTT.

If the queue in the bottleneck router is reduced to ½ its size, will the TCP
flow’s sending rate change? (Write your answer as Yes/No)

+ No
+ EXPLANATION
+ No. It is still larger than RTT*C.

If the queue in the bottleneck router is reduced to 500kbits, will the TCP
flow’s sending rate change?

+ Yes
+ EXPLANATION
+ Yes. The buffer is less than RTT*C, so the queue will go empty from time to
  time and the rate will be reduced.

If there are thousands of flows sharing the bottleneck link and 1 in every
10,000 packets is dropped, what is the sending rate of the flow from A to B?
(Write your answer rounded to one decimal digit in Mb/s)

+ MSS * sqrt(3/2) * 1/(RTT * sqrt(rate of drops))
+ MSS = 1024 bytes = 8192 bits
+ 8192 bits * sqrt(3/2) * 1/(0.01 seconds * sqrt(1/10,000 packets))
+ this gets us 100331099.9 bits / second
+ (100331099.9 bits / second) / (1,000,000 bits / MB)
+ 100.3 Mb/s

#PROBLEM 4-5A  (1.0/1.0 points)
Which of the following statements are true about TCP Tahoe?

TCP Tahoe uses the same mechanism for flow control and congestion control.
+ TCP flow control is intended to prevent a receiver from being overwhelmed with
  packets.
+ Congestion control is intended to prevent the network from being overwhelmed
  with packets.
+ The TCP Tahoe congestion window is an upper bound on the sender’s window size.
In slow start, the TCP sender sets its initial sending window to receiver’s window size

#PROBLEM 4-5B  (1/1 points)
Which of the following statements are true about TCP Tahoe?

A sender in the slow start state increases its congestion window by one MSS for
every packet it sends.
+ A sender in the congestion avoidance state increases its congestion window by
  one MSS for every congestion window (cwnd) worth of data acknowledged.
+ Suppose a sender transmits packets with sequence numbers a, a+1, a+2, a+3,
  a+4. Packet a+1 is lost. All other packets arrive successfully. The sender
  will receive packets with acknowledgment sequence numbers a+1, a+1, a+1, a+1.
A sender receives triple duplicate acknowledgments only when packets are lost.

+ EXPLANATION
+ An out of order packet can also trigger a triple duplicate ACK. (A) is not
  true, as a sender increases its cwnd only after it receives an ACK for every
  packet it sends. (D) is not true, as an out of order packet trigger a triple
  duplicate ACK.

#PROBLEM 4-5C  (1/1 points)
Suppose the network supports a sender up to a window of 16 packets after which
it drops packets. Assume the starting congestion window is 1 packet, and the
ssthresh is 8 packets. How does TCP Tahoe’s congestion window evolve over time?

cwnd = 1, 2, 4, 8, 16, 32, … (and the sequence repeats)
+ cwnd = 1, 2, 4, 8, 9, 10, …, 16, 17, (and the sequence repeats)
cwnd = 1, 2, 4, 8, 9, 10, …, 16 (and the sequence repeats)
cwnd = 1, 2, 3, 4, …, 16 (and the sequence repeats)

+ recall that passing the threshhold results in the state change from slow start
  to additive increase/congestion control

#PROBLEM 4-5D  (2.0/2.0 points)
Recall that self clocking is the process by which a TCP sender uses
acknowledgments from the receiver to trigger transmissions. Why is self-clocking
important?

Without self clocking, the sender will never know when to transmit more data.
+ Self clocking is a way for the sender to know that its packets have left the
  network.
Self clocking gives a TCP sender enough time to accumulate data before it
transmits a packet into the network.
+ A self clocking sender naturally paces out packet transmissions to the
  bottleneck link capacity without using timers.

#PROBLEM 4-6A  (1/1 points)
Why is round trip time (RTT) an appropriate timescale for retransmission?

+ A timeout much smaller than the estimated RTT may result in wasteful
  retransmissions.
A timeout that is roughly equal to the RTT gives enough time to the receiver
application to process data.
+ A timeout that is roughly equal to the RTT is favourable as it gives the
  sender enough time to receive ACKs from the receiver.
+ A timeout much larger than the estimated RTT may result in poor network
  utilization.

#PROBLEM 4-6B  (1/1 points)
Suppose you estimate some quantity V using an exponentially weighted moving
average (EWMA) of periodic measurements of V. Call the estimate "r." There is a
scaling factor alpha (a) which controls how much a new measurement factors into
the estimate, using the function r ← ar + (1-a)m. Let's say you start with an
initial estimate of r = 100. Suppose the true value of V stays constant at 50,
which of the following statements are true? Choose all correct answers.

+ A higher value of "a" means it takes longer time for r to reach 50.
If a = 0.5, it takes 5 samples for r to be within 0.5 of V (i.e. r - V <= 0.5)
+ If a = 0.5, it takes 7 samples for r to be within 0.5 of V.
+ If a = 0, r is always set to the newest sample m.

#PROBLEM 4-6C  (1/1 points)
A TCP Tahoe sender obtains RTT samples that have the following distribution:

50% of the time, the RTT is 100ms
25% of the time, the RTT is 150ms
20% of the time, the RTT is 200ms
5% of the time, the RTT is 500ms
In steady state, what will the sender’s RTT estimate (RTT_est) be?

+ RTT_est ~ 150ms
RTT_est ~ 200ms
RTT_est ~ 350ms
RTT_est ~ 400ms

+ (0.5 * 100) + (0.25 * 150) + (0.2 * 200) + (0.05 * 500) = 152.5

#PROBLEM 4-7A  (1/1 points)
Recall that TCP Reno and Tahoe differ in how they handle a triple-duplicate
ACKs. Reno enters a fast retransmit state while Tahoe re-enters slow start.
Suppose you have a network which supports a maximum window size of 16 packets
before it starts dropping packets. On this network, you first run a TCP Tahoe
flow that slow starts and observe its behaviour as it reaches steady state
(where you see a repetitive pattern of congestion windows emerging).

In this steady state, what is the throughput of the TCP Tahoe flow in
packets/second? Call this value “A.” Assume a constant RTT estimate of 1s, and a
RTT variance estimate of 0.5s.

Now, you run a TCP Reno flow and observe its steady state behaviour. What is its
throughput in packets/second (call it “B”), assuming the same RTT and variance
estimates as above?

Now that you’ve computed A and B, what is the value of B/A? Assume ACKs never
get lost, cwnd/ssthresh are always integers, and queueing delays are negligible.

~0.51
~1
+ ~1.18
~1.52
~1.3

+ Explanation: In steady state, TCP Tahoe will have a ssthresh of 8 packets. So,
  you have a SS phase that transmits 1+2+4+8 packets lasting 4s, and then a CA
  phase transmitting 9+...+16+16|+16 (the 17th packet is the round marked | is
  dropped) lasting 10s. Since you get a triple duplicate ACK, you enter slow
  start, setting ssthresh=17/2 = 8, after which the behaviour repeats. So, you
  transmit 15+100+16+16=147 packets every 14s, which is 10.5 packets/sec.

+ In steady state, TCP Reno will have also a ssthresh of 8 packets, but it
  always stays in CA phase. In the CA phase, you always transmit
  8+9+...+16+16|+16=140 packets which lasts 11s.

+ The train of packets in the 10th second triggers a triple-duplicate ACK in the
  11th second, and you immediately retransmit the packet in the 12th second. 
  Then, during fast recovery, the congestion window is set to
  cwnd / 2 = 17 / 2 = 8. For each of the 17 duplicate ACKs (one ACK from the
  last packet sucessfully received in the 16* window, and an additional 16 from
  the packets received in the following window), cwnd is increased by one. A
  packet is sent once cwnd grows to 18; another is sent once cwnd grows to 19;
  and so on until cwnd grows to 8 + 17 = 25. Thus, a total of
  1 + (25 - 18 + 1) = 9 packets are sent in the 12th second. This whole sequence
  repeats from the 13th second. The throughput in this case is
  149/12=12.42 packets/sec.

+ Thus, B/A ~ 1.18.
