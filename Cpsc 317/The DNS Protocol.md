### Naming and Network Structure
- Humans have the issue with having a hard time remembering numbers
- Addresses can change
**How do we know which destination IP address to use?**
- We can Map user-friendly names to IP addresses.
	-  Names are easier to remember and can mask address changes
This is called the **Domain Name System (DNS)**

### What is a Domain Name?
- identification string that defines a realm of administrative autonomy, authority or control within the internet.
- Formed by the rules and procedures of the DNS

For the string 'www.example.com', *www* is the World Wide Web, *example* is the Domain Name, and *com* is the Top Level Domain.

## So what is DNS?
- A **distributed database** implemented by a hierarchy of many name servers.
	- Distributed Database: a database that consists of two or more files located in different sites either on the same network or on entirely different networks. (the files are distributed)
- An [[Application-Layer Protocols]] used by hosts to communicate with name servers to *resolve* names, (*i.e.* translate names to addresses).

## Challenges for DNS
- **Scale**: 364.6 million domain name registrations as of September 2021
- **Ease of management**: Who decides if *cs.ubc.ca* can name a host "students".
- **Availability and consistency and security:** Is there only one IP address for an address?
- **Performance**: OpenDNS, Cloudflare each serve over 1 million requests per second.

### Solutions for DNS

*Hierarchical design, caching, and replication*

#### Hierarchy of Names
![[DNS Hierarchy 1.png]]
An example includes: 
- ROOT DNS SERVERS
	- ca ([[The DNS Protocol#Top-level Domain (TLD) Servers]])
		- ubc.ca ( Second-level domain/ authoritative name server)
			- cs.ubc.ca (Sub-domain / authoritative name server)
				- students.cs.ubc.ca (individual machine/ this data gets cached)
- Each bullet point is a name server and as we go down, each one stores a mapping of next level domain and IP addresses.
- 13 root name servers worldwide, they know the address of all TLD servers, while each server knows the address of the root name servers
- Every node knows the address of its children

#### Top-level Domain (TLD) Servers
- Responsible for domains ending in .com, .org, .net, .edu, etc.
- All top-level country domains.
- Root servers 
#### Authoritative DNS Servers
- provide authoritative hostname to IP mappings for an organizations' subdomains and servers. Can be maintained by organization or service provider
- Stores the name-to-address mappings for all names in the domain that it has authority for. This is scalable!

#### Local Name Servers
- Does not belong to the hierarchy, Each ISP has one. Also called "default name server"
- resolves queries iteratively.

### DNS Lookups
![[DNS Lookup 1.png]]
In step 2, every Local DNS server contacts the Root Server, and so there will be a [[Bottlenecks and Delay]] there. We therefore implement caching!
- Local DNS servers cache response to queries
- Responses include a "time to live" (TTL) field
- Server deletes cached entry after TTL expires
	- Good since popular sites are often cached and TLD name servers rarely change

#### DNS Records
- DNS servers store **resource records (RRs)**
	- RR is (name, type, value, TTL)
## DNS Protocol
- Client-Server interaction on UDP ([[Application-Layer Protocols#Two Transport Options]]) Port 53
	- Message size limited by max UDP segment size (512 bytes)
	- Reliability comes from repeating requests if the client times out
- Query and reply messages are both with the same message format. Format includes:
	- **Identification:** 16 bit number set in query, with reply using the same number.
	- **Flags:** query or reply, with reply authoritative
		- These two are a total of **12** bytes
	- The rest are resource records. *In a query,* **Questions** are always <Name, Type, Class> tuples, and is the only section included in a query.
	- *In a reply*,  RRs can be in three sections:
		- **Answer**RRs match the RR from the question. Is a DNS server has CNAME pointers for the requested query with same class, returns CNAME records in the answer. Can have multiple answers!
		- **Authority** RRs are the NS records pointing to name servers that are closes to the target name, redirecting a client to a 'better' server. Optional field.
		- **Additional** RRs are records that the name server believes may be useful to the client.
- Resolution is almost always "iterative"
### DNS questions
-  What is the purpose of the ‘AA’ (Authoritative Answer) flag in a DNS response?
	- The 'AA' flag indicates whether the server responding to the DNS query is authoritative.
- What is DNS name compression?
	- A technique used so that responses and messages for a DNS query isn't too long with all the domain names. It removes duplicate information. Instead of duplicate ASCII encodings, only pointers to the duplicate ASCII is in the message.
- Difference between an 'A' record and a 'CNAME' record?
	- 'A' record maps a domain name to its corresponding Ipv4 address. Translates human readable domain names to IP addresses!
	- **For 'A', the resolution process is simple.** There's no additional lookups required.
	- A 'CNAME' is a Canonical Name Record. It maps from an alias name to a true or canonical domain name. This target domain has its own 'A' address.
	- **For 'CNAME', additional lookups are required for resolution.**