#PROBLEM 2-2A  (1/1 points)
Which of the following statements are true about the TCP service model?

TCP uses a checksum to determine if the application data follows the right
syntax.

+ TCP delivers a stream of bytes from one end to the other, reliably and 
  in-sequence, on behalf of an application.

Because TCP encodes data with enough information to correct errors, it never
needs to retransmit missing segments.

+ When a TCP arrives at the destination, the data portion is delivered to the
  service (or application) identified by the destination port number.

Because TCP can not trust the IP layer to format packets correctly, it must
include the destination IP address in the TCP header.

#PROBLEM 2-2B  (1/1 points)
TCP is responsible for providing reliable, in-sequence end-to-end delivery of
data between applications. Which of the following are direct consequences of
this design choice? Choose all correct answers.

TCP is self-contained with sufficient addressing information in its header that
it does not need the IP layer, or any other layer beneath it to deliver data
to the far end.

+ TCP will retransmit missing data even if the application can not use it - for
  example, in Internet telephony a late arriving retransmission may arrive too
  late to be useful.

+ TCP saves an application from having to implement its own mechanisms to 
  retransmit missing data, or resequence arriving data.

TCP can only work correctly if the layer below it also delivers data reliably
and in sequence.

TCP requires no connection state to be established prior to the end points
communicating.

#PROBLEM 2-2C  (1/1 points)
Before communication begins, TCP establishes a new connection using a three-way
handshake. This is because:

It is considered polite to shake hands three times when first meeting.

TCP could use a two-way handshake instead, and only introduced the third
handshake to prevent the conversation from being intercepted.

+ TCP end points maintain state about communications in both directions, and the
  handshake allows the state to be created and initialized.

+ TCP establishes a stream of bytes in both directions, and the three-way
  handshake allows both streams to be established and acknowledged.

#PROBLEM 2-2D  (1/1 points)
UDP is a much simpler protocol compared to TCP. It is connectionless and it does
not provide reliable transmission. Which of the following statement about UDP is
true:

No application would like to use UDP, since it does not provide reliable
transportation.

+ Short request-response transfer, such as DNS, would prefer to use UDP to avoid
  TCP overheads, such as three-way handshake.

+ UDP is suitable for applications that don't necessarily need a reliable
  transport (e.g., voice-over-IP, online games)

+ UDP is often used for broadcast communication because it does not require
  per-recipient state (such as acknowledgement numbers).

#PROBLEM 2-2E  (1/1 points)
A server typically listens for TCP connections on a particular port (e.g. 80 for
HTTP or 22 for SSH). Which fields can the server use to distinguish packets for
a particular client connection?

Sequence Number
Acknowledgement Number
+ Source Port
+ Destination Port
+ Source IP
+ Destination IP
Checksum

#PROBLEM 2-3A  (1/1 points)
Which of the following statements are true about the ICMP service model:

+ ICMP messages are typically used to diagnose network problems. 

ICMP messages are useless, since they do not transport actual data.

+ ICMP messages can be maliciously used to scan a network and identify network
  devices.

ICMP messages are reliably transmitted over the Internet.

#PROBLEM 2-3B  (1/1 points)
Which of the following statements are true about the "ping" program:

+ ping can be used to measure end-to-end delay. 

+ ping can be used to test if a machine is alive.

+ ping can be maliciously used as a way to attack a machine by flooding it with
  ping requests.

Every machine on the Internet will reply to ping requests.

+ ping sends out ICMP ECHO_REQUEST message to the destination.

#PROBLEM 2-3C  (1/1 points)
Which of the following statements are true about the "traceroute" program?

traceroute can only work by sending out ICMP ECHO_REQUESTs, it will not work by
sending out UDP packets or TCP packets. 

+ traceroute can be used to figure out network topology.

+ traceroute works by increasing the TTL values in each successive packet it
  sends.

+ traceroute can be used to identify incorrect routing tables

#PROBLEM 2-3D  (1/1 points)
ICMP appears to be useful in diagnosing network problems. What are some problems
it cannot help diagnose? Choose all correct answers.

Test if a network router or host is alive or not.

Detect if a packet expired in the network (TTL reached zero) before arriving at
its destination.

+ Test if a web server is sending correct responses to requests.

+ Know the exact link utilization between two routers to see if it is overloaded
  with packets.

#PROBLEM 2-3E  (1/1 points)
Which of the following ICMP Messages will always have a router’s IP address in
the source field?

Echo Reply
+ Destination Network Unreachable
+ Destination Host Unreachable
Destination Port Unreachable
Echo Request
+ TTL Expired

#PROBLEM 2-4C  (1/1 points)
Which of the following are true regarding the end to end argument?

The network is forbidden from doing anything beyond forwarding packets

+ Correctness can only be ensured at the end points

+ The network can perform optimizations but they are only performance enhancements

The end host’s operating system can verify correctness for the application

