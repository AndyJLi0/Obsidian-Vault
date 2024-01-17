*Basic definitions*
- **Networks**: connection of computers by "wires" (virtual or physical)
	- the [[internet]] is a network of networks
- **Host:** or end system, the device at the end of a wire that can run an application.
- **Bandwidth:** measured in bits/second, maximum rate at which data can be sent over a link.
	- 1 Gbps ethernet, 'Speed'
- **Latency:** the time it takes for one way travel process.

***Important aside on units***:
Data size is usually measured in Bytes (KB, MB, GB, TB). These are all powers of 2 ( $2^{10}, 2^{20}, 2^{30}, 2^{40})$
Rates at which data is sent is in X **Bits*** per second. (Kbps, Mbps, Gbps, Tbps). These are all powers of 10 ($10^3, 10^6, 10^9, 10^{12}$). If instead rate is measured in **Bytes** per second (KB/s etc), we use the multiple of 2's again.

*Network Metrics*
- **RTT (Round-trip-time):** latency for sending something and receiving something back
	- RTT is easy to measure locally, latency is difficult
- **Jitter:** variation in latency and/or RTT.
	- causes include different packet paths (see [[Packet Switching and Circuit Switching]])
	- network congestion
	- wireless jitter (interference in medium)
- **Throughput:** amount of data moved from one location to another in a given time.
	- Bytes per second or bites per second
- **Goodput:** rate at which *useful* data arrives. We ignore headers, encoding costs, data loss, etc.
	- "My file downloads at 10MB/s", this talks about **ONLY** the file, not its headers and other info. Therefore, 10MB/s would be the goodput, and the throughput would actually be more.
- 