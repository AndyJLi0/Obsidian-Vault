### DCCP (Datagram Congestion Control Protocol)
- Does not provide reliable in-order delivery for data (like UDP)
- Provides application-selected congestion control (like TCP)
- Useful for multimedia streaming, online gaming.
### SCTP (Stream Control Transmission Protocol)
- Message-based (like UDP)
- Multiplex several application "streams"
- Independent reliable in-order delivery and congestion control for each stream (like TCP)
- Used for VoIP

### Today's Internet Traffic
- Majority of traffic on the web: [[The HTTP Protocol]] + TLS(security that ensures encryption)+ [[TCP Protocol]]
- Lots of traffic suffers inefficiencies in protocol stack of:
	- TCP
	- HTTP
	- interaction between HTTP and TCP
## Problems with TCP
- 3-way handshake is too expensive
	- worse with added TLS handshake for HTTPS
- RTT estimation too inefficient ( for congestion control)
	- RTT: time between transmitting a segment and receiving an ACK
	- Sequence numbers conflate ordering and reliable delivery
		- Retransmitted segments have the same sequence number as original
		- Confounds RTT estimates.
- head-of-line blocking
	- A small request blocked behind a large request, (HTTP/1.1 and TCP, since TCP ensures in-order delivery)

### Improvements in Application Protocol
**HTTP/2**
- Headers and encoded in binary and compressed 
	- Compressed binary format called QPACK
	- Not human readable
- Server allowed to push objects without being asked
	- Via "push promises" -reduce latency for objects client likely to ask
	- Client can say "No thanks, I got it already"
- Responses may be reordered w.r.t requests
	- can still prevent transfers if TCP lost packets
---
**We want to:**
- Multiple requests independently to avoid HOL blocking
- Improve connection handshake protocol
- Reduce congestion control inefficiencies
Since layered architectures creates constraints due to upper layers not being able to control lower layers, we can co-design applications and transport protocols (improve performance by combining some of the protocols).

## QUIC
Quick UDP Internet Connections. HTTP/2 over QUIC is HTTP/3

**Features Include:**
- Adds reliability on top of UDP
	- Implemented in user space instead of kernel
- Could be an alternative to UDP and TCP above IP layer itself
	- Problem is that existing network middleboxes cannot handle new protocols
- Integrates TLS into the transport
- Completes both TCP and TLS handshake in 1 RTT