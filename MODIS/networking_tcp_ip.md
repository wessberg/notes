# Networking (TCP/IP)
>  Coulouris 3.1-3.4, 4.1-4.3, 4.5, 7.4

### Principles
The principles on which computer networks are based include protocol layering, packet switching, routing and data streaming.

Computers and other devices that use the network for communication purposes are referred to as *hosts*. The term *node* is used to refer to any computer or switching device attached to a network.

A *subnet* is a unit of routing (delivering data from one part of the network to another). It is a collection of nodes that can all be reached on the same physical network.

The internet's infrastructure includes an architecture and hardware and software components that effectively integrate diverse subnets into a single data communication service.

### Network issues in distributed systems
#### Performance
Issues regarding how fast individual messages can be transferred between two interconnected computers.

-	*Latency* is the delay that occurs after a send operation is executed and before data **starts** to arrive at the destination device. **It can be measured as the time required to transfer an empty message.** (only considering network latency).
- *Data transfer rate* is the speed at which data can be transferred between two computers in the network once transmission has begun, usually quoted in bits per second.

**The time required for a network to transfer a message containing *length* bits between two devices is:**
`Message transmission time = latency + length / data transfer rate`

Obviously, this is only true as long as the *length* of the message doesn't exceed the maximum determined by the underlying network technology. In that case, longer messages must be segmented and transmission time will take longer (due to the added latency overhead).

### What determines latency & data transfer rate (+ perceived perf)
- Data transfer rate of a network is determined primarily by its physical characteristics.
- Latency is determined primarily by software overheads, routing delays and a load-dependent statistical element arising from conflicting demands for access to transmission channels.

The perceived transfer rate of a network is greatly influenced by latency. The reason being that most of the things we do over the network usually consists of a lot of small message passing back and forth. The packet size doesn't affect latency. Latency is therefore of greater significance than transfer rate in determining average performance.

### Total system bandwidth
The *total system bandwidth* of a network is a measure of throughput. That is the total volume of traffic that can be transferred across the network in a given time.

In some LAN-technologies such as Ethernet, the full transmission capacity of the network is used for *every* transmission. In these cases, **the system bandwidth is the same as the data transfer rate**.

But, in most WANs (Wide Area Networks), messages can be transferred on several different channels simultaneously. Here, the total system bandwidth bears no direct relationship to the transfer rate. For instance, the performance of networks greatly deteriorates in conditions of overload (when there are too many messages in the network at the same time).

The precise effect of overload on the latency, data transfer rate and total system bandwidth of a network depends strongly on the network technology.

## Types of networks
This table seems pretty outdated, but I've made a few changes to it.
<table>
	<tr>
		<td>Name</td>
		<td>Wireless</td>
		<td>Example</td>
		<td>Range</td>
		<td>Bandwidth</td>
		<td>Latency</td>
	</tr>
	<tr>
		<td>LAN</td>
		<td>No</td>
		<td>Ethernet</td>
		<td>1-2 kms</td>
		<td>High</td>
		<td>Low</td>
	</tr>
	<tr>
		<td>WAN</td>
		<td>No</td>
		<td>IP Routing</td>
		<td>worldwide</td>
		<td>-</td>
		<td>-</td>
	</tr>
	<tr>
		<td>MAN</td>
		<td>No</td>
		<td>ATM</td>
		<td>2-50 kms</td>
		<td>-</td>
		<td>Low</td>
	</tr>
	<tr>
		<td>WPAN</td>
		<td>Yes</td>
		<td>Bluetooth (IEEE 802.15.1)</td>
		<td>10-30m</td>
		<td>Low</td>
		<td>Low</td>
	</tr>
	<tr>
		<td>WLAN</td>
		<td>Yes</td>
		<td>WiFi (IEEE 802.11)</td>
		<td>0.15 - 1.5 km</td>
		<td>-</td>
		<td>Low</td>
	</tr>
	<tr>
		<td>WiMAX</td>
		<td>Yes</td>
		<td>WiFi (IEEE 806.16)</td>
		<td>5 - 50 km</td>
		<td>-</td>
		<td>Low</td>
	</tr>
	<tr>
		<td>WWAN</td>
		<td>Yes</td>
		<td>3G</td>
		<td>1 - 5 km</td>
		<td>-</td>
		<td>High</td>
	</tr>
</table>

