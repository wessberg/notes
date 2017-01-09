#### INTRO
I would like to begin with interprocess communication through the transport layer protocols TCP and UDP which builds on top of the IP protocol and then if we have the time, I'd also like to cover HTTP.

#### COMMUNICATION
TCP and UDP are interesting to us as developers because they are the lowest level communication protocols where we still use the abstraction of message passing.

They are based on the IP protocol which supports communication between pairs of devices identified by their IP addresses.

TCP and UDP, however, offers process-to-process communication, which allows us to target specific programs running on networked computers.

To accomplish this, these protocols do not only target an IP address, but also a given port which is a software-defined communication endpoint on a specific device.

Once an IP packet has been delivered to the destination host, the TCP- or UDP-layer software then dispatches it to a process where a TCP- or UDP-socket listens for connections on that specific port.

#### UDP
The simplest of the two protocols is UDP which is almost a transport-level replica of IP.

All it cares about is message passing, following the *best-effort* principles of IP datagrams. This means that it doesn't offer any guarantees of the actual delivery of a message.

It offers minimal overhead on top of IP datagrams, one of which is an optional checksum of the contents of the header.

The datagram approach to UDP also means that the size of UDP packages is restricted the maximum transmission unit of the network up to a theoretical limit of 64 kilobytes of size between pairs of processes.

UDP is great for situations where we want to transmit small packages with maximum performance and the least possible amount of overhead.

#### TCP
And then we have the TCP protocol which is far more sophisticated. It provides reliable delivery of arbitrarily long messages via a stream-based programming abstraction. This means that we are able to transmit messages of much greater size.

TCP is connection-oriented. This means that, before any data is transferred, a bidirectional communication channel is established between the sending and receiving process.

This, among other things, allows TCP to meet reliability guarantees which are:
- Validity: Messages must eventually be delivered, despite a packets being dropped or lost.
- Integrity: Messages must arrive uncorrupted and without duplication, exactly once.

TCP divides the stream into a sequence of data segments and transmits them as IP packets. Each of these segments gets a ascending sequence number attached to them. The receiver then uses the sequence numbers to order the received segments before placing them in the input stream at the receiving process. This is also the reason why messages of arbitrary size can be delivered using the TCP protocol.

Identifiers are associated with each IP packet which enables the recipient to detect and reject duplicates or to reorder messages that do not arrive in sender order which is another great feature of TCP.

The connection-oriented design of TCP makes it possible for TCP to guarantee the eventual delivery of messages through an acknowledgement scheme where the receiver sends acknowledgements from time to time, giving the sequence number of the highest-numbered segment in its input stream.

#### Request-Reply protocols
But most importantly, TCP provides the most obvious foundation for the design of request-reply protocols which is a form of communication designed to support the roles and message *exchanges* of typical client-server interactions.

Here, when a *request* is transmitted, an eventual *reply* is expected which also acts as an acknowledgement from the recipient. The client-server model builds on this exchange where one process acts as the client and another as the server which receives requests and replies to them.

#### HTTP
The HyperText Transfer Protocol is the foundation of the web and is based on the request-reply protocol implemented with TCP.

It specifies the format of messages involved in a request-reply exchange and supports a fixed set of methods (GET, PUT, POST, etc).

#### HTTP 1.0 vs 1.1
In HTTP1, the connection was closed after each request-reply exchange which was pretty expensive since loading a webpage obviously requires multiple HTTP requests to the same server.

HTTP1.1 fixed that with something called persistent connections which remain open over a series of request-reply exchanges and will be closed eventually, either by the client or server sending an indication to the other guy or after the connection has been idle for some time.

Now HTTP2 has arrived, offering cool thing such as Server Push as well as allowing more concurrent connections and other stuff.

#### HTTP Request contents
So, the actual contents of an HTTP request message is:
- The name of the operation (such as "GET").
- The protocol version (such as "HTTP/1.1")
- Some headers
- The relative URL of a resource
- The host
- An optional body, if the method supports it.

#### HTTP Reply contents
A *Reply* message specifies the protocol version, a status code, a 'reason', some headers and an optional message body.

A status code describes the outcome of the performed operation. For instance, a status code of 404 means "NOT FOUND" and status code 200 means "OK".
