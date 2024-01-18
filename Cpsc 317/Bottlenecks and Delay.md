*edited 2024-01-17*
___

*What is a bottleneck?*
-  Suppose we have 4 devices connected in a link. Host to router has a bandwidth of 600 Mbps, router to another is 75 Mbps, and the second router to the last host is 10 Mbps. The maximum speed at which data can transfer is 10 Mbps. This is a **Bottleneck.**

### Types of Delay
- ***Processing delay:*** Examine packet to decide where to direct it (happens twice when packet goes down network protocol and back up on the other side).
- ***Queueing delay:*** Waiting time to get access to the link (time sat in the queue).
- ***Transmission delay:*** Time to actually write the packet onto the medium (virtual or physical wire) (related to bandwidth of network).
- ***Propagation delay:*** How long it takes to go from one of the physical layer to the other physical layer.
- ***End-to-end delay:*** All of these times combined.
	- As data travels, it goes through all the delay again as it hops from a node to a node. 
---
#### Classification of Delay : Fixed vs Variable?
 - **Processing delay** is essentially fixed. It depends on how good the processor is. Its super negligible.
 - **Queuing delay** is **variable**. Sometimes there's nothing in the queue, sometimes there's a lot.
 - **Transmission delay** is **fixed** per bit (every nanosecond per say) but **variable** for a packet since a length of a packet varies.
 - **Propagation delay** is **fixed** per k/m, but the distance is variable, so therefore its **variable** for location.
 - **EE delay** is trivially variable since there's another variable delay.

#### Delay Calculation Example ([[more delay worked problems]])
- **Transmission:** **NUMBER OF BITS IN A PACKET / BANDWIDTH***
- **Propagation**:
- **End to end:** Assume transmission delay for the ACK is 0ms, and that processing and queueing delay is 0. total round-trip for one packet is:
	- $5ms(\text{propagation}) + .1ms(\text{transmission}) + 0 ms(\text{ACK trans}) = 5.1ms$
- **Throughput:** Assume we can transmit packets back-to-back our throughput would be exactly our bandwidth since we are using all of it.
	- **Further:** Assume that we done transmit the next packet until you get an ACK for the $n^{th}$ packet. The new throughput would be: $$ 8* 1250 \text{bytes} / 5.1 \text{ms} =  ~2.0 Mbps$$
---
### Traffic Intensity
- Arrival rate of 100 packets/second and we can depart packets ever 1 millisecond (1000/second)
	- our intensity would be $$ 100/ 1000 = 0.1$$
- Generally, Traffic intensity is: $$\frac{La}{R}$$
	- $(a)$ is the number of packets arriving per second
	- $(L$) is our average packet size in bites
	- $(R)$ is outgoing rate (bandwidth)

#### Traffic Intensity VS Queueing Delay
- when traffic intensity becomes large, our queueing delay grows exponentially. So if $La > R$ we have an issue, but if $La >> R$ (traffic intensity greater than 1) is a complete disaster.
- formally: $$ \text{Total Delay} = \frac{S}{1 - U}$$ where $(S)$ is average service time when server is idle. $(U)$ is the traffic intensity.
- ***Takeaway***: if packets arrive faster than they can be disposed off, they may have to be dropped.