#PROBLEM 2-5A  (1/1 points)
Suppose you have a single burst of 14 bit errors (bit flips) within a packet and
no other errors. Which of the following error detection schemes will detect the
packet is corrupted with 100% probability? An 8-bit checksum sums all 8-bit words
using 8-bit one's complement arithmetic. Choose all correct answers.

8-bit checksum
16-bit checksum
8-bit CRC
+ 16-bit CRC
16-bit MAC
32-bit MAC
none of the above

+ checksums are useless for a burst of errors
+ 8 bit CRC is to small
+ A MAC can't catch the error with 100% certainty.  A large MAC might have a
  really good chance to catch it, but it won't be 100%

#PROBLEM 2-5B  (1/1 points)
Assume you have a single burst of bit errors (bit flips) within a packet and no
other errors. Consider the shortest burst length which each of the following
schemes might not detect. An 8-bit checksum sums all 8-bit words using 8-bit
one's complement arithmetic. Which scheme's burst length is longest? I.e., which
scheme can detect the longest burst errors with 100% probability?

+ checksums and MACs can't guarantee anything for bursts
8-bit checksum
16-bit checksum
6-bit CRC is too short
+ 12-bit CRC
16-bit MAC
32-bit MAC

#PROBLEM 2-5C  (0 points)
Your packet has two bit errors, spaced 16 bits apart (for instance, bits 1 and
17), and no other errors. Which of the following schemes will detect the error
at least 99.99% of the time? Choose all correct answers.

+ 99.99% is not guaranteed
8-bit checksum
16-bit checksum
8-bit CRC
16-bit CRC
16-bit MAC
32-bit MAC

#PROBLEM 2-5D  (1/1 points)
Which of the following statements are true?

The same data segment can have two different checksums.

A CRC is robust to a malicious changes (an attacker cannot change a packet such
that the CRC remains the same).

+ It is faster to compute a checksum than a CRC.

+ The same data segment can have two different MACs.

#PROBLEM 2-7A  (1/1 points)
Which one best summarizes the underlying idea of flow control?

A receiver must send acks to a sender to guarantee reliability.

Many system designs are easier to build if you use a layered architecture.

There is no point in sending data faster than the network can deliver it.

+ There is no point sending data faster than the receiver can process it.

#PROBLEM 2-7B  (1/1 points)
Which of the statements below are false? Select all the answers that apply.

+ In a network that does not corrupt packets (but still can drop packets),
  stop-and-wait without an additional parity bit guarantees that the receiver
  receives all data in order.

+ In a network that does not corrupt any packets and cannot drop any
  acknowledgment packets, stop-and-wait without an additional parity bit
  guarantees that the receiver receives all data in order.

+ In a network that does not drop, duplicate, or corrupt any packets,
  stop-and-wait without an additional parity bit guarantees that the receiver
  receives all data in order.

In a network that does not drop, duplicate, or corrupt any packets,
stop-and-wait without an additional parity bit guarantees that the receiver
receives all data, but not necessarily in order.

#PROBLEM 2-7C  (1/1 points)
T/F For stop and wait to work correctly, the data receiver must maintain a 
timeout on the acks it sends so that it can retransmit acks that are dropped.

+ False

#PROBLEM 2-7D  (1/1 points)
Assume a network that does not drop, duplicate, or corrupt any packets and that
you are using a stop-and-wait protocol between two endpoints, A and B. The time
it takes for a packet to get from A to B is uniformly distributed between 10 and
20ms. The time it takes for a packet to get from B to A is uniformly distributed
between 20 and 30ms. The time it takes any packet to get from one endpoint to
the other is independent of the time it takes any other packet.

You want to choose the timeout so that for each data packet that you send, 50%
of the time you will not have to retransmit it due to a timeout. What is the
largest possible timeout you can choose (in ms)?

+ stop and wait, so send, wait for response, THEN send another
+ half the time we won't have to re-transmit
+ A -> B is [10, 20]
+ B -> A is [20, 30]
+ the quickest we can send another is 10 + 20 = 30
+ the latest we can send another is 20 + 30 = 50
+ average of 30 and 50 for the 50% retransmission?
+ 40

#PROBLEM 2-8A  (1/1 points)
Identify the false statements. Choose all the answers that apply.

You have two sender-receiver pairs using sliding window protocols. Assume that
for each, the sender and receiver have the same window sizes. The
sender-receiver pair with the larger window size will not always deliver data
faster from the sender to the receiver. It may depend on network conditions and
how often the application tries to write data.

+ Sliding window does not work if the send window size is different from the
  receive window size.

In a sliding window protocol, if a receiver receives a data packet with sequence
number 10, there are circumstances in which it will send an acknowledgment,
which acknowledges sequence number 20, even if no additional data packets arrive
between when it receives the data packet with sequence number 10 and when it sends
the ack. 

The TCP packet header includes a window size field for each side to communicate
how large its receive window is.

