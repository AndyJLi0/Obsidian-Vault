## Per-User State
- Web sites may use cookies to maintain state
	- "Remember me" authentication
	- Session state
	- Shopping carts, recommendation
- Process:
	- Response includes Set-Cookie header
	- Browser saves cookie associated with the server
	- Next request to the same server includes Cookies header with the same value!

### Two Cookie Styles

#### All the State in the Cookie
- Cookie has to be one line, (limits)
	- Total header size limited by servers:~ 8Kbytes
	- Individual cookie size limited by browsers: ~4Kbytes

#### State Stored Server-Side
- Cookie is used to identify entry in server database
- Cookie can be just an email address or user identifier

### Web Cache
*A way to save all the info from a site that is visited often. Good for any repeated task done by you to speed up process.*
- Browser may maintain a cached version of page
	- Reduces network utilization
	- Reduces page load time
- Cache may also be maintained in a separate host (proxy)
	- Browser sends request to proxy, with redirects to server
	- If proxy has page cached, return immediately
	- Good since proxy is usually closer to client than server.
- Conditional GET: request page only if modified
	- **Cache Validation:**
	-  You include a 'If-Modified-Since' header, and if response includes 'NOT MODIFIED', then we can get our stuff from the cache and reduce our RTT.