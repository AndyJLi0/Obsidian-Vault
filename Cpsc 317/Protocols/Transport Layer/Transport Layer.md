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
There's 2 main protocols for the transport layer. See [[Application-Layer Protocols#Two Transport Options]] for some of the same information. Also see [[TCP Protocol]].

| Reliable Stream (TCP) [[TCP Protocol]] | Unreliable Packet (UDP)[[UDP Protocol]] |  |
| ---- | ---- | ---- |
| Connection | No connection |  |
| Reliable ordered Delivery ([[Windowing Protocols]]) | Best effort |  |
| Flow/Congestion control ([[Flow and Congestion Control]]) | None |  |
| Possible delays  ([[Lost Segments and Timeouts]])  | No (transport level) delay |  |
| Provides almost all services | Practically no services |  |

-  Congestion Control: When packets are getting dropped, the transport layer can detect that and tell the sender to slow down the transmission
- Flow Control: the receiver cant keep up how fast the sender is sending the data and the transport layer can negotiate with them.
- Look at the packet headers to determine the transport protocol the connection is using.