### Personal Area Networks (PANs)
A subcategory of local networks in which the various digital devices carried by a user are connected by a low-cost, low-energy network.

They are not of much significance because few users wish to be encumbered by a network of wires on their person. Instead, Wireless Personal Area Networks (WPANs) are of increasing importance due to the number of personal devices such as smartphones, tablets, smartwatches, etc. carried by many people.

### Local Area Networks (LANs)
LANs carry messages at relatively high speeds between computers connected by a single communication medium (for instance twisted copper wire, coaxial cable or optical fibre). A *segment* is a section of cable that serves a department or a floor of a building and may have many computers attached.

No routing of messages is required within a segment since the medium provides **direct connections between all of the computers connected to it.**

Larger local networks, such as those that serve a campus or an office building, are composed of many *segments* interconnected by switches or hubs.

In LANs, total system bandwidth is high and latency is low, except when message traffic is very high.

Ethernet has emerged as the dominant technology for wired LANs. However, Ethernet lacks the latency and bandwidth guarantees needed by many multimedia applications. ATM networks were developed to fill this gap, but their cost has inhibited their adoption in LAN applications.

Instead, High-speed Ethernets have been deployed in a switched mode that overcomes these drawbacks to a significant degree, although not as effectively as ATM.

### Wide area Networks (WANs)
WANs carry messages at lower speeds between nodes that are often in different organizations and may be separated by large distances (e.g. different cities, countries or continents). They communicate through a set of routers (communication circuits linking a set of dedicated computers).

### Metropolitan Area Networks (MANs)
Is based on high-bandwidth copper and fibre optic cabling. Allows for transmission of video, voice and other data over distances up to 50 kilometers.

DSL (Digital Subscriber Line) and cable modem connections are examples of MANs. DSL typically uses ATM switches located in telephone exchanges to route digital data onto twisted pairs of copper wire (using high-frequency signaling on the existing wiring used for telephone connections.)

### Wireless Local Area Networks (WLANs)
WLANs are designed for use in place of wired LANs to provide connectivity for mobile devices (or simply to remove the need for a wired infrastructure).

### Wireless Metropolitan Area Networks (WMANs)
An example of such a network is the IEEE 802.16 WiMAX standard. It aims to provide an alternative to wired connections to home and office buildings and to supersede 802.11 WiFi networks in some applications.

### Wireless Wide Area Networks (WWANs)
Most mobile phone networks are based on digital wireless network technologies such as the GSM (Global System for Mobile communication) standard. The underlying technology is referred to as UMTS (Universal Mobile Telecommunications System).

### Internetworks
A communication subsystem in which several networks are linked together to provide common data communication facilities that overlay the technologies and protocols of the individual component networks and the methods used for their interconnection. In Internetworks,
a variety of local and wide area network technologies can be integrated to provide the networking capacity needed by each group of users. It can be thought of as a *virtual network* constructed by overlaying an internetwork layer on a communication medium that consists of the underlying networks, routers and gateways.

The Internet is an example of an Internetwork, and the TCP/IP protocols are an example of this integration layer.

### Network errors
Reliability of the underlying data transmission is really high in all cabled networks, but not necessarily so in wireless networks where packets are frequently lost due to external inference.

However, packets may be lost in *all* types of network due to processing delays and buffer overflow at switches and at the destination node (which is by far the most common cause of packet loss).

## Network principles
The basis for all computer networks is a packet-switching technique. This enables data packets addressed to different destinations to share a single communications link, unlike the circuit-switching technology that underlies conventional telephony.

Packets are queued in a buffer and transmitted when the link is available.

**Communication is asynchronous**: Messages arrive at their destination after a delay that varies depending upon the time that packets take to travel through the network.

### Packet transmission
*Messages* to be transmitted is data items of arbitrary length. **Before a message is transmitted, it is subdivided into *packets*.**

A packet is a sequence of binary data (an array of bits or bytes) of restricted length, together with addressing information sufficient to identify the source and destination computers. These packets are used:
- So that each computer in the network can allocate sufficient buffer storage to hold the largest possible incoming packet.
- To avoid the undue delays that would occur in waiting for communication channels to become free if long messages were transmitted without subdivision.

### Switching schemes
Switching systems are required to transmit information between nodes. Four popular kinds are:

