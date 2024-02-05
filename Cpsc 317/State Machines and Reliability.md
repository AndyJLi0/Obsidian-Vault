#TransportLayer #Cpsc317 
*Protocols and state machines, and the Alternating Bit Protocol*

>[!Protocols]
>A protocol defines the *format* and the *order* of messages exchanged between two or more communicating entities. The actions taken on transmission and/or receipt of a message or other event. *A fully defined protocol must provide a proper action for any event in any state*!

Thus is why we can represent a protocol with a finite state machine. There must be a determined path or output for every message/input a in a protocol.
>[!Example]
>##### A 'Queen Greeting Protocol'
>![[State Machine Example.png]]

## Building a Reliable Protocol. Version 1.0
- For reliable delivery:
	- Send only one segment at a time
	- Identify when sending is allowable action
	- Enumerate events and actions for both sender and receiver
	- Draw state machine
- Initial scenario assumptions
	- One sender, one receiver
	- Data to send comes from upper layer
	- Segments are never lost, never corrupted.
	
![[State Machine Example 2.png]]
In the example above, we assume that nothing bad ever happens, however what bad things could happen in a real-world scenario?

### Version 1.1
- Lets also include when re-sending is required
- lets loosen our assumptions with that segments are still never lost, but *may* be corrupted.
#### Possible Events and Actions
- Receiver:
	- Segment received without problems, we send an ACK
	- Segment received corrupted, send NAK
- Sender:
	- Data ready to send, send data
	- Received ACK, get ready to send more data
	- Received NAK, re-send data
---
- Our new state machine would have a second state in the Sender called **Wait for ACK**. Sending data sets Sender's state to **Wait for ACK**, receiving a NAK = stay in state, receiving a ACK = go to **Idle**.
- Our new state machine would also have a second option in our **Wait for Data** state in Receiver. If the receiver gets corrupted data, it would send a NAK instead of an ACK.

#### Corrupted ACK/NAK
- It isn't always the senders data that can be corrupted. The Receiver's ACK or NAK could get corrupted as well, so we should have a fail-safe for that in our protocol. What can the sender to after receiving a corrupt ACK/NAK?
	- Ignore it
		- Everything stops since we are still at the **Wait for ACK stage**. Doing nothing means we stay on this state but the Receiver wont send another ACK again, so we're stuck!
	- Treat it as an ACK
		- Lets say a Sender sends some data and its corrupted. The Receiver wants to send a NAK but it gets corrupted. The sender treats it as an ACK in this case and gets ready to send more data. **Our receiver never gets the data**.
	- Treat it as a NAK
		- Lets build a similar example to the one above. If we treat the corrupt ACK (data sent properly) as a NAK, we would resend our data. **Our receiver gets the data twice**.
##### Dealing with Duplicate Transfers
We need to be able to figure out what information needs to be included to allow receiver to identify segment as a duplicate. We introduce the **Alternating Bit Protocol**.

>[!Example]
>#### Alternating Bit Protocol
>
![[Alternating Bit Protocol.png]]
>  #### Local and Authoritative DNS server Protocol
>  [DNS Protocol](The%20DNS%20Protocol.md)
>![[dns State Machine.png]]

 We have considered that packets can be corrupted. **Now what if they're lost?** We explore timeouts and lost packets in [[Lost Segments and Timeouts]].