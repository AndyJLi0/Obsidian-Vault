>You are designing a satellite network. The satellites are 360km from the surface of the earth. The speed of light is 3x10^8 metres per second. Packets are 1300 bytes. The network has a transfer rate of 100Mbps. Assume the transmission delay for an ACK is 0ms, and that the processing and queueing delays are 0.

- What is the one-way propagation delay for a bit in this network?
	- Take bytes and turn them into bits. divide by speed.
	- $$\frac{360000}{300000000} \times 1000 = 1.2 \text{ miliseconds}$$
- What is the transmission delay for one packet?
	- Take the number of bites and divide by the bandwidth
	- $$\frac{1300 \times 8}{100e6} * 1000 = 0.104 \text{ miliseconds}$$
- What is the total round-trip delay for one packet?
	- propagation delay $\times 2$ and add transmission delay
	- $$ 2.4 + 0.104 = 2.504 \text{ miliseconds}$$
- Assume that you can transmit packets back-to-back. What is the throughput?
	- throughput == bandwidth if back-to-back so it would be $100Mbps$
- Assume that you don’t transmit the n+1st packet until you get a (very short) ACK for the nth packet. What is the throughput?
	- We transmit 1 packet (1300 * 8) bits every 2.504 ms. (rt delay). Convert to $Mbps$ to get $$\frac{10400}{2.504} \times \frac{1000}{1e6} = 4.153 \text{ Mbps}$$
---
