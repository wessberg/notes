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
