# Remote Invocation, Indirect Communication & Web Services (REST)
> Coulouris CDK 5.1-5.3, 6.1, 6.3-6.4, 7.4, (9.1-9.4). (Ranges are inclusive, parts of 9 only cursory).

## Quick thoughts about Representational State Transfer (REST)
REST defines a set of architectural principles by which you can design Web services that focus on a system's resources.

REST has emerged in the last few years and has become the predominant Web service design model, far surpassing if not displacing SOAP- and WSDL-based interface design.

Reason: It is considerably simpler (and pretty too!)

### Design principles
- Use HTTP methods directly.
- Be stateless
- Expose directory structure-like URIs
- Transfer XML, JSON or both.

### Nouns vs Verbs
**USE NOUNS!**

Stuff like `addXXXXX` or `getXXXX` is redundant since the request method already dictates whatever action you are trying to do, whether its GET, PUT, POST or DELETE.

So please just put */somethings* in your URI.

### Use HTTP methods directly
For instance, HTTP GET is defined as a data-*producing* method that's intended to be used by a client application to retrieve a resource, to fetch data from a Web server, or to execute a query with the expectation that the web server will look for and respond with a set of matching resources.

Developers should use HTTP methods explicitly and in a way that is consistent with the protocol definition.

#### Mapping to CRUD
It is this one-to-one mapping between POST, GET, PUT, DELETE as HTTP methods and the CREATE, READ, UPDATE, DELETE methods in CRUD operations that makes it so intuitive.

- To create a resource on the server, use POST.
- To retrieve a resource, use GET
- To change the state of a resource (to update it), use PUT
- To remove or delete a resource, use DELETE.

#### GET
GET is an operation that should be free of side effects (a property known as idempotence). It is for data retrieval **only**.

That also means that `GET /adduser?name=Robert` is horrible API design from a semantical standpoint but also because it invites web crawlers and search engines to crawl or index it.

On top of that, RESTful API's should be resource-oriented at preserve directory structure-like URIs. So, if the entity is called *user*, the endpoint shouldn't be called *adduser*. Instead, call the base endpoint *users* (yes, in plural, please always do that).

#### PUT
PUT is for updates. You can be tempted to do `PUT /users/1234?name=New Name`, but you should really put that inside the request body as XML/JSON instead since some update operations require a lot of fields to be updated on the entity.

Or, and this depends a lot on whether or not you are working in a strongly typed language server-side, you could simply pass in a serialized representation of the new entity in the request body (meaning, all the fields of the new user). Please do remember that this means a bigger payload that has to be transmitted from client to server.

It is quite intuitive since the user you are trying to update are already given in the URI, (preserving `PUT /users/1234` and then moving the new contents down inside the request body).

### Be stateless
Web Service clients should send complete, independent requests. That is, the requests must include all data needed to be fulfilled so that the components in the intermediary servers may forward, route and load-balance without any state being held locally in-between requests.

A complete, independent request doesn't require the server, while processing the request, to retrieve any kind of application context or state.

The request should include within the HTTP headers and the body of the request all of the parameters, context and data needed by the server-side component to generate a response.

To better understand this, here's an example of stateful design instead. Look how complex it gets:

<img src="assets/stateful_design.png" />

The server must then know something about the context and history of previous requests.

Basically, asking for the *"next"* of something implies there are some knowledge at the server about whatever came before. Also known as state.

To achieve what the example above are trying to do, the stateless way would be to submit in the request the pagination (or offset) in terms of which resources to get.
For instance, `GET /resources?offset=3` returning the third resource and up.

Another example illustration of a stateless design solution to the one before is this:

<img src="assets/stateless_design.png" />

### Conditional GET
Is one where the *If-Modified-Since* HTTP header is set from the client on a GET request for a resource it already has in a local cache (the value of which is read from the *Last-Modified* date value on the cached one).
- If the server-side copy of the file has been modified since, it returns the resource and HTTP status code 200.
- If it hasn't, it responds with a simple HTTP status code 304 (Not Modified) and omits the actual resource.

### Expose directory structure-like URIs
It is primarily the URIs that determines how intuitive the REST Web service is going to be and whether the service is going to be used in ways that the designers can anticipate.

