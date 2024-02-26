*see [[TCP Protocol]] For its transport layer counterpart*
## UDP 
[[The DNS Protocol]] uses UDP.
- Very simple segment/message format
	- 8 byte header:
		- Source port #, dest port #, length of entire UDP segment, and checksum
	- The application data goes behind the header. Wouldn't want an empty body so at least 1 byte body, and therefore the **shortest UDP message length would be 9 bytes**.
	- $2^{16} -$ length of the header would be the greatest UDP message length 
- Note that the IP address isn't in the header but its located in the network layer header.

### Checksums
- Detects errors (bit flip or something)
	- If error is detected, UDP just discards the packet
- Sender:
	- Computers some function on the data
	- Adds the checksum value to the data
	- Sends the data and the checksum
- Receiver:
	- Compares the same function on the received data
	- Check if computed checksum equals received checksum value.
		- **NO-** error
		- *yes-* no error
- Note that no all errors can be detected. 
#### Checksum Algorithm
- Treat data as 16 bit integers
- Function: addition (1's complement, carry out added back in)
- Checksum is another 1's complement of the computed value
- Verifying is computing the same function over the data and checksum (correct if 0)
##### Example:
``` (5 bit integer)
	1 1 0 1 0
	0 1 0 0 1
	1 0 1 1 0
	1 0 0 1 1 
	---------
	0 1 1 0 0 (sum and carry)
		  1 0 (carry)
	----------(add the carry)
	0 1 1 1 0
	--------- (ones complement)
	1 0 0 0 1 (done)
When verifying, use the same 4 '5 bit' integers, along with the sending check sum value, should get all zeroes if no bits have been fliped.
```

- 1 bit flip: detectable
- 2 bits flipped: detectable if on different columns, undetectable if in the same column.
