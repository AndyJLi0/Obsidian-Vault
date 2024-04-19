Network layer functions are divided into forwarding and routing.

## IP  Service Model

- Best effort delivery from one network interface to another network interface. Now guarantees and no (per flow) state in routers. There are *two* parts. **Forwarding and Routing**.
### Forwarding
Move packets from router's input to appropriate router output. Think of it as getting through a single intersection.
### Routing
Determines route taken by packets from source to destination. Has *routing algorithms*. Think of it as planning a trip from source to destination.

## Network Layer Components
### Data Plane
- Local, per-router function. Determines how datagram arriving on router input port is forwarded to router output port. Is a forwarding function.
- It makes these *forwarding decisions* by comparing addresses of networks in its forwarding table to the destination IP in header.
### Control Plane
- network-wide logic. Determines how datagram is routed among routers along end to end path from source host to destination host.
- There are 2 control-plane approaches:
	- traditional routing algorithms implemented in routers.
	- software-defined networks (SDNs) implemented in remote servers.

## IPv4 Addresses
- In dotted decimal notation. One byte is eight bits. 4 Bytes separated by periods. This is 32 bits (4 billion addresses). Now we need more!
- We introduce a class based addressing system: ![[Class Based Addresses.png]] We see that *class B* gives you 16 bits for the network address, however **2 are reserved for the class**, so class B has a total of $2^{14}$ possible networks.
- Class A reserved addresses include:
	- Network = 0, Local identification.
	- Network = 10, Private networks.
	- Network = 127, Local host.
- We still need to continue complicating this because we have ran out of addresses again!
### Classless Inter-Domain Routing Addresses (CIDR)
Allow any number of bits in the network number (any between 8 and 31). The rest of the bits name the host within the network. Finish off the IP address with a /(number) indicating the boundary.
### NetMask
We have the / (number) to indicate how many bits are given to the network address. Since the router doesn't care about the host, we mask it using AND.

![[Pasted image 20240301124733.png]]
## IPv6 Addresses
- 128 bits, 32 Hex-digits, separated by colons. Zero suppression.
- Separated into network+subnet (48+16), host(64).
- 
