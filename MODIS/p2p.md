## Peer-to-Peer systems

A scalable decentralized alternative to the client/server architecture where all nodes in the network acts both as client and server.

### Traits
- It scales linearly - for each time a new client (a peer) enters the network, so does a new server (the peer itself). Thus, the system is practically infinitely scalable.
- Decentralized - Modern P2P-systems use decentralized distributed hash tables for locating (addressing) which nodes holds which resources. The system requires no maintenance and does not rely on any single node.
- Dynamism/Churn - Nodes can enter or leave the system at any point. The system will then adapt.

### Unstructured P2P-systems
In Unstructured P2P-systems, nodes are connected to each other randomly. If a node wants to get a resource, it has no way of knowing where to look for it. It must search throughout the system for a peer that holds the resource. Most nodes will hold the most popular resources. It will then become difficult and sometimes impossible to retrieve the resource.

However, since most nodes will hold the most popular content, unstructured p2p-systems are highly robust under high rates of churn in general and under most circumstances.

### Structured P2P-systems
Here, the responsibilities of each peer are defined and structured. A distributed hash table (DHT) is used to assign ownership of specific files to specific peers. This makes it easy to retrieve a specific resource efficiently and easily, if it is rare.
In order to keep the system structured and route traffic efficiently, nodes must maintain lists of neighbors that satisfies specific conditions.

This is made possible by giving each node and resource a Global Unique Identifier (GUID) and allowing them to address each other using those.

But how do do we do so in a way that scales? If all nodes always needs to know how to address every node and resource directly, they must hold a full local copy of the DHT. That would have the two performance/consumption issues:
- The size would be linear to N (for every new node, a the size of each individual node's replica of the hash table would need to grow by one).
- When peers enters or leaves the system, a total of N messages must be passed around so that all local replicas of the hash table would be updated.

All in all that would make the system very inefficient.

### Hybrid P2P overlays
Hybrid P2P overlays are not unstructured, but they aren't fully structured either. They may have centralized index servers and they may have a hierarchy of peers and so-called *superpeers*.

**Fundamentally, they don't avoid scalability problems**.

### Key-Based Routing API
We want an API like for the system routing:
```javascript
route(G, m)    // Route message message m to G.
forward(G, m)  // Callback when forwarding m.
deliver(G, m)  // Callback when delivering m.
join(P)        // add node P to the overlay
```

This is how the P2P-system would act using a key-based routing API to route a message `m` to a peer holding a resource `G`:

#### `route(G, m)`:
when peer `P` wants to send a message `m` to a peer holding resource `G`, it invokes this method.

#### `forward(G, m)`:
Just before message `m` received at peer `P` is forwarded towards resource `G`, the P2P overlay invokes this method repeatedly on each intermediate peer in the routing path.

#### `deliver(G, m)`:
The overlay invokes this method at the peer that holds the resource `G`.

#### The task at hand
For instance, say a peer P1 wants to find resource G. A so-called *resourcegroup* (set of peers that has the resource) is also called G (same GUID as the resource has). P1 simply needs to locate a neighbor who has the resource.

There are various ways to accomplish this. As always, we want to archive at least `O(lg(N))`.

### Solutions in unstructured P2P overlays

#### Solution - Random walk
One way is simply "walking" the tree/graph at random. P1 would send a message `m=<<SEARCH(G)>>` to one of its neighbors. If the neighbor has G, it responds with it (returns it) to P1. Otherwise, it sends `m` on to some neighbor who hasn't yet seen `m`. If all neighbors have seen `m`, the original message `m` is simply returned to P1.

**But this is not scalable!**

Clearly, the worst case complexity of this would be `O(N)`. That would be the case if we had to traverse *all* nodes before finding the node that has the resource.

#### Solution - Flooding
P1 would a message `m=<<Search(G)>>` to *all* of its neighbors. If any one of them has G, it responds directly to P, otherwise it sends `m` on to *all* of its neighbors unless it has already done it.

**But this is not scalable!**
Clearly, the worst case complexity of this would be `O(N)`. That would be the case if we had to traverse *all* nodes before finding the node that has the resource. Also, we might likely end up traversing the same nodes more than once.

**This is fundamental to unstructured P2P overlays**

### Solutions in structured P2P overlays
To accomplish better asymptotic complexity, we can use a global scheme specifying which peer `P` is responsible for which resource `G`.

This works by using Distributed Hash Tables (DHT), mapping a key (GUID) to a value (resource).

Those 128-bit GUIDS are computed using a secure hashing function such as SHA-1. For files, this could be computed to the filename, for instance. For nodes, it could be to the public key with which the node is provided.
The 128-bit size makes it extremely unlikely that clashes occur between the generated GUIDs.

Hashing should be **consistent**. Key lookups should return the same resource, no matter at which peer the lookup starts.

It must also **guarantee** to find a resource if it exists in the system.

One way of structuring nodes in a GUID space is using a **Pastry** system.

### Pastry
A pastry structure is a ring of GUIDs each of which uniquely identifies a node.

A Pastry GUID is a number in the range [0, 2<sup>128</sup>[

A peer P(i)'s neighbors are the 2 peers closest to it: P(i - I) and P(i + I).

P(i) also knows about the `l` closest peers above and below it. This would be the Leaf set `{P(i - l), ..., P(i), ..., P(i + l)}`

P(i) is responsible for all resources G that are closest to P(i).

#### Simple routing
Simple routing works. It routes messages correctly, but inefficiently. It doesn't use a routing table.

1) Each node holds a *leaf set* (a vector `L` of size `2l`). This set contains the GUIDs and IP-addresses of the nodes **whose GUIDS are numerically closest on either side of its own (`l` above and `l` below)**.

Be aware that Pastry is circular. Thus, the lower neighbor of the peer with GUID 0 is the peer with GUID 2<sup>128</sup> - 1.

Pastry is responsible for maintaining these leaf sets as nodes come and go.

It works like this:
1) A node `P` receives a message `m` with destination address `D`.
2) It compares D with its own GUID `P` as well as the GUIDS of all the nodes in its' leaf set and forwards `m` to the one that is numerically closest to `D`.

