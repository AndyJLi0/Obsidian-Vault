---
date: 2024-02-12
tags:
---
*Explain flow and congestion control, and how a sliding windowing protocol can implement it. Explain TCP.*

# Flow Control -- Slow Receiver
*Should we always send the full window size?*
- What if the receiving application is slow in accepting new data?
	- Packets will accumulate in the receiver's buffer
	- Eventually buffer will be full, packets will be dropped
	- Immediately re-sending this data does not resolve the problem
- Receiver will notify the sender how much data it can handle
	- Information usually just included in the ACK
	- The *sender* adjusts its window size based off this information

Flow Control is essentially a way for the sender to not overwhelm the receiver if they have different speeds. The windowing protocol lets us implement this.


# Congestion Control -- Busy Network
*What if the bottle neck isn't the sender or the receiver, but the network?*
- If the network can't handle a full window's worth of data, packets and ACKs will be dropped by the router.
- Missing ACKs are a sign that there is congestion somewhere (in either direction)
- Sender can reduce sender window once congestion is detected 
	- Example detection if some number of ACKs are missing in a period of time
- If all packets are ACKed, we can increase the window again

Just like flow control, congestion control can be implemented using a sliding window size to dynamically change how much data is being sent over a network to prevent packet loss due to a bottle neck in the router.

## Effect of Flow and Congestion Control on Throughput
- Throughput is affected by 
	- Bandwidth of senders direct network connection
	- Sender's adjusted window size (*congestion control*)
	- Receiver's specified window size (*flow control *)
- If sender's direct connection **is not** the bottleneck:
	- Another router will experience congestion elsewhere
	- Congestion control will reduce the transmission speed