None of the above.

#PROBLEM 2-8B  (1/1 points)
Assume that:

LastSegmentSent is the sequence number of the last data packet the sender sent.

LastAckReceived is the sequence number of the last acknowledged data packet.

Which of these invariants must the sender of a sliding window protocol maintain?

+ LastSegmentSent - LastAckReceived <= (Sending window size)
LastSegmentSent - LastAckReceived + 1 <= (Sending window size)
LastSegmentSent + LastAckReceived >= (Sending window size)
LastSegmentSent + LastAckReceived > (Sending window size)

#PROBLEM 2-8C
If a sender sends a packet with sequence number 1000 and its window size has been
constant at 20, what is the largest sequence number that the receiver has
definitely received?

+ 980

#PROBLEM 2-8D
Assume that it takes a packet 50ms to get from A to B and 150ms to get from B to
A and that there are no drops or corruption in the network. A has a send window
size of 20 packets, each 100 bytes long, and uses a sliding window protocol. 
What is the maximum bandwidth for A to send packets to B? (In kB/s, where 
1 kB = 1000 bytes.)

+ 10
+ The round trip time of a single packet is 200ms. We want to maximize bandwith,
  so maximize the number of packets sent in our window, so 20 packets. We can't
  send another packet until we hear back from the first packet in window sent
  (packet 1).
+ So, 200 ms round trip, and 20 packets in this amount of time MAX.
+ 20 packets / 200 ms = 100 packets / 1 second = 10000 bytes / 1 second = 10 kB/sec

#PROBLEM 2-8E
You have set up a reliable protocol with a receive window of 10kB and a send
window of 10kB. Your friend is sending you a file using the protocol and you 
think it's too slow. Your friend tells you to increase *your receive window* to
20kB. In the best possible case, how much will this improve your throughput?
Express the value as a fractional increase. For example, if the larger window
will triple your throughput (e.g., from 2 to 6), then write 3. If it does not
increase (eg. stays at 10), then write 1.

+ 1
+ receive window doubles to 20kb, but send window is still 10kb. sender is
  sending at most 10kb of traffic at a given time, so receiver can only ack
  those 10kb traffic received...

#PROBLEM 2-9A  (1/1 points)
A sender is using selective repeat as its retransmission strategy. It sends
packets 13,14,15,16,17 and does not receive an ACK for packet 13. It will
retransmit which of the following when packet 13 times out:

+ 13
12 and 13
13 and 14
13, 14, 15, 16, 17

#PROBLEM 2-9B  (1/1 points)
Which retransmission approach is better if there are bursts of losses?

+ go back N

#PROBLEM 2-9C  (1/1 points)
A sender has a window of 4, while the receiver has a window of 1. Is the
protocol going to behave like go-back-N or selective repeat?

+ go back N
+ the receiver can't buffer everything in the window, so if the first packet in
  the window was dropped, the receiver also misses the rest of the window, so
  the sender has to re-send everything

#PROBLEM 2-9D  (1/1 points)
A sender has a window of 4 and the receiver also has a window of 4. Is the
protocol going to behave like go-back-N or selective repeat?

+ selective repeat
+ the receiver can buffer packets after it missed one, so the sender doesn't
  have to re-send the buffered packets along with the missed one

#PROBLEM 2-10A  (1/1 points)
A sender transmits a TCP packet with 500 bytes of data, in which the sequence
number is 3400. The other side receives the packet and piggybacks an ACK in a
data packet with sequence number 2800. Assume no other data packets have been
sent. What is the ACK number in the packet?

2800
2899
3300
3400
3401
+ 3900
3901

+ You start counting the first byte at 3400, and the last byte sent would be
  3899. So the next one, the expected, should be 3900.

#PROBLEM 2-10B  (1/1 points)
The checksum in the TCP header covers part of the IP header. T/F?

+ True

#PROBLEM 2-10D  (1/1 points)
Which of the following is/are true about the TCP header?

The 'window' field informs the receiver about network congestion
TCP options have a fixed length
the length of the header is exactly 20 bytes
+ the offset field indicates where in the packet the data starts

#PROBLEM 2-11B  (1/1 points)
TCP allows for a "simultaneous open" in which both sides know each other's port
numbers. How many messages in total are sent to establish the connection?

2 -- two SYN packets
3 -- SYN, SYN/ACK, and ACK packets
+ 4 -- two SYN and two SYN/ACK packets
depends on whose message arrives first

#PROBLEM 2-11C  (1/1 points)
A correctly-behaving host that has no more data to send, can send a FIN packet
and immediately close the connection. T/F?

+ False

#PROBLEM 2-11D  (1/1 points)
Why would a host enter the TIME_WAIT state?

+ to avoid data corruption in the case that ports are immediately reused and
  there is a sequence number overlap

to let everybody on the wire know that the connection is over

+ to make sure that the final ACK is not lost

none of the above
