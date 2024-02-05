#cpsc317 #java
### What is a Socket?
- A **socket** is one endpoint of a two-way communication link between two programs running on the network. A socket is bound to a port number so that the **TCP** (transport) layer can identity the application that data is destined to be sent to.
- When a **client** tries to make a connection request to a **host** and gets accepted, the server gets a new socket bound to the same local port and has its remote endpoint set to the address and port of the client. 
	- The server needs a new socket to be able to listen to the original socket for other connection requests *while* tending to the needs of the already connected client.
	- A socket is created on the client side to communicate with the server.
- An **endpoint** is a combo of an IP address and a port number. Every TCP connection can be uniquely identified by its two endpoints so a client and server can have multiple connections.

### Reading from and Writing to a Socket
1. Open a socket.
2. Open an input stream and output stream to the socket.
3. Read from and write to the stream according to the server's protocol.
4. Close the streams.
5. Close the socket.
Here is an example with an echo server and echo client:
```java
String hostName = args[0];
int portNumber = Integer.parseInt(args[1]);

try (
    Socket echoSocket = new Socket(hostName, portNumber);        // 1st statement
    PrintWriter out =                                            // 2nd statement
        new PrintWriter(echoSocket.getOutputStream(), true);
    BufferedReader in =                                          // 3rd statement 
        new BufferedReader(
            new InputStreamReader(echoSocket.getInputStream()));
    BufferedReader stdIn =                                       // 4th statement 
        new BufferedReader(
            new InputStreamReader(System.in))
)

String userInput;
while ((userInput = stdIn.readLine()) != null) {
    out.println(userInput);
    System.out.println("echo: " + in.readLine());
}
```
