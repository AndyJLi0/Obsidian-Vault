-  *Explains how TCP handles lost data and lost ACKs, "fast retransmit" mechanism, and TCP's congestion detection and control.*

### TCP Re-transmission Scenarios
- Lost Data/ Lost Ack:
	- The packet is resent by the sender and everything proceeds as normal
- Premature Timeout:
	- If the sender timeouts but the ACK is actually en route and is received after retransmission, we re-ACK the same thing. If 2 segments are sent in sequence by Host A and the above scenario happens. We would retransmit the first segment, receive ACKs for both segments or just the latter segment, and thus the second segment is not resent.
- Early retransmission (triple duplicate ACK)
	- ignoring the first ACK, after the sender receives 3 additional ACKs with the same number (lets say seq 100 gets lost, and so the receiver transmits ACK 100 over and over again), the sender retransmits segment 100.

We have that TCP handles lost data packets better than both [[Windowing Protocols#Go-Back-N Strategy]] and [[Selective Repeat]]. TCP reduces the number of ACKs because of its cumulative property (particularly helpful during retransmissions). During retransmission, since TCP retransmits only the first unacknowledged segment, reducing bandwidth unlike GBN where it retransmits the whole window.

We have that TCP handles lost ACKs just like GBN. It has cumulative ACKs and so its similar to GBN's behaviour.

### Congestion Detection
TCP has its own [[Flow and Congestion Control]] and management system.
- Congestion is detected via:
	- Timeouts
	- Multiple (4) identical ACKs( triple duplicate ACK)
- Sender adapts its congestion window
- Lack of congestion is detected via:
	- Sending a window full of data without detecting congestion

### Congestion Management
- At the first sign of congestion it slows down a lot
	- Cuts congestion window in half
	- Multiplicative decrease
- Increases window size slowly when congestion has eased
	- Adds 1 segment to congestion window
	- Additive increase
*This is the additive increase multiplicative decrease algorithm*

### Slow Start Mechanism
TCP starts with a congestion window of 1 segment (sender window). The congestion window increases by 1 each time a segment is ACKed. (which is equivalent to doubling it each time we send a full window of data with no congestion detected). We stop doubling when we detect congestion.