From this we know that for each step, `m` will get closer and closer to `D`.

<img src="./assets/simple_routing_pastry_p2p.png" />

It is still very inefficient, though. We need to use a routing table to reach `O(log(N))`.

#### Prefix routing (routing using routing tables)
In this way of routing, each node maintains a tree-structured routing table containing GUIDs and IP addresses for a set of nodes spread **throughout the entire range of 2<sup>128</sup> possible GUID values, with increased density of coverage for GUIDs numerically close to its own.**

Here's how a routing table is structured for a node:

- GUIDs are viewed as hexadecimal values and the table classifies GUIDs based on their hexadecimal prefixes.
- The table has as many rows as there are hexadecimal digits in a GUID.
- Any row `n` contains 16 entries: an entry for each possible value of the nth hexadecimal digit, excluding the value in the local node's GUID.

<img src="./assets/pastry_p2p.png" />

The algorithm in pseudo-code per node on an incoming message `m` is then:

- If the destination `D` is within the leaf set `L` or is the current node `P`:
	-	 Forward message `M` to the element `L`<sub>`i`</sub> of the leaf set `L` with the GUID closest to `D` or the current node `P`.
- **else** use the routing table to despatch the message `m` to a node with a closer GUID:
	- Find `p`, the length of the longest common prefix of the destination node `D` and the current node `P`, and `i`, the `(p+1)`th hexadecimal digit of `D`.
	- Let R = `Routing table(P)`. Let `R[p, i]` be the element at column i, row p.
	- If `R[p, i] != null`, forward the message `m` to `R[p, i]`. This is the same as saying that we should route the message `m` to a node with a longer common prefix.
- **else** there is no entry in the routing table. Forward the message `m` to any node in the current node `P`'s Leaf set or routing table with a common prefix of length `p` but a GUID that is numerically closer to the destination node `D`.

### Joining a Pastry system.
When a new node joins a Pastry system, it:
1 - Computes a suitable GUID (typically by applying the SHA-1 hashing function to its public key).

2 - Makes contact with a nearby pastry node. *Nearby* refers to network distance, i.e. few network hops, latency.

3 - The new node, `A` sends a `join()` request to the nearby node `B`, giving itself, `A` as the destination. `B` dispatches the `join` message via Pastry in the normal way.

4 - Pastry then routes the `join` message to the existing node whose GUID is numerically closest to `A`. Lets call that node `C`.

5 - `B`, `C` and all other nodes through which the `join` message was routed on its way to `C` transmits whatever relevant parts they have in their routing tables and leaf sets to the new node `A` which then constructs its own routing table and leaf set from them, requesting additional information if necessary.

6 - `A` now transmits its own contents to all the nodes in its routing table and leaf set so that *they* can adjust their own tables and incorporate the new node.

All of this requires `O(log(n))` messages. Great!

### Nearest neighbor algorithm in Pastry

That a node is nearest to another node doesn't necessarily mean that it is close in a physical sense. Its just that there is no other node that is nearer.

Pastry includes a *nearest neighbor* algorithm which recursively measures the round-trip delay for a probe message periodically sent to each member of the leaf set of the nearest currently known pastry node.

**Pastry chooses the closest "candidate" in the routing table in terms of IP distance.**
- **First hop has long GUID but short IP distance.**
- **Following hops has exponentially longer IP-distance, as candidate sets get exponentially smaller.**
- **Last hop is the longest in IP distance.**

### Leaving a Pastry system
When a node leaves, it doesn't send a *'goodbye'* message. However, each peer always send regular *heartbeat* messages to its higher neighbor. If a peer detects its lower neighbor `P` has left, it tells its leaf set. Each peer then checks connectivity to `P` and updates their leaf set accordingly.

Routing tables entries are also checked at intervals and replaced if they have left.
