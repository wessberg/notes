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

#### Solution - Random walk
One way is simply "walking" the tree/graph at random. P1 would send a message `m=<<SEARCH(G)>>` to one of its neighbors. If the neighbor has G, it responds with it (returns it) to P1. Otherwise, it sends `m` on to some neighbor who hasn't yet seen `m`. If all neighbors have seen `m`, the original message `m` is simply returned to P1.

** But this is not scalable! **
Clearly, the worst case complexity of this would be `O(N)`. That would be the case if we had to traverse *all* nodes before finding the node that has the resource.
