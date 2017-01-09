2) (b) List the three main components of an HTTP URL, stating how their boundaries are denoted and illustrating each one from your example.

Answer:
Protocol: http[s]|file
Host: example.com
Port: 1234
But I'd also say path: /test?query=1234

2) (c) To what extent is a URL location transparent?
Answer:
It allows clients to access a resource without knowledge of their actual address (IP-address). Instead, they are accessed through a symbolic name (a domain name)

1.2 How might the clocks in two computers that are linked by a local network be synchronized without reference to an external time source? What factors limit the accuracy of the procedure you have described? How could the clocks in a large number of computers connected by the Internet be synchronized? Discuss the accuracy of that procedure. (page 2)

Answer:
By message passing. Communication, even on a local network, implies transmission delays which is caused by latency, bandwidth and jitter. So, one clock would need to set its local clock to the received timestamp *t* including the transmission time *T<sub>trans</sub>*: *T<sub>i</sub> = t + T<sub>trans</sub>*. To synchronize clocks on the Internet, the Network Time Protocol (NTP) should be used.

1.3 Consider the implementation strategies for massively multiplayer online games as discussed in Section 1.2.2. In particular, what advantages do you see in adopting a single server approach for representing the state of the multiplayer game? What problems can you identify and how might they be resolved? (page 5)

Answer:
A single server approach (though maybe a better term would be centralized since a cluster of servers with load balancing might be used) has the advantage that the management of the state is located a single place. That eases up consistency concerns and reduces the required message passing around the system. The problem is obviously related to scalability. In massive multiplayer online games, there can be hundreds of thousands if not millions of concurrent requests and the workload should be distributed evenly between servers. So, taking a cluster architecture into use with a load balancer would definitely help with that.

2.5 A search engine is a web server that responds to client requests to search in its stored indexes and (concurrently) runs several web crawler tasks to build and update the indexes. What are the requirements for synchronization between these concurrent activities? (page 46)

Answer:
They must share a distributed index to ensure that no two crawlers maps a specific document to the same index position.

2.6 The host computers used in peer-to-peer systems are often simply desktop computers in users’ offices or homes. What are the implications of this for the availability and security of any shared data objects that they hold and to what extent can any weaknesses be overcome through the use of replication? (page 47,48)

Answer:

First in terms of security. We can assume nothing from the peers in terms of trust. So, we might figure out a way to protect the resources from being altered. One way to do so (and how its done in P2P-systems) is by computing a hash of the filename or some of the file contents to ensure that other peers can validate that the file hasn't changed. If any client altered the resources, the hash values would change.

And then in terms of availability. Again, we can do no assumptions about the availability of the nodes. They can leave the system whenever they want. So, we must make sure that the resources they hold are replicated to other nodes at all times, so that there'll always be a peer which holds the requested resource.

2.8 Give examples of applications where the use of mobile code is beneficial (page 50)

Answer:
I'm a strong opponent of applets. The browser is just as capable as providing rich offline-capable web apps. Though the correct answer to this question would probably be to say that mobile code allows for running code with better interactive response time since it does not suffer delays or variability of bandwidth associated with network communication.

2.11 Consider a simple server that carries out client requests without accessing other servers. Explain why it is generally not possible to set a limit on the time taken by such a server to respond to a client request. What would need to be done to make the server able to execute requests within a bounded time? Is this a practical option? (page 62)

Answer:
On the Internet, we can do no assumptions about the amount of time something might take. There are various reasons for this:
- Varying network conditions
- Lost messages and attempts to resubmit them (TCP does this) which adds further delay.
- The server or the network might be overloaded, for instance, by other requests.

If we are to make such assumptions, we need to have a synchronous distributed system where we can place guarantees on the maximum transmission time as well as process execution time.

2.13 The Network Time Protocol service can be used to synchronize computer clocks. Explain why, even with this service, no guaranteed bound is given for the difference between two clocks. (page 64)

Answer:
Even if the concept of global time existed and we were able to calculate the "correct" and absolutely accurate clocks of two host machines, it wouldn't change the fact that the clocks may drift from one another. Their drift rates are different, and even though it may seem incredibly irrelevant at first, the accumulated difference over time can be significant.

3.17 How does a newly installed personal computer connected to an Ethernet discover the IP addresses of local servers? How does it translate them to Ethernet addresses? (page 111)

Answer:
An Address Resolution Protocol service needs to be taken into use which will be able to translate the 48-bit Ethernet address into a 32-bit IP-address and vice-versa.

