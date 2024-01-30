*describe the architecture of  a peer to peer application, design goals for the bit-torrent protocol and block-chain protocols*

# Bit-Torrent
A p2p protocol for file sharing. Designed in 2001. Protocol v2 in 2017 (upgraded the has function).  Each node functions as **both** a consumer and provider of data.

## File Sharing Scenario
- Suppose some N machines have a file (N $\geq$ 1)
- Suppose some other $M$ machines want the file.
- if $N = 1$. and $M$ is sufficiently large, the sharing will be very slow.
- Overall, it is limited to the throughput of the $N$ machines.

$N$ hosts are called "seeds". All hosts are called "peers" $N+M$ nodes are both sources and sinks of data. As soon as 1 gets the data, it can share it with another.
### A clever way
Suppose we have 1 seed, and 1000 peers. the seed sends a different 1/1000 part  of the file to each peer, and then each peer sends this part to to each other. This is a lot faster than sending a single full file once at a time. In BitTorrent, there's ways to see if the chunk is corrupted or not with hashing and other security measures.

Each peer would ask another peer: "I have these chunks, do you have these chunks?", and then the process continues. A group of peers for a specific file is called a **swarm**. Each peer talks to one subset of the swarm at any time.
#### Policy Questions
- The rarest "chunk" is always asked for first. This information is located in the tracker file. The peer that sends to most data to a peer is called the **preferred peer.** The receiving peer will prioritize the preferred peer (tit-for-tat). 
- When a peer joins that doesn't have a chunk, the system randomly pairs together peers to share files. This is called **opportunistic unchoking** which prevents people from not getting anything if they join late.
#### In the real world
- Gaming and software updates. Millions of users for a game, only so many providers.
	- Billions of users Apple needs to update for.
- Often used to copy copyrighted material (Bit-Torrent Protocol is not illegal, but just lets the sharing of illegal content easier), however not exclusively:
	- Meta and X uses peer-to-peer to share content between servers.
- Decentralized system, so its good for security and anonymity.

### Implementation
- Bit-Torrent is an open protocol with many implementations
	- Most use TCP as a transport mechanism
	- Some use μTP – a UDP-based reliable transport protocol
## Blockchain
The purpose for blockchain is for an unmodifiable transaction history. It uses cryptography to ensure that records added to the history can never be changed or modified.
- Centralized or decentralized. 
	- The value of a decentralized system is that it doesn't' require a trusted agent.
- The mechanism includes:
	- All changes are broadcast (using gossip) to every peer in the system.
	- Changes are grouped into **blocks.**
		- When a block fills up, it is added to the **Chain**.
		- Since there is not central repo for the "truth", (every peer holds all of the history), its hard to modify since so many replicates are created.