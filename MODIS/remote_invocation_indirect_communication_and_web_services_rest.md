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
PUT is for updates. You can be tempted to do `PUT /users/1234?name="New Name"`, but you should really put that inside the request body as XML/JSON instead since some update operations require a lot of fields to be updated on the entity.

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
Well, RESTful design guidelines are almost as divisive for web developers as religion unfortunately can be for some, but those I've read that I've agreed with (:-D) is:

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
