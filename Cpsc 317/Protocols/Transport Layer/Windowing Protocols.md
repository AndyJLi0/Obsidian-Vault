---
tags:
  - "#TransportLayer"
date: 2024-02-09
---
## The Basic Idea
- Expanding on [[Protocol State Machines and Reliability]] Sender sends a bunch of segments before waiting for an ACK, instead of sending a segment and waiting for its ACK (linear).
	- How many segments should we send?
- Expand sequence numbers to integers to account for multiple in-transit and unacknowledged packets
- Since we are sending so many, any or all of these segments might get lost.
- Sender has to be ready to re-send any segments that get lost. 
- Receiver has to be ready to handle segments that are lost.

### Sender's Window
- **Sender's window:** the range of segments that are stored for potential re-send.
- Usually considered to be fixed size.
- Window only moves when the first segment in the window is acknowledged. (SLIDING WINDOW)
- New segments are sent only when they "fit" in the window
### Receiver's Window
- **Receiver's Window:** store segments received out of order
	- out of order because of drops or -reordering in the network. (Basically just gets total number of segments, if segment 1 is received, *immediately* send the ACK and the **sender's** window slides forward).
	- If window is 1 segment: out of order segments are dropped
- Once missing segments arrive, window is processed.
- Segments received beyond the window limit will be discarded.
  
>[! example]
>- It takes 1 time unit for the sender to transmit each segment
>- It takes 2 time units for a bit to get from the sender to the receiver or from the receiver to the sender
>- No segments are ever lost or delayed
> 
> (basically 3 time units from sender -> receiver. 2 time units from receiver ->sender)
> 
>![[Windowing Protocol.png]]
>
>We would need a sender window of 5 to maintain continuous sending and no idling. Once the 5th segment is sent, we immediately get the ACK for the first sent segment from the receiver, and we can then send our 6th segment.
>
>We are basically "loading" up our pipeline the size is the initial time it takes before we can get the process going efficiently. We compute this with $2 + 3 = 5$. This is kind of like [[y86 Pipelining]].

### Problems to Consider
**Sender**
- How does the sender know that data got lost?
- Can lost data be distinguished from a lost ACK?
- If we send more than one segment, how many segments can we remember?
**Receiver**
- How can you tell if data is out-of-order or missing?
- What should be ACKed?

### Solutions
#### Go-Back-N Strategy 
*see [[Selective Repeat]] for another strategy for reliable ordered delivery*
**Receiver:**
- Window of size 1
- When a segment is received, send an *ACK for the last segment that was received in order*.
- Deliver the arriving segment if received in order, otherwise discard the segment
**Sender:**
- *Sender's window* determines the number of outstanding (unacknowledged) segments held in memory.
- Start timer on first segment sent
- Received ACKs may be cumulative (restart timer on receipt)
- On timeout go to last unACK'ed segment and re-send everything (re-start timer as well).
---
**What if an ACK gets lost?**
The receiver is the one sending the ACKS. Lets say the receiver is sending back segment number 7, then 8, then 9, but 7 gets lost. The sender gets ACK 8 and 9. *However the receiver can **only** send ACK 8 and 9 **if** it sent ACK 7 (even if its lost).* Therefore, we can safely continue with segment 10, 11, etc.

**What if a segment gets lost?**
### Sequence Numbers
The range of a sequence numbers is limited by $n$, the number of bits used to represent them. If we have 3 bits, we have the numbers 0 to 7, and when we get to segment/ACK 7, we then send number 0. This is computed by $2^n$. 

The issue we need to consider is **what can the sender's maximum window size be ?**

Lets say sender's window is size $2^n$. Let $n=2$. We have the sequence numbers $0,1,2,3$. Let say we send all 4 bits and segment or ack 0 gets lost. The receiver and segment cannot distinguish between the the *new* and *old* 0. 

Therefore, we have this rule:
$$\text{S size + R size} \leq \text{sequence number range}$$
Therefore, for the [[Windowing Protocols#Go-Back-N Strategy]], we have:
- receiver's window size is $1$
- sender's maximum window size is $n-1$.

--- 
#### If SWS + RWS == N
- Lets say sequence number from 0 to 3. Sender window size equals 3, receiver window size equals 1.
	- Sender sends 0, 1, 2. Sender waits for ACK
	- Receiver receives 0, 1, 2. Sends ACKS for all of them. Receiver's window is now \[3\]
	- Sender's window is now \[3, 0, 1\].
- Lets say the sender sends again 0, 1, 2. The receiver receives them and rends ACKs for all of them, but all the ACKs are lost. Sender's window can only move once ACKs are received. the Sender times out and resends 0, 1, 2. The receiver ACKs 2 to let the sender know it has received everything up to and including 2.

#### If SWS + RWS > N (BAD)
- Sender sends 0, 1, 2, 3. Senders window is \[0, 1, 2, 3 \]
- Receiver ACKs all of them and the receiver's window is now \[0\].
- All ACKs are received and the Sender sends more packet again with seq no. 0, 1, 2, 3.
	-  This is an issue because the receiver doesn't know if it's ACKs were lost or not. For all it knows, these packets could be the old ones, or a set of new ones. There is no way of knowing.