The URIs should be so intuitive that they are actually easy to guess.

**Think of an URI as a kind of self-documenting interface that requires little, if any, explanation or reference for a developer to understand what it points to and to derive related resources**.

A nice way to achieve this is by defining directory structure-like URIs. These are hierarchical, rooted at a single path and branching from it are subpaths that expose the service's main areas.

An example of a nice, hierarchical URI would be:
`https://api.myservice.com/users/{userId}/hobbies`.

### Other guidelines for URIs
Well, RESTful design guidelines are almost as divisive for web developers as religion unfortunately can be for some, but those I've read that I've agreed with (:-D) are:

- Keep everything lowercase.
- Substitute spaces with underscores.
- Generally only use query strings for optional stuff (like projection/filtering) and keep the rest inside the request body.
- Keep the URIs static as much as possible. Use versioning directly from the URI or at least make sure that the links stay the same, even though the actual implementation of the service changes.

### Transfer XML, JSON or both
- And please use JSON by default!

But, if you're really cool, make use of the built-in HTTP ACCEPT header that clients can assign values to (like "JSON", "XML" or "XHTML") and serve the data in whatever format that is requested.

This is called *content negotiation* which lets clients choose which data format is right for *them*.

#### camelCase your JSON!
Please format your field names in camelCase so they follow JavaScript naming conventions when you parse them into Javascript objects with `JSON.parse()`. For instance, given this JSON string:
```json
{
	"created_at": 123456,
	"followed_by": 38
}
```

```javascript
const obj = JSON.parse(responseBody);
obj.created_at; // :-(
obj.followed_by; // :-(
```

Instead, do:
```json
{
	"createdAt": 123456,
	"followedBy": 38
}
```

```javascript
const obj = JSON.parse(responseBody);
obj.createdAt; // :-D
obj.followedBy; // :-D
```

## The foundations of Remote Method Invocation (RMI)
- *Request-Reply protocols* represent a pattern on top of message passing and support the two-way exchange of messages as encountered in client-server computing. This provides the relatively low-level support for requesting the execution of a remote operation and thus also provide direct support for RPC and RMI.

- RPC (*Remote Procedure Call*) then came which allowed client programs to transparently call procedures in server programs running in separate processes and generally in different computers from the client.

- But then in the 90s, the Object-based programming model was extended to allow objects **in different processes** to communicate with one another by means of *remote method invocation (RMI)*. *RMI* is an extension of local method invocation that allows an object living in one process to invoke the methods of an object living in another process.

## Request-reply protocols
This form of communication is designed to support the roles and message exchanges of typical client-server interactions.

In the normal case, this kind of communication is synchronous because the client process blocks until the reply arrives from the server.

It can also be reliable because the reply from the server is effectively an acknowledgement to the client.

Asynchronous request-reply communication is an alternative that may be useful in situations where clients can afford to retrieve replies later.

Imagine web development where some requests can be *blocking* because they wait for the server to reply before moving on the next ones. Non-critical requests shouldn't be blocking and thus should be asynchronous.

### The request-reply protocol

<img src="assets/request_reply_communication.png" />

It matches requests to replies. It may or may not be designed to provide certain delivery guarantees.
- If UDP datagrams are used, the delivery guarantees must be provided by the request-reply protocol.
- If TCP is used, well, it is already reliable, meaning that the eventual delivery of the message is guaranteed.

### Message identifiers
any scheme that involves the management of messages to provide additional properties such as reliable message delivery or request-reply communication requires that each message have a unique message identifier by which it may be referenced. Such an identifier consists of two parts:

1. A *requestId*, which is taken from an increasing sequence of integers by the sending process.

2. An identifier for the sender process, for example, its port and Internet address.

When the value of the `requestId` reaches the maximum value for an unsigned integer (for example 2<sup>32</sup> - 1), it is reset to zero.

### Failure model of the request-reply protocol
If the primitives (`doOperation`, `getRequest`, `sendReply`) are implemented over UDP datagrams, then they suffer from the same communication failures which are:

- They suffer from omission failures.
- Messages are not guaranteed to be delivered in sender order.

