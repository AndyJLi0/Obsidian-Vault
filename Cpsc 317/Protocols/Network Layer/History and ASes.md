### Idea of a Network of Networks (ASes)
- First we had a bunch of private networks. Thesenetworks started to interconnect. (the internet is a network of networks).
- These entities are called autonomous systems (ASes).
	- Remnant of old private networks.

## Autonomous Systems
- Assigned a range/collection of [[IP addresses]]
- Responsible for routing to addresses it "owns"
- Responsible for routing to addresses that are not its responsibility
- All these allows for multiple entities and keeps everything autonomous.
---
**Autonomous Systems** are usually controlled by companies. Some do end systems (last mile. Service directly to end users/customers), some do haul lines (backbones. Connecting different networks, withing country, around the world).
- Some companies do a mix of both
- Networks often connection to each other at Internet Exchanges (like a broker).

Networks in ASes are arranged in tiers between 1 to 3. Tier 1 networks are the largest networks, and are providers to the tier 2 networks. Tier 2 networks are the customers of tier 1 networks. Tier 3 networks are the closest to the users. They can be serving really small localized regions.

### Tier 1 Networks, International
- Lumen (USA)
- Arelion (Sweden)
- GTT (USA)
- NTT (Japan)
- Tata (india)
### Tier 2 Network, National
- Hurricane Electric
-  Korea Telecom
### Tier 3 Networks, (Stubs)
- Everybody else
### Naming (addresses) of a Network
- Purpose of the network layer is to route messages from source machine to destination machine.
A name includes:
- Format, Semantics, Properties.

For addresses, each address name is for an interface, **not** for a host.
#### Why?
- Consider a cell phone, it has multiple interfaces. WIFI and Cellular data.
- When you make a connection using one of these interfaces, you expect the data to come back on the same interface. (thus need to address interface not host.)
-  A laptop has at least one *real* network interface, and at least one *loopback* network interface.
### What Exactly is a Network?
- At the network layer, we use the term *network* in two different ways:
	-  Talking about autonomous systems as networks (Collection of computers and routers owned and admined by a single org
	- Talking about forwarding, a network is a single shared communication medium connected to at least 2 interfaces