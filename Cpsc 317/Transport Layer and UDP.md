#TransportLayer #Cpsc317
Look at [[Application-Layer Protocols#Two Transport Options]] for some basic info

## Purpose
- Provide logical communication between application processes running on different hosts
- **Multiplexing** of communication to different application on end hosts. Makes sure data is sent to *appropriate* end point on a host.
- Provide services to applications 
	- Partial, Reliable, or ordered delivery
	- Flow, congestion, control
	- Bi/unidirectional
- Works with the network layer to actually enable a transfer of data over the network
---
- The **Network layer** provides logical communication between hosts (terminated at the interface)
- The **Transport layer** provides logical communication between processes (terminated at the application)
- The Transport protocol runs in end systems.
	- When sending, breaks application messages into *segments* for the network layer
	- When receiving, reassembles segments into *messages*, passes to application.

##### Multiplexing Applications Over Transport
- An application is identified by a transport layer address:
	- <IP address, port>
- The **IP address** gets you to the host 9technically the interface, but the interface is part of the host)
- The **port** gets you to some application process or thread on that host
	- Historically a 16 bit unsigned number (0 - 65535)
	- DICT - 2628, DNS - 53, HTTP - 80.
---
### The Big Picture
There's 2 main protocols for the transport layer. See [[Application-Layer Protocols#Two Transport Options]] for some of the same information.

| Reliable Stream (TCP) | Unreliable Packet (UDP) |
| ---- | ---- |
| Connection | No connection |
| Reliable ordered Delivery | Best effort |
| Flow/Congestion control | None |
| Possible delays | No (transport level) delay |
| Provides almost all services  | Practically no services |

-  Congestion Control: When packets are getting dropped, the transport layer can detect that and tell the sender to slow down the transmission
- Flow Control: the receiver cant keep up how fast the sender is sending the data and the transport layer can negotiate with them.
- Look at the packet headers to determine the transport protocol the connection is using.

## UDP
- Very simple segment/message format
	- 8 byte header:
		- Source port #, dest port #, length of entire UDP segment, and checksum
	- The application data goes behind the header. Wouldn't want an empty body so at least 1 byte body, and therefore the **shortest UDP message length would be 9 bytes**.
	- $2^{16} -$ length of the header would be the greatest UDP message length 
- Note that the IP address isn't in the header but its located in the network layer header.

### Checksums
- Detects errors (bit flip or something)
	- If error is detected, UDP just discards the packet
- Sender:
	- Computers some function on the data
	- Adds the checksum value to the data
	- Sends the data and the checksum
- Receiver:
	- Compares the same function on the received data
	- Check if computed checksum equals received checksum value.
		- **NO-** error
		- *yes-* no error
- Note that no all errors can be detected. 
#### Checksum Algorithm
- Treat data as 16 bit integers
- Function: addition (1's complement, carry out added back in)
- Checksum is another 1's complement of the computed value
- Verifying is computing the same function over the data and checksum (correct if 0)
##### Example:
``` (5 bit integer)
	1 1 0 1 0
	0 1 0 0 1
	1 0 1 1 0
	1 0 0 1 1 
	---------
	0 1 1 0 0 (sum and carry)
		  1 0 (carry)
	----------(add the carry)
	0 1 1 1 0
	--------- (ones complement)
	1 0 0 0 1 (done)
When verifying, use the same 4 '5 bit' integers, along with the sending check sum value, should get all zeroes if no bits have been fliped.
```

- 1 bit flip: detectable
- 2 bits flipped: detectable if on different columns, undetectable if in the same column.