But also, we do assume that processes can have crash failures. These fall into the failure model of the request-reply protocol too.

### Timeouts
Usually, if the sending process has performed a `doOperation` ( a `send(m)` operation), it begins a timer and waits for a reply before a assigned timeout value. If it doesn't get any before the timeout, it *may* be because the reply got lost - and not be cause the actual operation wasn't carried out in the first place. So, it should resubmit the request repeatedly until it either gets a reply or is reasonably sure that the delay is due to lack of response from the server rather than to lost messages.

### Discarding duplicate request methods
It may be that a request is resubmitted several times from the client (for instance, due to timeouts as just described), even though the requested operation has been carried out already.

This is where the request identifiers comes in handy. The server can see from the identifier, that the message is a duplicate and then discard it.

### Lost reply messages
Following the description from before, if the server has already sent the reply when it receives a duplicate request, it will need to execute the operation *again* to obtain the result, unless it has stored the result of the original execution (which is obviously much preferable, unless the operation is *Idempotent* in which case there is no risk associated with repeating it).

### Idempotent operation
An *Idempotent operation* is one that can be performed repeatedly with the same effect as if it had been performed exactly once.

### History
In the case from before where it would be preferable to store the result of the original execution instead of executing a duplicate request (for example, if the operation isn't Idempotent), a *history* may be used.

A *history* refers to a structure that contains a record of `reply` messages that have been transmitted. An entry in a history contains a request identifier, a message and an identifier of the client to which it was sent.

Its only purpose is to allow the server to retransmit reply messages when client processes request them.

#### Potential disadvantages
It does cost memory (e.g. space). It is now unreasonable to think of a server receiving thousands or even millions of requests and thus holding equally many `reply` objects in their history. If the server can tell when the messages will no longer be needed for transmission, it can "clean up" the history to avoid such problems.

One simple way to do that would be to internet each request from the same client as an acknowledgement of its previous reply (even though it may not be so). Therefore the history need contain only the last reply message sent to each client.

But, it would still grow over time given the fact that when a client process terminates, it does not acknowledge the last reply it has received. Thus, messages in the history will stay there and slowly grow over time unless they are automatically discarded after a limited period of time.

### Styles of request/exchange protocols
Here's 3:
- The *request (R)* protocol.
- The *request-reply (RR)* protocol.
- The *request-reply-acknowledge reply (RRA)* protocol.

<table>
	<caption>Request exchange protocols</caption>
	<tr>
		<td><strong>Protocol</strong></td>
		<td><strong>Client</strong></td>
		<td><strong>Server</strong></td>
		<td><strong>Client</strong></td>
	</tr>
	<tr>
		<td>R</td>
		<td>Request</td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td>RR</td>
		<td>Request</td>
		<td>Reply</td>
		<td></td>
	</tr>
	<tr>
		<td>RRA</td>
		<td>Request</td>
		<td>Reply</td>
		<td>Acknowledge reply</td>
	</tr>
</table>

The *R* protocol is convenient when you just want to pass a message and doesn't care about an acknowledgement of its reception from the receiver. Naturally it is based on UDP datagrams.

The *RR* protocol is useful for most client-server exchanges. Here, special acknowledgement messages are not required (they would be redundant!) because a server's reply message is regarded as an acknowledgement of the client's request message. Similarly, a subsequent call from a client may be regarded as an acknowledgement of a server's reply message.

But then we have the *RRA* protocol which is based on the exchange of 3 messages:

The additional *Acknowledge reply* message contains the *requestId* from the reply message being acknowledged. This will enable the server to discard entries from its history. The arrival of a *requestId* in an acknowledgement message will be interpreted as acknowledging the receipt of all reply messages with lower *requestIds*, so the loss of an acknowledgement message is harmless.

### Implementing the request-reply protocol with TCP streams
One major advantage is that TCP streams allows arguments and results of *any* size to be transmitted, in contrast to UDP which transfer single datagrams of a maximum size of usually 8 kilobytes.

Also, it makes sure that the request and reply messages are delivered reliably, so we don't need to worry about dealing with retransmission of messages and filtering of duplicates with histories in the request-reply protocol here.

This is the reason why the TCP protocol is chosen as the foundation for request-reply protocols: It greatly simplifies their implementation.

Obviously, if the application does not require all of the facilities offered by TCP, a more efficient, specially tailored protocol can be implemented over UDP.

### HTTP is an example of a request-reply protocol
Yup. It is a protocol that specifies the messages involved in a request-reply exchange, the methods, arguments and results, and the rules for representing them in the messages. It supports a fixed set of methods (GET, PUT, POST, etc.).

The HTTP protocol also allows for content negotiation and password-style authentication:

- *Content negotiation*: Client's requests can include information as to what data representations they can accept (Mime-types), enabling the server to choose the representation that is the most appropriate for the user.

- *Authentication*: Credentials and challenges are used to support password-style authentication. On the first attempt to access a password-protected area, the server reply contains a challenge applicable to the resource. When a client receives a challenge it gets the user to type a name and password and submits the associated credentials with subsequent requests. Cute.

#### Closing HTTP connections
In HTTP1.0, the connection is closed after **each** request-reply exchange. That's pretty expensive since loading a webpage obviously usually requires multiple requests to the same server.

So, HTTP1.1 came which uses so-called *persistent connections*. These definitely help. They remain open over a series of request-reply exchanges between client and server.

Such a connection can be closed by the client or server at any time by sending an indication to the other participant. Servers will close a persistent connection when it has been idle for a period of time.

### HTTP methods
I already covered a bit of this in the REST-part of these notes, but this is more closely related to the HTTP protocol and the formal definitions therein.

**Be warned: This is the original HTTP methods and their uses. Today, we are so used to working with RESTful principles that some of the following may seem counter-intuitive**

#### GET
Requests the resource whose URL is given as its argument.
If the URL refers to data, then the web server replies by returning the data identified by that URL.

If the URL refers to a program, then the web server runs the program and returns its output to the client.

Arguments may be added to the URL. For example, GET can be used to send the contents of a form to a program as an argument.

In REST, GET maps to the CRUD-operation *READ*.

##### Conditional GET
It can be made conditional on the date a resource was last modified (so if a local cached version of the resource exists and isn't modified, the server should return the resource again - instead the client will simply use the one it has already).

#### HEAD
Identical to GET, except it doesn't return any data. It does however return all the information *about* the data, such as the time of last modification, its type or its size. So the meta-information which is found in the header.

#### POST
Specifies the URL of a resource (for example a program) that can deal with the data supplied in the body of the request.
It is designed to deal with:
- Providing a block of data to a data-handling process, for example, submitting a web form to buy something from a web site.
- Posting a a collection of resources (adding a new one).
- Extending a database with an append operation

In REST, *POST* maps to the CRUD-operation *CREATE*.

#### PUT
Requests that the data supplied in the request is stored *with* the given URL as its, either as a modification of an existing resource or as a new resource.

Today, in REST, *PUT* maps to the CRUD-operation *UPDATE*. People would get mad if you used *PUT* for adding a new resource, as the definition otherwise suggests.

#### DELETE
The server deletes the resource identified by the given URL. Servers may not always allow this operation, in which case the reply indicates failure.

Again, today, in REST, *DELETE* maps to the CRUD-operation *DELETE*. The server should *always* reply, even if the resource wasn't deleted, counter to what the original definition states.

#### OPTIONS
The server supplies the client with a list of methods it allows to be applied to the given URL and its special requirements.

As far as I know, there really isn't anyone that takes this method into account in their API design.

#### TRACE
The server sends back the request message. Simply for diagnostic purposes.

### Message contents
#### Request
A *Request* message specifies the name of a method (such as "GET"), the URL of a resource (such as "https://domain.com/assets/static-resource.png"), the protocol version (such as "HTTP/1.1"), some headers and an optional message body (if the request method supports it; GET doesn't).

A simple HTTP *Request* could be:

```http
GET /index.html HTTP/1.1
Host: www.example.com
```

The header fields could contain lots of things, for instance the latest date of modification of the resource or acceptable content types. An authorization field could also be used to provide the client's credentials in the form of a certificate specifying their rights to access a resource.

#### Reply
A *Reply* message specifies the protocol version (such as "HTTP/1.1"), a status code (such as 200 ("OK")), a 'reason' (for instance, if the status code reflects an error), some headers and an optional message body.

## RPC (Remote Procedure Call)
The whole point is to make the programming of distributed systems look similar if not identical to conventional programming. That is, achieving a high level of distribution transparency.

In RPC, procedures on remote machines can be called as if they are procedures in the local address space. It is the underlying RPC system that hides the important aspects of distribution, including the encoding and decoding of parameters and results, the passing of messages and the preserving of the required semantics for the procedure call.

### Programming with interfaces
In order to control the possible interactions between modules, an explicit *interface* is defined for each module.

The interface specifies the procedures and variables that can be accessed from other modules.

Modules are implemented so as to hide all the information about them except that which is available through its interface. So long as its interface remains the same, the implementation may be changed without affecting the users of the module.

In a distributed program, the modules can run in separate processes.
In the client-server model in particular, each server provides a set of procedures that are available for use by clients.

For example, a file server would provide procedures for reading and writing files.
The term *service interface* is used to refer to the specification of the procedures offered by a server, defining the types of the arguments of each of the procedures.

### Benefits with interfaces in distributed systems
- As with any form of modular programming, programmers are concerned only with the abstraction offered by the service interface and need not be aware of implementation details.

- Extrapolating to distributed systems, programmers also do not need to know the programming language or underlying platform used to implement the service.

- This approach provides natural support for software evolution in that implementations can change as long as the interface remains the same.

### Constraints
- It is not possible for a client module running in one process to access the variables in a module in another process.

- Passing references as parameter-arguments are not suitable when the caller and procedure are in different processes. Rather, input parameters are passed to the remote server by sending the *values* of the arguments in the request message and then supplying them as arguments to the operation to be executed in the server.

- Addresses in one process are not valid in another remote one. Thus, addresses cannot be passed as arguments or returned as results of calls to remote modules.

### interface Definition Languages (IDLs)
An RPC mechanism can be integrated with a particular programming language if it includes an adequate notation for defining interfaces, allowing input and output parameters to be mapped onto the language's normal use of parameters.

This is useful when all the parts of a distributed application can be written in the same language.

*However*, many services are written in other languages. It would be beneficial to allow programs written in a variety of languages to access them remotely.

*Interface Definition Languages (IDLs)* are designed to allow procedures implemented in different languages to invoke one another.

An IDL provides a notation for defining interfaces in which each of the parameters of an operation may be described as for input or output in addition to having its type specified.

CORBA was quite popular for defining the CORBA IDL.

### RPC call semantics
There are different choices to make regarding the required delivery guarantees:

- *Retry request message*: Controls whether to retransmit the request message until either a reply is received or the server is assumed to have failed.

- *Duplicate filtering*: Controls when retransmissions are used and whether to filter out duplicate requests at the server.

- *Retransmission of results*: Controls whether to keep a history of result messages to enable lost results to be retransmitted without re-executing the operations at the server.

Combinations of these choices lead to a variety of possible semantics for the reliability of remote invocations as seen by the invoker.

These semantics are:

- **Maybe semantics**: With *maybe* semantics, the RPC may be executed once or not at all. If the result message has not been received after a timeout and there are no retries, it is uncertain whether the procedure has been executed. *Maybe* semantics is useful only for applications in which occasional failed calls are acceptable.

- **At-least-once semantics**: The invoker receives either a result, in which case the invoker knows that the procedure was executed at least once, or an exception informing it that no result was received. *At-least-once* semantics can be achieved by the retransmission of request messages. If the operations in a server can be designed so that all of the procedures in their service interfaces are idempotent operations, then *at-least-once* semantics may be acceptable.

- **At-most-once-semantics**: Here, the caller receives either a result, in which case the caller knows that the procedure was executed once, or an exception informing it that no result was received, in which case the procedure will have been executed either once or not at all. A pretty good all-around choice I'd say.

### Transparency
Even though RPC aims to make remote procedure calls as much like local ones with no distinction in syntax whatsoever, it doesn't change the fact that that remote procedure calls are more vulnerable than local ones, since they involve a network, another computer and another process and all the potential issues that may contain, including the significant latency.

But it does give location and access transparency!

## Implementation of RPC
RPC is generally implemented over a request-reply protocol like the ones mentioned.

The client that accesses a service includes one *stub procedure* for each procedure in the service interface.

### Stub procedure
It behaves like a local procedure to the client, but instead of executing the call, it marshals the procedure identifier and the arguments into a request message, which it sends via its communication module to the server.

When the reply message arrives, it unmarshals the results.

The server process contains a dispatcher together with one server stub procedure and one service procedure for each procedure in the service interface.
The dispatcher selects one of the server stub procedures according to the procedure identifier in the request message.

The server stub procedure then unmarshals the arguments in the request message, calls the corresponding service procedure and marshals the return values for the reply message.

<img src="assets/stub_procedures.png" />

### Compilation
The client and server stub procedures and the dispatcher can be generated automatically by an interface compiler from the interface definition of the service.

## Indirect communication

The essence is to communicate through an intermediary and hence have no direct coupling between the sender and the one or more receivers.

Indirection is a fundamental concept in computer science. In terms of distributed systems, the concept of indirection is increasingly applied to communication paradigms.

With indirect communication, you achieve:

- *Space uncoupling*: The sender does not know or need to know the identity of the receiver(s), and vice versa. Because of this space uncoupling, the system developer has many degrees of freedom in dealing with change: participants can be replaced, updated, replicated or migrated.

- *Time uncoupling*: The sender and receiver(s) can have independent lifetimes. They do not need to exist at the same time to communicate. This has important benefits.

**Indirect communication is often used in distributed systems where change is anticipated.**

It is also heavily used for events dissemination in distributed systems where the receivers may be unknown and liable to change.

<table>
	<caption>Space and time coupling in distributed systems</caption>
	<tr>
		<td></td>
		<td><strong>Time-coupled</strong></td>
		<td><strong>Time-uncoupled</strong></td>
	</tr>
	<tr>
		<td><em>Space coupling</em></td>
		<td>Communication directed towards a given receiver or receivers. Receiver(s) must exist at that moment in time. Examples of this are Message passing and remote invocation.</td>
		<td>Communication directed towards a given receiver or receivers. Sender(s) and receiver(s) can have independent lifetimes.</td>
	</tr>
	<tr>
		<td><em>Space uncoupling</em></td>
		<td>Sender does not need to know the identity of the receiver(s). Receiver8s) must exist at that moment in time. An example of this is IP multicast (messages are directed towards the multicast group, not any particular receiver(s)).</td>
		<td>Sender does not need to know the identity of the receiver(s). Sender(s) and receiver(s) can have independent lifetimes. Most indirect communication paradigms are based on this.</td>
	</tr>
</table>

## Publish-subscribe systems
Sometimes also referred to as *distributed event-based systems*

This is one of the most used indirect communication techniques.

A Publish-subscribe system is one where *publishers* publish structured events to an event service and *subscribers* express interest in particular events through *subscriptions* which can be arbitrary patterns over the structured events.

The task of the Publish-subscribe system is to match subscriptions against published events and ensure the correct delivery of *event notifications*.

Obviously, since a given event will be delivered to potentially many subscribers, publish-subscribe is fundamentally a one-to-many communications paradigm.

### Characteristics of publish-subscribe systems
There are two main characteristics:

- *Heterogeneity*: When event notifications are used as a means of communication, components in a distributed system that were not designed to interoperate can be made to work together. All that is required is for the event-generating objects to publish the types of events they offer.

- *Asynchronicity*: Notifications are sent asynchronously by event-generating publishers to all the subscribers that have expressed an interest in them to prevent publishers needing to synchronize with subscribers.

### Reliability for publish-subscribe
When it comes to delivery guarantees, that depends on the application requirements. For instance, if IP multicast is used to send notifications to a group of receivers, the failure model will relate to the one described for IP multicast and thus will not guarantee that any particular recipient will receive a particular notification message.

### The programming model for Publish-Subscribe systems
Is based on a small set of operations:

- Publishers disseminate an event *e* through a `publish(e)` operation.
- Subscribers express an interest in a set of events through subscriptions with a `subscribe(f)` operation where `f` refers to a filter - that is, a pattern defined over the set of all possible events.
- Subscribers can later revoke this interest through a corresponding `unsubscribe(f)` operation.
- When events arrive at a subscriber, the events are delivered using a `notify(e)` operation.

<img src="assets/publish_subscribe.png" />

### Expressiveness
Expressiveness is determined by the subscription (filter) model, with a number of schemes defined:

- *Channel-based*: Publishers publish events to named channels and subscribers then subscribe to one of these named channels to receive all events sent to that channel.

- *Topic-/Subject-based*: Each notification is expressed in terms of a number of fields, with one field denoting the topic. Subscriptions are then defined in terms of the topic of interest. For instance, a subscription could be defined in terms of *cats* or *cats/funny* where the directory structure way of defining the topic or subject makes the latter even more specific.

- *Content-based*: A generalization of topic-based approaches allowing the expression of subscriptions over a range of fields in an event notification. So, it is a query defined in terms of compositions of constraints over the values of event attributes. An example could be a subscription on the topic *cats* whose  *age* is greater than 8. That's pretty expressive, but definitely comes with some implementation challenges.

- *Type-based*: Linked to object-oriented approaches. Here, subscriptions are defined in terms of *types* of events and matching is defined in terms of types or subtypes of the given filter. These can be integrated elegantly into programming languages and they can check the type correctness of subscriptions.

### Publish-subscribe implementation issues
#### Centralized versus distributed implementations
##### Centralized
The simplest way to implement at publish-subscribe system is to centralize the implementation in a single node with a server on that node acting as an event broker. Publishers then publish events to this broker, and subscribers send subscriptions to the broker and receive notifications in return.

But like in all other centralized indirect communication systems, it lacks resilience and scalability, since the centralized broker represents a single point for potential system failure and a performance bottleneck.

##### Decentralized
Here, the centralized broker is replaced by a *network of brokers* that cooperate to offer the desired functionality. Can survive node failure.

You could also have a fully *peer-to-peer* implementation of a publish-subscribe system which would be the ultimate solution. Here, all nodes act as brokers, cooperatively implementing the required event routing functionality.

## (Distributed) Message queues
Whereas publish-subscribe provide a one-to-many style of communication, message queues provide a *point-to-point* service using the concept of a message queue as an indirection, thus achieving the desired properties of space and time uncoupling.

They are point-to-point in that the sender places the message into a queue, and it is then removed by a single process.
### The programming model of Distributed Message Queues
It is actually very simple.

Producer processes can *send* messages to a specific queue and consumer processes can then *receive* messages from this queue.

That's it!

### *Receive* styles
There are three:
- A *blocking receive*: Blocks until an appropriate message is available.
- A *non-blocking receive*: Checks the status of the queue and returns a message if available, or a not available indication otherwise. Is a polling operation.
- A *notify* operation, which will issue an event notification when a message is available in the associated queue.

### Ordering
It is normally FIFO-ordered (*first-in-first-out*), but many message queues also support the concept of priority, with higher-priority messages delivered first.

A message consists of a *destination*, *metadata* including the priority and delivery mode, as well as a *body*.

<img src="assets/message_queue.png" />

### Persistence
**Messages are persistent - they will be stored indefinitely until they are consumed**.

### Reliability
Message queues ensures that any message sent is eventually delivered (validity) and the message received is identical to the one sent, and no messages are delivered twice (integrity). This makes message queues reliable.

### Implementation issues
Again, the key issue is choosing between a centralized and distributed implementation. As always, the centralized version suffers from no scalability as well as a single point of failure where the distributed once is adds a lot to the complexity of the implementation.

## SOAP
The SOAP specification states:
- How XML is to be used to represent the contents of individual messages.
- How a pair of single messages can be combined to produce a request-reply pattern.
- The rules as to how the recipients of messages should process the XML elements that they contain.
- How HTTP and SMTP should be used to communicate SOAP messages.
