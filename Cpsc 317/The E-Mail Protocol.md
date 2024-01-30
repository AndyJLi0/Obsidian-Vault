*This not identifies and describes the roles of various components and protocols of email, trace an email protocol, describe and compare STMP, POP3, IMAP, and how they're used*

### A Case Study
Comparing a P.O. Box system vs the E-Mail system:
- For a P.O. Box system:
	- Sender sends package to its local postal office
	- Postal system forwards the package to final postal office
	- Recipient must pick up package at its local office
- For Email:
	- Sender sends message to its local server
	- The local server relays the message to the destination server (through several hops)
	- Recipient must connect to its server to retrieve ("pull") message.
### Email Components
- **User Agents  (UA)**:
	- Basically a host. Has direct user interaction. Can compose, edit, real mail messages.
		- Outlook, Gmail, etc.
- **Mail Servers**:
	- Maintains mailboxes (incoming messages for each user)
	- Queue outgoing messages to be sent to other servers (kind of like a router)
		- Communicates with other servers with STMP
## Email Protocols
#### Simple Mail Transfer Protocol (SMTP)
SMTP is used to send messages from a user agent to a server. This is called **Submission**. It's also used for servers to send messages to other servers. This is called **Relay**.
- Uses TCP to transfer messages
	- **Port 25** for message relay
	- **Port 587** for message submission, 465, 2525 if 587 is blocked
- Uses Command-response interaction (==similar to DICT==)
	- Command in single line ASCII text; responses with 3-digit status code followed by text
- **SMTP Commands:**
	- HELO (EHLO), MAIL FROM:, RCPT TO:, DATA, QUIT
- The Mail Message Format (specified by RFC 822)
	- Header lines with each line having the format "Name: value". Ends with a blank line 
	- Message body has ASCII characters only, may have multiple parts like attachments.
Think of SMTP as *manipulating an envelope*. STMP commands can instruct an SMTP server to send a message to particular receivers. It doesn't care about the contents of the message.

#### Post Office Protocol 3 (POP3)
POP3 is used by a user agent to retrieve a message form a server.
- Uses TCP as well
	- **port 995** (typically)
- 2 Modes, *Download and delete*; *Download and keep*
- stateless across sessions

#### Internet Message Access Protocol (IMAP)
IMAP similar to POP3, is also used by an user agent to retrieve message from a server
- Uses TCP
	- **Port 143** For IMAP, **Port 993** For IMAPS
- All messages are kept in the server and organized in folders
- Message state, organization and deletion status is synchronized between server and all clients across session
- Commands including create folders, move messages between them, search them, can request part of the messages.

### Web vs UA
- An email transfer example: Alice uses a UA to compose a message to Bob. UA sends the message via SMTP to mail server. Alice's local mail server sends it to Bobs mail server via SMTP. Bob invokes his UA to retrieve the message using POP3 or IMAP.
- A Web-based email transfer: Instead of using a UA and sending a message via SMTP, Alice uses a browser to compose a message and sends the message via HTTP to to the server, and Bob retrieves the message via HTTP as well.