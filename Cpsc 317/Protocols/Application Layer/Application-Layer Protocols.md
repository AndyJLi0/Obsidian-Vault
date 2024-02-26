#applicationLayer #Cpsc317
>[!info] Examples Include:
>[[The HTTP Protocol]]
>[[The DNS Protocol]]
>[[The E-Mail Protocol]]

### Choices to make
- Open vs. Proprietary
- Architecture: client-server, peer-to-peer(P2P)
- Choice of transport protocol
	- what do I need from the lower layer and what quality/characteristics of service do I value?
- Types and formats of messages (request, response, etc.)
	- message syntax semantics and message encoding format.

### Open Vs. Proprietary Protocols
- **Open protocols** are publicly known. Examples include:
	- DICT, HTTP, SMTP, SSH
- They are usually defined by standards in **RTC** (Request for comments) documents.
- They can have many different implementations
--- 
- **Proprietary protocols** have only one implementation, and are basically closed to the public. How it is made is secret. Examples include:
	- iCloud, Zoom, Skype
- You can create a monopoly with this!
### Architecture
- **Client-Server architecture** applications has well-defined roles for client and server
- The server is always on, with a permanent address or host name
- The client establishes connection and the connection itself is always between one client and one server (however server can have multiple clients).
---
- **Peer-To-Peer architecture** applications typically connect peers with the same hierarchical role
- Decentralized and has no real 'server'
- Peers request service from other peers, and provide service in return
- *Self scalability*: new peers bring new demand and new capacity
- The big issue is finding a peer itself! How can I find someone with the info I want to download?
- *(many times illegal )*
- ---

### What Quality of Service Does it Need?
Here are some things to consider for services and how much they can tolerate it:
#### Data Loss
- some apps can tolerate some loss, (*e.g. audio*), (*e.g*., zoom)
- other apps require 100% reliable data transfer (*e.g*., file transfer, web, email)
--- 
#### Time Sensitivity
- some apps require low delay to be "effective" (*e.g.,* interactive ones, multiplayer games, anything in real-time).
- downloading files and stuff or browsing the web, I don't have the expectation to finish my task in 3 $ms$ or something like that.
---
#### Bandwidth
- some apps (*e.g.,* multimedia) require minimum amount of bandwidth to be "effective". *ZOOM*
- other apps (*e.g.,* elastic apps) make use of whatever bandwidth they get.


### Two Transport Options
see [[Transport Layer]] for more info
- **Reliable stream (TCP):** Connection, reliable ordered deliver, flow/congestion control, possible delays. 
	- wonderful features, *no bound on delay.*
- **Unreliable packet (UDP):** No connection, best effort, no flow/congestion control, no (Transport level  delay). (see [[The OSI Model]]) 
	- lets send some network, *hope it gets there*. 
- Use the latter for low latency (see [[Bottlenecks and Delay]], the former if you want anything else.

> [!info] Transport Layer Digression
> The Transport layer has 2 important things. A **Transport Layer Address** and a **socket**.
> - The Transport Layer Address: a pair of 32-bit IP host address and a 16-bit port number (just an identifier). It's how network applications are identified.
> - The socket is a network end point. Its an 'object' and can be made with sys calls. Represented as a file descriptor (doesn't point to an open file entry but a socket!).
> 