- **Broadcast**: Broadcasting is a transmission technique **that involves no switching**. Everything is transmitted to every node, and it is up to receivers to notice transmissions addressed to them. **Ethernet, a LAN technology, are based on broadcasting**. So is Wireless networking to some extend, but in the absence of fixed circuits, the broadcasts are arranged to reach nodes grouped in cells.

- **Circuit switching**: Old-school analog telephone networks work like this: A caller dials a number, the pair of wires from the phone to the local exchange is then connected by an automatic switch at the exchange to the pair of wires connected to the recipients phone.

This is also called **The plain old telephone system (POTS)**. That's typically a *circuit-switching network*.

- **Packet switching**: A *store-and-forward* network is one where instead of making and breaking connections to build circuits, the packets are just forwarded from their source to their destination. There is a computer at each switching node. Each packet arriving at a node is first stored in memory at the node and then processed by a program that transmits it on an outgoing circuit which transfers the packet to another node that is closer to its ultimate destination.

This is just like the postal system works, except this concept has found a digital implementation in the way of packet switching.

- **Frame relay**: In a *store-and-forward* network as described in *Packet switching*, switching packets through each network node takes some time (usually a maximum of a few milliseconds). However, since the internet is build on *store-and-forward*, packets may need to travel hundreds of kilometers. It is not unusual for this to take upwards of 200 milliseconds.
The *Frame relay* switching method overcome the delay problems by switching small packets on the fly. The actual switching nodes (which are usually special-purpose parallel digital processors) route frames based on the examination of their first few bits. Frames as a whole are not stored at nodes but pass through them at short streams of bits.

ATM networks are a prime example: high-speed ATM networks can transmit packets across networks consisting of many nodes in a few tens of microseconds. Impressive!

### Protocols
A protocol is a well-known set of rules and formats to be used for communication between processes in order to perform a given task.

The definition of a protocol has two important parts to it:
- A specification of the sequence of messages that must be exchanged.
- A specification of the format of the data in the messages.

For example, a *transport protocol* transmits messages of any length from a sending process to a receiving process. To send a message, a process issues a call to a transport protocol module, passing it a message in the specified format. It is then the transport software that does the hard work of subdividing the message into packets, formatting it and transmitting it to the destination via the *network protocol*, another even lower-level protocol.

The receiving end then performs inverse transformations to regenerate the message before passing it to a receiving process.

### Protocol layers
**Network software is arranged in a hierarchy of layers.** Each layer presents an interface to the layers above it that extends the properties of the underlying communication system.

<img src="assets/protocol_layering.png" />

A layer is represented by a module in every computer connected to the network.
Each module appears to communicate directly with a module at the same level in another computer in the network, but in reality, data is not transmitted directly between the protocol modules at each level. Instead, each layer of network software communicates by local procedure calls with the layers above and below it.

Each layer provides a service to the layer above it and extends the service provided by the layer below it.

#### Physical layer
The communication medium, be it copper, fibre optic cables, satellite communication, radio transmission, etc and by analogue signaling circuits that place signals on the communication medium at the sending node and sense them at the receiving node.

At receiving nodes, data items are received and passed upwards through the hierarchy of software modules, being transformed at each stage until they are in a form that can be passed to the intended recipient process.

#### Protocol suites
A complete set of protocol layers is referred to as a *protocol suite* or a *protocol stack*.

<img src="assets/osi_protocol_model.png" />

#### OSI protocol
The Open Systems Interconnection (OSI) reference model for protocol stacks/suites is a foundation for a developing protocol standards that will meet the requirements of open systems. It defines seven layers:

