#Cpsc317 #definitions 
There are 5 layers (technically 7), layers to the protocol stack.  This note goes through each from the highest, to the lowest level.

## Application Layer

The application layer ensures an application can effectively communicate with other applications on different computer systems and networks. It is **not** an application, but its a way for an application accesses the network services. Protocols like *HTTP, FTP, DNS*, are **application** layer level protocols. The data at this layer is presented in a visual form that the user can understand.

## Transport Layer
The transport layer identifies the process on the local machine and destination machine responsible for the message (Who are the 2 machines?). Maybe the resource within process (*e.g.,* browser tab) . It breaks up the data into segments and insures the data arrives in order via headers, and recovers lost data (receiving end). It is also in charge if determining which order a series of messages should be processed
## Network Layer

Responsible for delivering a message form a source to a destination. Identifies if a message should be sent to a host or another router. Its overall job is to "route" the packets through routers to the destination machine and decides where the packet goes.
## Link Layer
Routes the fames (packets) to adjacent machines, not routers. Its more close contact and "direct".
## Physical Layer
Encodes the data appropriately for the physical medium.