3.18 Can firewalls prevent denial of service attacks such as the one described on page 112? What other methods are available to deal with such attacks? (page 112, 125)

Answer:
No, not really. The flooding of requests can cause buffer overflow at any point, including at the firewall. And it will so, since new connection tables are generated for each request before finally being exhausted and blocking access for all legitimate users.

Obviously, you can defend yourself from DDoS attacks simply by investing in a larger bandwidth capacity. That will enable you to handle more concurrent requests, and in some cases, outperform the capacity of the attackers. Load balancing and then splitting the load between the servers with most available resources is a fine strategy as long as your network bandwidth is sufficient.

3.8 Explain how it is possible for a sequence of packets transmitted through a wide area network to arrive at their destination in an order that differs from that in which they were sent. Why can’t this happen in a local network? (page 97, 131)

Answer:
In a local network, there is no concept of *routers*. Computers are connected directly to each other (or at least they do not require for the packets to be forwarded by intermediary nodes as IP routing suggests.)

This means that the same network conditions apply. The packets never leave the network they are sent within. Thus, they will arrive in happened-before ordering.

In a Wide Area Network, packets will be routed from node to node until they finally arrive at their destination. Congestion can happen at nodes along the way, which causes transmission between intermediary nodes to slow down. In the meantime, a message sent at a later point might actually make less IP hops along the path or not meet any congestion along the way. Thus, a later packet might actually arrive before the one sent earlier.

4.1 Is it conceivably useful for a port to have several receivers? (page 148)

Answer:
In the case of multicast ports, then yes. Several processes on a host system might want to listen on the same port if they want to be notified of an event.

However, the transport protocol, whether TCP or UDP, states that communication is process-to-process based communication. So, there might be a use-case in regards to IP Multicast, but not for TCP/UDP.

4.12 Why can’t binary data be represented directly in XML, for example, by representing it as Unicode byte values? XML elements can carry strings represented as base64. Discuss the advantages or disadvantages of using this method to represent binary data. (page 164)

Answer:
The whole point of XML is to support heterogeneous environments, both in terms of platform and language. If a binary representation was used, it would imply that the sender and receiver was capable of executing the same bytecode which they may not be.

One disadvantage of XML (well, beside being horrible ugly - use JSON!) is the fact that it takes up a lot more space than a binary representation which results in a bigger payload size.

11.1 Describe some of the physical security policies in your organization. Express them in terms that could be implemented in a computerized door locking system. (page 464)

Answer:
on ITU, only students connected to the internal network are allowed to share and access resources. To get this permission, not only are you required to be connected to the network, you also need to be granted a license to actively use the network.

11.2 Describe some of the ways in which conventional email is vulnerable to eavesdropping, masquerading, tampering, replay and denial of service attacks. Suggest methods by which email could be protected against each of these forms of attack. Hint: Is e-mail encrypted? Authenticated? What could I do if I control the WiFi Access Point you're using to transmit an e-mail? (page 466)

Answer:
SMTP is based on the TCP protocol, not the TLS protocol, which makes the communication channel between sender and receiver insecure.

This means that we can protect ourselves against *some* attacks.
- We can employ shared-key or public-key encryption of the messages we sent. The receiver will then be able to decrypt the contents. This eliminates eavesdropping.

- We can make sure that message tampering and masquerading cannot happen by digitally signing the messages.

11.3 Initial exchanges of public keys are vulnerable to man-in-the-middle attacks. Describe as many defenses against it as you can. (pages 473, 511)

Answer:

If the browser or the OS comes bundled with public keys for well-known certificate authorities, it will be able to determine upon browsing a website if the issuer of the certificate is valid and if its public key matches the one it has locally already.

11.4 PGP is often used to secure email communication. Describe the steps that a pair of users using PGP must take before they can exchange email messages with privacy and authenticity guarantees. What scope is there to make the preliminary key negotiation invisible to users? (The PGP negotiation is an instance of the hybrid scheme.) (pages 493, 502)

Answer:
PGP, based on the hybrid cryptosystem, allows for an initial exchange of messages using public-key encryption before then moving on to shared-key encryption:

1. User 1 obtains the public key of user 2 (from a trusted source). Preferably a signed certificate containing the public key.

2. User 1 generates a shared key and encrypts it using user 2's public key.

3. User 2 then decrypts the shared key, using his/her private key.

4. User 1 and User 2 will now encrypt all future emails using the shared key.

11.10 In the Needham and Shroeder authentication protocol with secret keys, explain why the following version of message 5 is not secure:
*A -> B: {N<sub>B</sub>}K<sub>AB</sub>* (page 504)

