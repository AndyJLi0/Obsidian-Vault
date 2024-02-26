---
date: 2024-02-11
tags:
  - "#caching"
  - "#memory"
---
## Cache Terminology
- A **Hit**: When the application requests for data and the cache has the data, so it gives it back without needing to access memory!
- A **Miss**: When the application requests for data and the cache *does not* have the data. The cache goes to the memory and asks for the data, memory gives cache the data and then the cache returns the data to the application.
	-  Form the application viewpoint, it doesn't know whether the data came from the cache or from memory.
- **Compulsory Miss:** On first access to an object, you take a miss; there is little you can do about it.
- **Capacity Miss:** You are touching more data than can fit in the cache. If your cache were larger, you would have fewer misses.
- **Conflict Miss:** In most caches there only a limited number of places in the cache in which you can put a particular piece of data. Misses that occur because the particular place (or places) in which a piece of data must go are occupied are called conflict misses.
- **Spatial Locality:** Data that are close together, are more likely to get access than data far away.

## During a Miss
When the cache doesn't have the data, it asks the data source.
- *How much data should the data source return*
	- The size of the objects/ items in the cache is called the **block size**
	- Common block sizes (cache line sizes in hardware caches):
		- 64 Bytes in Broadwell caches
		- 4 KB in File system caches
		- Size of the object in object caches
- *What to do when the cache is full?*
	- Eviction policies
- *Organization Policies?*
	- Cache sets, indices, tags

# Direct Map Caches

## Cache Associativity