<table>
	<caption>OSI protocol summary</caption>
	<tr>
		<td><strong>Layer</strong></td>
		<td><strong>Description</strong></td>
		<td><strong>Examples</strong></td>
	</tr>
	<tr>
		<td>Application</td>
		<td>Protocols at this level are designed to meet the communication requirements of specific applications, often defining the interface to a service.</td>
		<td>HTTP, FTP, SMTP, CORBA, IIOP</td>
	</tr>
	<tr>
		<td>Presentation</td>
		<td>Protocols at this level transmit data in a network representation that is independent of the representations used in individual computers, which may differ. Encryption is also performed in this layer, if required.</td>
		<td>TLS security, CORBA data representation</td>
	</tr>
	<tr>
		<td>Session</td>
		<td>At this level, reliability and adaption measures are performed, such as detection of failures and automatic recovery</td>
		<td>SIP</td>
	</tr>
	<tr>
		<td>Transport</td>
		<td>This is the lowest level at which messages (rather than packets) are handled. Messages are addressed to communication ports attached to processes. Protocols in this layer may be connection-oriented or connectionless.</td>
		<td>TCP, UDP</td>
	</tr>
	<tr>
		<td>Network</td>
		<td>Transfers data packets between computers in a specific network. In a WAN or an Internetwork, this involves the generation of a route passing through routers. In a single LAN, no routing is required.</td>
		<td>IP, ATM virtual circuits.</td>
	</tr>
	<tr>
		<td>Data link</td>
		<td>Responsible for transmission of packets between nodes that are directly connected by a physical link. In a WAN, transmission is between pairs of routers or between routers and hosts. In a LAN, it is between any pair of hosts.</td>
		<td>Ethernet MAC, ATM cell transfer, PPP</td>
	</tr>
	<tr>
		<td>Physical</td>
		<td>The circuits and hardware that drive the network. It transmits sequences of binary data by analogue signaling, using amplitude or frequency modulation of electrical signals (or cable circuits), light signals (on fibre optic circuits) or other electromagnetic signals (on radio and microwave circuits)</td>
		<td>ISDN</td>
	</tr>
</table>

#### Performance cost of protocol layering
The transmission of an application-level message via a protocol stack with *N* layers typically involves *N* transfers of control to the relevant layer of software in the protocol suite and taking *N* copies of the data as a part of the encapsulation mechanism. All of these overheads result in data transfer rates between application processes that are much lower than the available network bandwidth.

#### Packet assembly
It is usually the transport layer that divides and reassembles packages from and back into messages.

The network-layer protocol packets consist of a *header* and a *data field*. The data field is usually variable in length. The maximum length is called the *maximum transfer unit (MTU)*. If the length of a message exceeds the MTU of the underlying network layer, it must be fragmented into chunks of the appropriate size, with sequence numbers for use on reassembly, and transmitted in multiple packets.

**An example of a MTU is for Ethernet where it is 1500 bytes.**

#### Ports
The transport layer's task is to provide a network-independent message transport service between pairs of network ports. Ports are software-defined destination points at a host computer. They are attached to processes, enabling data transmission to be addressed to a specific process at a destination node.

#### Addressing
The transport layer is responsible for delivering messages to destinations with *transport addresses* that are composed of the *network address* of a host computer and a *port number*.

- A Network address is a numeric identifier that uniquely identifies a host computer and enables it to be located by nodes that are responsible for routing data to it. In the internet, every host computer is assigned an IP number which identifies it and the subnet to which it is connected as well as enabling data to be routed to it from any other node.

- Services such as HTTP and FTP have been allocated *contact port numbers* which are registered with a central authority.

- Port numbers below 1023 are defined as *well-known ports* whose use is restricted to privileged processes in most operating systems.

#### Packet delivery
Two approaches:

##### *Datagram packet delivery*

Delivery of each packet is a 'one-shot' process: No setup is required. Once the packet is delivered, the network retains no information about it. In a datagram network, a sequence of packets transmitted by a single host to a single destination may follow different routes.

Every datagram packet contains the full network address of the source and destination hosts.

The Internet's network layer (IP), Ethernet and most wired and wireless LAN technologies are based on datagram delivery.

##### *Virtual circuit packet delivery*

Some network-level services implement packet transmission in a manner analogous to a telephone network.

A virtual circuit is set up before packets can pass from A to B. At each node along the route, a table entry is made, indicating which link should be used for the next stage of the route.

**Each network-layer packet contains only a virtual circuit number in place of the source and destination addresses. The addresses are not needed, because packets are routed at intermediate nodes by reference to the virtual circuit number. When a packet reaches its destination, the source can be determined from the virtual circuit number.**

### Routing
Is a function that is required in all networks except those LANs such as Ethernets that provide **direct connections between all pairs of attached hosts**.

#### Adaptive routing
In large networks, *adaptive* routing is employed: The best route for communication between two points in the network is re-evaluated periodically, taking into account the current traffic in the network and any faults such as broken connections or routers.

#### Routing algorithm

Unless the source and destination hosts are on the same LAN, the packet has to be transmitted in a series of hops, passing through router nodes.

It is the responsibility of a *routing algorithm* implemented by a program in the network layer at each node to determine the best routes for the transmission of packets to their destinations.