Answer:
Because that wouldn't prove anything. Anyone would be able to intercept the message {N<sub>B</sub>}<sub>K<sub>AB</sub></sub> and just send that to Bob. By making a modification to the actual contents of the nonce, Alice proves to Bob that she has access to the shared key and have been able to decrypt and verify the contents.

E) Alice and Bob wants to send tamperproof messages to each other.
They know that Mallory controls their communication channel.
Bob suggests that they send their messages using TCP since the protocol contains a checksum that handles data corruption.
Argue whether Bobs suggestion solves their problem?

Answer:
It does not. Any of the IP headers can be changed, even the source and destination addresses. And so can the checksum found in the TCP headers. And so, if Mallory wants to change the message during transit, he can, and then make sure to forge the checksum and reinsert it inside the headers.

15.4 In the central server algorithm for mutual exclusion, describe a situation in which two requests are not processed in happened-before order. (page 636)

Answer:

Since the central server handles the requests in the order they arrive, it doesn’t take network delays into account. For instance, imagine two processes p1 and p2.

p1 request access to the critical section with the event e. It then submits a message to p2. This has the event f. This means that we can now define the happened-before relation e -> f. p2 now also requests access to the critical section.

Even though p1 submits the request before p2, p2’s request might still arrive at the central server before p1’s, effectively breaking the happened-before relation and ultimately the third
requirement for mutual exclusion (“If one request to enter the CS
*happened-before* another, then entry to the CS is granted in that order.”).

15.11 How, if at all, should the definitions of integrity, agreement and validity for reliable multicast change for the case of open groups? (page 647)

The validity requirement which states: “If a correct process multicasts message m, then it will eventually deliver m” only holds for closed groups. What is
required for reliable multicast is that the message m will eventually be delivered by some correct member of the group. To support open groups, the validity
requirement could be changed to this definition: “If a correct process multicasts message m, then some process p contained in group(m) will eventually deliver m”. The integrity and agreement requirements holds.

The three following packages are either fully or partially encrypted:

1. {M1}KAB

2. {M2}KBpub

3. (M3, {H(M3)}KApriv)

Where M1, M2 and M3 are messages, H is a hash function, and KAB, KApriv and KBpub are cryptographic keys. A represents Alice and B represents Bob.

M1 is encrypted with a symmetric key and M2 is encrypted with asymmetric keys.

Exercise 1: Which keys are needed to decrypt M1 and M2, respectively?

Answer:
The shared key KAB is needed to decrypt M1.
The private key KBpriv is needed to decrypt M2.

M3 is digitally signed by Alice in package 3.

Warm up: How has Alice signed her message?

Answer:
Alice has computed a digest of message M3 and then encrypted the digest using her private key.

Exercise 2: How can the recipient of M3 be sure that it is Alice who has signed the message?

Answer:
Provided that the recipient has received the public key of Alice from a trusted source, he/she should be able to compute the digest of the received message M, decrypt the digest of the message using Alice's public key and then make sure that the decrypted hash value is identical to the one he/she calculated.

Exercise 3: How do we know that the message has not been tampered with?

Answer:
Provided that the recipient has received the public key of Alice from a trusted source, he/she should be able to compute the digest of the received message M, decrypt the digest of the message using Alice's public key and then make sure that the decrypted hash value is identical to the one he/she calculated.

Exercise 4: Given package 3, how could an adversary create a message that looks like it was signed by Alice?

Consider a poorly designed hash function, H, which returns the number of characters in M.

Answer:
An adversary could prepare two different messages of the same length. He would then know that the hash value would be identical between the two. When anyone then wants to check if the signature is valid, they would compute the digest of the message, decrypt the digest encrypted with Alice's public key and see that the two hash values were identical and believe that the signature was authentic, even though it wasn't.

Exercise 5: What hash property has been violated?

Answer:
The one stating that for a message *m*, it must be hard finding another message *m'* such that H(m) = H(m').

The following is an example of a certificate:

A. Certificate type: Public key

B. Name: Your Bank (YB)

C. Public key: KYBpub

D. Certifying authority: Faythe

E. Signature: {H(field B + field C)} KFpriv

Exercise 6: If Alice obtains the above certificate, how can she validate that she is communicating with YB?

Answer:
Alice needs to retrieve the public key of 'F', whoever he is from a trusted authority 'Faythe'.

Exercise 7: What do we trust, when connecting to YB using this certificate?

Answer:
The certificate authority.
