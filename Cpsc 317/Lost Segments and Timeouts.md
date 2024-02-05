#TransportLayer #Corruption 
>[!Features to Handle Corruption]
>- Checksums- detects errors
>- ACK Packets - detects last received data
>- Retransmissions - to overcome errors
>- Sequence Numbers - to detect duplicates
### Building a Reliable Protocol (V.2) 
Continues from [[State Machines and Reliability#Version 1.1]]. We now consider if a data segment is lost, if a ACK is lost, and how to determine which one is which if one gets lost. 
- Further need to know who determines this? Sender or receiver?
	- We will handle lost segments with timeouts on the sender side. Every time the sender sends some data, it starts a timer and waits for the Receiver's ACK. When received, it cancels the timer and sends another one when more data is ready to send.

>[!example]
>- Here is an example of an incomplete state machine. We interpret a a lost segment and corrupt data as essentially the same thing since corrupt data 'becomes' lost. The issue arises when the send re-transmits data because it *thought* it timed out, but the receiver is just slow!
>
>![[timeOutStateMachine.png]]
>- note that we only need a timeout for the sender since our receiver doesn't do anything unless requested by the host. Think of a server host relationship where the server just reacts to the hosts activity.
>- Here is an example of why the above state machine is incomplete:
>
> ![[timeOut.png]]

This begs the question: **How long should the timeout be?** Assume sequential segments experience the following sequence of RTTs. What should the timeout be?
- 10, 10, 10, 10, 10, 10
- 100, 10, 100, 100, 100, 100, 100
- 1000, 10, 10, 10, 10, 10
The answer is a very complicated formula! (don't need to know it)

## Utilization and Performance

Using the alternating bit protocol, how much of the link are we actually using? 
>[! example] 
> Assume a connection has:
> - RTT of 30 milliseconds
> - Link speed (bandwidth) of 1Gbps
> - Segment size of 1000 bytes (including headers)
> - The segment is sent only after an ACK from the receiver
> 
> We have a transmission time of $8000 / 10^9 = 0.008 ms$. Divide segment size by RTT and transmission time: $\frac{8000}{0.008 + 30} \cdot 1000 \approx 2.66 \times 10^5$. Take this value, divide by the Link speed to get a ratio, and then multiply by 100 to get a percent. we have that we use roughly $0.027 \%$ of the link!

This is horribly slow...