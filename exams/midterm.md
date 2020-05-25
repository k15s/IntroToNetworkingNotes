#Question 1: General
What data rate is needed to transmit an uncompressed 4" x 6" photograph every
second with a resolution of 1200 dots per inch and 24 bits per dot (pixel)?

+ 691,200 kb/s
28.8 kb/s
28.8 kbits
8.29 Mb/s
829 MB/s

+ 4 * 6 = 24 square inches
+ 24 inches * 1200 dots/inch = 28800 dots
+ 28800 dots * 24 bits/dot = 691,200 bits

#Question 2: Layering
"Layering" is commonly used in computer networks because (check all that apply):

It forces all network software to be written in ‘C’.'
Encapsulation is the lowest overhead method to transmit data.
+ It provides a separation of concerns; each layer has well defined
  responsibilities.
+ It allows widespread re-use of code and functionality.
It keeps networks warm enabling them to run faster

#Question 3: The Internet Protocol
IP provides (check all that apply):

a connection-oriented service
+ unreliable delivery
best-effort delivery
in-sequence delivery

#Question 4: The Transmission Control Protocol
TCP provides (check all that apply):

+ a connection-oriented service
unreliable delivery
+ best-effort delivery
+ in-sequence delivery

#Question 5: TCP and Flow Control
You set up an experiment in which host A blasts a 26 Megabyte file to host B
through a single TCP Tahoe connection over a 1 Gigabit/sec Ethernet. B transmits
no data. Neither host uses any IP or TCP options. The maximum Ethernet payload
size is 1500 bytes. With no options, the IP+TCP headers occupy exactly 40 bytes.
This leaves a TCP maximum segment size (MSS) of 1,460 bytes, approximately
1/18,000 the size of the file. Which of the following statements are true:

+ Host B can send TCP acknowledgements to host A, even though it has no data to
  send.

The size of the flow control window in segments B sends (the window field)
cannot increase throughout the duration of the connection.

Suppose that the last RTT sample at host A is 10 msec. The current value of the
RTT estimate must be greater than or equal to 10 msec.

If the user temporarily stops or suspends the process receiving the data on
host B, host A will eventually stop sending new data until B's receiver process
is resumed.

#Question 7: Queues
While visiting Disneyland you watch the single queue of people waiting to go on
one of the rides. You estimate an average of 12 new people arriving per minute,
and an average of 240 people waiting in the queue. What is a reasonable estimate
of the average time people wait in line before they board the ride?

20 seconds 240 seconds 20 minutes 0.5 minutes

+ 240 people in line
+ average, so consider the middle person in line, meaning 119 behind, 120 in
  front
+ 120 people leave before he gets to leave
+ we want to keep the average at around 240
+ 20 min / 120 people = 1 min / 6 people, rate close to 12 people arriving / min

#Question 10
We can think of the network as a spanning tree, with the video server at the
root, and the one hundred million subscribers at the leaves. Assuming that the
tree has degree eight (i.e. each router in the tree connects to eight other
routers closer to the leaves), then roughly how many routers does a packet pass
through from the root to each subscriber?

9
10
11
12
13

+ consider tree form
    + root: 1 node
    + root has 8 children: 8 nodes
    + those 8 children each have 8 more children: 64 nodes
    + ...
    + 8^0 + 8^1 + 8^2 + 8^3 + 8^4 + 8^5 + 8^6 + 8^7 + 8^8 + 8^9 is greater than
      100 million
+ this means we can reach any child within 9 hops with this structure?

#Question 11: Queues
At the start of every second, a train of 100 bits arrive to a queue at rate 1000
bits/second. The departure rate from the queue is 500 bits/second. The queue is
served bit-by-bit, and you can assume the buffer size is infinite.

a) What is the average queue occupancy? (in bits)

+ train arrives bit by bit, so the moment bits arrive, they start departing
+ 100 bits at rate 1000 bits/second
    + 0.1 seconds for all bits to arrive
+ departure 500 bits/second
    + so when the first bit arrives, it departs
    + 0.2 seconds for all bits to leave
+ so it takes twice as long for all bits to leave than for all bits to arrive
    + so at 0.1 seconds, half the bits have left, but entire train already
      arrived
    + therefore, 50 bits are in the queue at 0.1 seconds
    + so in 0.2 seconds, we see 0 bits -> 50 bits -> 0 bits in queue
        + that gives us 25 bits average in this time span
+ at 0.2 seconds, queue is empty, so 0.8 seconds for remainder of second, is
  occupancy 0
+ average occupancy = (25)(0.2) + (0.8)(0) = 5 bits

b) What is the average delay of a bit in the queue? (in milliseconds)

+ at 0 seconds, first bit arrives, no delay whatsoever
+ we established that at 0.1 seconds, only 50 bits are in the queue
    + we also established that at 0.2 seconds, the queue is empty
    + so 0.1 second delay starting at 0.1 and ending at 0.2
+ 0 delay from 0.2 to 1.0 seconds, ignore range
+ so we only concern ourselves with range from 0 seconds to 0.1 seconds during
  which bits arrives then maybe delay
    + 0.1 is the max delay for bits that arrive precisely at 0.1 s in range of
      0.1 s
    + so average delay intuitively is 0.05 seconds = 50 seconds

c) If the trains of 100 bits arrived at random intervals, one train per second
on average, would the average queue occupancy be the same, lower or higher than
in part (a)?

+ in prior steps, trains never overlapped, so we never had bits from two trains
  in the queue simultaneously
+ if two trains arrive near each other and we have bits from two trains in the
  queue simultaneously this will raise the average
    + one arrives at 0, another arrives at 0.1

d) If the departing bits from the queue are fed into a second, identical queue
with the same departure rate, what is the average occupancy of the second queue?
(in bits)

+ first queue
    + 100 bits go into it total
    + after 0.1 seconds, all bits have arrived
        + 50 bits in queue at this time
    + after 0.2 seconds, all bits have departed
        + 0 bits in queue at this time
+ second queue
    + departure rate of 500 bits/second
        + therefore, every 0.1 second, 50 bits depart
    + 100 bits will also go into this total
    + after 0.1 seconds, 50 bits have arrived
        + all depart within 0.1 seconds
    + after 0.2 seconds, other 50 bits/all 100 bits have arrived
        + empty right as the second half of the bits start arriving
        + all depart in 0.1 seconds
+ so 0 bits, never accumulates since the first queue's departure rate is setting
  the arrival rate for the second queue
