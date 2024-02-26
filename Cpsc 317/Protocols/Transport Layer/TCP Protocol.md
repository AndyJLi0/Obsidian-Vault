---
date: 2024-02-12
---
# TCP Overview

![[TCP Overview.png]]

# TCP Segment Format
![[TCP Segment Overview.png]]

## TCP Sequence Numbers and ACKs
- Each byte of a TCP segment has a sequence number
- The sequence number of a segment is the sequence number of its first byte
- ACKs correspond to the first sequence number not yet received (similar to Go-Back-N, but byte-based and +1)
-  Receiver stores packets in its own window ([[Selective-Repeat]])
-  Cumulative ACKs (getting ACK 11 means segments up to 10 have been received). ([[Windowing Protocols#Go-Back-N Strategy]])
### Seq. #'s
- byte stream "number" of first byte in segment's data.
	- **Host A**, sends 1 byte with *seq = 42*. **Host A** sends another 1 byte, now with *seq = 43*.
### ACKs
- The ACK number is the *next* sequence number expected from the other side. Since **Host A** sends a segment with *seq = 42*, **Host B** sends ACK of *ACK = 43 (now expecting 43)*. However **Host B** also has its OWN sequence number, with lets just arbitrarily choose *seq = 79*. Now **Host A** will *ACK = 80 (expects 80)*, and **Host B** will send *seq =  80* right after.


### TCP Windows
- **Receiver** has a flow control window measured in **bytes**
- **Sender** has a congestion flow window measured in **segments**