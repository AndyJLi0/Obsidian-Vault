---
date: 2024-02-14
tags:
  - "#TransportLayer"
---
*Selective repeat tries to overcome the two limitations of Go Back N (a sliding window protocol).

### Receiver:
- Each segment is ack'ed individually
- Out of order segments are stored for later: **Receiver's window**.
### Sender:
- Can have a specific number of outstanding (unacknowledged) segments in memory: **Sender's window**.
- Each segment has its own timer
- Each segment is individually resent if timeout is reached
- ACKs received in order move the sender's window.

---
Let both receiver and sender window size be 5. 
- Sender sends 0, 1, 2
- Receiver receives them, ACKs all, and sender gets all
- By now, both sender and receiver window on packet number 3 to 7.
--- 
Lets say the sender sends 5 data. (cannot send anymore before an ACK since window size is 5)
- Let packet number 3 get lost. Receiver receives 0, 1, \_\, 3, 4.
	- Since 0, 1 are received in order, R window gets immediately shifted by 2.
	- Since 3 and 4 are not received in order (2 is missing), it is stored in the receiver's window buffer for later. (*selective* acknowledgements are sent for packet 3 and 4, with **do not** shift the sender's window size)
	- The sender resends packet 2 (the missing one). Receiver receives. and then the R window can jump from 2 all the way to 5!

## Sequence Numbers for SR
We still have the rule: $$\text{S size + R size} \leq \text{sequence number range}$$
However, we can have window size of R greater than 1. However, it doesn't make sense to have the receiver window size be greater than the sender (more capacity than the network can handle), *nor* does it make sense to have the sender window be greater than the receiver.
- Thus, its optimal to have $S_S + S_R = \lfloor n/2 \rfloor$

## Difference between GBN and SR
- GBN has cumulative ACKs. If packet 0 is not ACKed, everything needs to be sent again. GBN supports in order delivery only. 
- SR has selective/individual ACKs. Because of this, SR supports out of order delivery and if a ACK is lost, the sender only needs to retransmit specific segments, not all of them.
---
- *if an ACK gets lost, GBN suffers less than SR since GBN has cumulative ACKs, which may account for the lost ACK, however for SR, the data packet is sent again*
- *if a data segment gets lost, SR suffers less than GBN since SR has a window size greater than 1, and an buffer all the non-lost data, while GBN's receiver must wait again for the data to get retransmitted before its window can slide.*

