### Hypertext Transfer Protocol (HTTP)
- HTTP is the Web's main application layer protocol
- Client/server model
	- Client: browser that requests, receives, displays Web objects
	- Server: sends objects in response to requests
	- uses TCP, typically port 80 (443 for HTTPS)
- For each Web object (URL)
	- Client sends on request message at once (ascii text)
	- Server sends full response message at once
- **Stateless:** server maintains no information about past requests.

### HTTP Message Format
- Request is formatted using text in ASCII
	- First line: **method, URL, version**
	- Follow lines use format "Header-Name: value"
	- Lines end with CR-LF
	- Empty line ends request header
- Response follows the same rule except:
	- First line: **version, response code, response text**
### Request Methods
- **GET** method (get info from server):
	- Relevant data is in URL
	- Form data, if needed, found in URL itself
	- Often used in forms that search for data (varies)
- **POST** method (update server, send lots of data):
	- includes form input in message body
	- Often used in forms that submit new data
- **HEAD** method:
	- Similar to GET, but returns only header
	- Used to check if existing content was modified
### HTTP Response Status Codes
- **2xx**: SUCCESS
	- 200 == OK
- **3xx**:  ADDITIONAL ACTION REQUIRED
	- 301 == Moved (redirection), 304 Not Modified (use cache)
- **4xx**: CLIENT PROBLEM
	- 400 bad request, 404 not found
- **5xx**: server problem
	- 500 bad gateway, 505 HTTP version not supported

### HTTP Connections (I)
- Non-persistent HTTP (version 1.0)
	- At most one object is sent over each TCP connection (one request, one response)
	- Connection is closed as soon as data is transferred
	- Multiple objects requires multiple connections

![[Request-Response Diagram.png]]

>[!info] Number of Round Trips For HTTP 1.0
>Suppose the web page we are interested in refers to another 'object' that we need to fetch as well. we would need 4 round trips total! (2 round trips per 'object').  If we had a Web page of 10 images and a main html page, we would need 22 RTTs to retrieve the page. $2n$ RTTS for $n$ objects

### HTTP Connections (II)
- Persistent HTTP (version 1.1)
	- Multiple requests can be sent over single connection
	- Responses are received in the same order as the requests

> [!info] Number of Round Trips For HTTP 1.1
>  We would still need to make the initial TCP connection with the webpage, but we don't need to keep doing that for additional objects. $n+1$ RTTs for $n$ objects. So a base HTML page with 10 images is $2$ RTTs (plus transmission time)

### HTTP Connections (III)
- Pipelining (see [[y86 Pipelining]] for another example):
	- Assumes persistent HTTP (version 1.1)
	- Client may send several requests *without* wating for response
	- Responses are received in the same order as the requests

> [!info] Number of Round Trips for HTTP with Pipelining
> Client still needs to request base html first, so 2 RTTs for TCP connection and Base connection, then *all* objects can be requests without waiting for a response, all at once. These will arrive in order however, since that's what TCP guarantees. **Guarantees 3 round trips** (plus the transmission time). **OBJECTS MUST BE COMING FROM SAME PLACE. IF FROM DIFFERENT, WE NEED TO DO THE TCP AND BASE CONNECTION TO EACH SERVER
> **