Such an algorithm must:
1. Make decisions that determine the route taken by each packet as it travels through the network. In packet-switched network layers such as IP, it is determined separately for each packet.
2. It must dynamically update its knowledge of the network based on traffic monitoring and the detection of configuration changes or failures.

These decisions are made on a hop-by-hop basis.

#### A simple routing algorithm: Distance vector algorithm

Routing in networks is an instance of the problem of path finding in graphs. The **shortest path algorithm provides the basis for the distance vector method**.

- Routing tables is held at each of the routers for the network.
- Each row provides:
	-	A *link* field that specifies the outgoing link for packets addressed to the destination.
	- A *cost* field which is a calculation of the vector distance (the number of hops to the given destination)
	- The destination node.
- A single entry exists for each possible destination.
- When a packet arrives at a router, the destination address is extracted and looked up in the local routing table.
- How routing tables are maintained:
	-	Because each routing table specifies only a single hop for each route, the construction or repair of the routing information can proceed in a distributed fashion.
	- A router exchanges information about the network with its neighboring nodes by sending a summary of its routing table using a *Router information protocol (RIP)*.
	- RIP actions are:
		- *Periodically and whenever the local routing table changes*, send the table to all accessible neighbors.
		- *When a table is received from a neighboring router*, if the received table shows a route to a new destination or a better route to an existing destination, update the local table with the new route. If the table was received on link *n*, replace the cost in the local table with the new cost.

### Congestion control
When the load at any particular link or node approaches its capacity, queues will build up at hosts trying to send packets and at intermediate nodes holding packets whose onward transmission is blocked by other traffic. If the queues keep on growing, they will eventually reach the limit of available buffer space.

At this point, the node has no other option but to drop further incoming packets.

**Instead of allowing packets to travel through the network until they reach over-congested nodes where they will have to be dropped, it would be better to hold them at earlier nodes until the congestion is reduced**.

This will result in increased delays for packets but will not significantly degrade the total throughput of the network.

*Congestion control* is the a name given to techniques for achieving this.

Congestion control is achieved by informing nodes along a route that congestion has occurred and that their rate of packet transmission should therefore be reduced.

**All datagram-based network layers such as IP and Ethernets rely on end-to-end control of the traffic. That is, the sending node must reduce the rate at which it transmits packets based *only* on information that it receives from the receiver.**

Something called *choke packets* can be supplied to the sending node by explicit transmission, requesting a reduction in transmission rate.

### Internetworking
To build an integrated network (an internetwork), we must integrate many subnets, each of which is based on one of the network technologies.
We need:

1. A unified internetwork addressing scheme that enables packets to be addressed to any host connected to any subnet.

2. A protocol defining the format of internetwork packets and giving rules according to which they are handled.

3. Interconnecting components that route packets to their destinations in terms of internetwork addresses, transmitting the packets using subnets with a variety of network technologies.

Lets see how to internet achieves this:

- (1) (a unified addressing scheme) is provided by IP addresses.
- (2) (A protocol defining the format of packets and their handling) is the IP protocol.
- (3) (how to route packets) is performed by *internet routers*.

#### Ethernet hubs
Ethernet hubs are simply means of connecting together several segments of Ethernet cable, all of which forms a single Ethernet at the network protocol level.

#### Ethernet switches
Connects several Ethernets, routing the incoming packets only to the Ethernet to which the destination host is connected.

#### Routers
As stated, routing is required in all networks except for Ethernets and Wireless networks in which all hosts are connected by a single transmission medium. Routers are responsible for forwarding the Internetwork packets that arrive on any connection to the correct outgoing connection. They maintain routing tables for that purpose.

#### Bridges
Bridges link networks of different types. Some bridges link several networks, and these are referred to as bridge/routers because they also perform routing functions.

#### Hubs
Simply a convenient way of connecting hosts and extending segments of Ethernet and other Broadcast LAN technologies. They have a number of sockets to each of which a host computer can be connected.

#### Switches
Perform a similar function to routers, but for LAN's (typically Ethernet) only. They interconnect several separate Ethernets, routing the incoming packets to the appropriate outgoing network.

#### Advantages of switches over hubs.
Switches separate the incoming traffic and transmit it only on the relevant outgoing network, reducing congestion on the other networks to which they are connected.

#### Tunneling
A pair of nodes connected to separate networks **of the same type** can communicate through another type of network by constructing a protocol 'tunnel'. It is a software layer that transmits packets through an alien network environment.
