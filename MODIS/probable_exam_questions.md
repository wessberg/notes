1 ) List the three main components that may fail when a client process invokes a method in a server object, giving an example of a failure in each case. To what extent are these failures independent of one another? Suggest how the components can be made to tolerate one another's failures.

2) (a) Give an example of an HTTP URL.
2) (b) List the three main components of an HTTP URL, stating how their boundaries are denoted and illustrating each one from your example.
2) (c) To what extent is a URL location transparent?

3) Describe and illustrate the client-server architecture of one or more major Internet applications (e.g. the Web or email)



1.2 How might the clocks in two computers that are linked by a local network be synchronized without reference to an external time source? What factors limit the accuracy of the procedure you have described? How could the clocks in a large number of computers connected by the Internet be synchronized? Discuss the accuracy of that procedure. (page 2)

1.3 Consider the implementation strategies for massively multiplayer online games as discussed in Section 1.2.2. In particular, what advantages do you see in adopting a single server approach for representing the state of the multiplayer game? What problems can you identify and how might they be resolved? (page 5)

2.5 A search engine is a web server that responds to client requests to search in its stored indexes and (concurrently) runs several web crawler tasks to build and update the indexes. What are the requirements for synchronization between these concurrent activities? (page 46)

2.6 The host computers used in peer-to-peer systems are often simply desktop computers in users’ offices or homes. What are the implications of this for the availability and security of any shared data objects that they hold and to what extent can any weaknesses be overcome through the use of replication? (page 47,48)

2.8 Give examples of applications where the use of mobile code is beneficial (page 50)

2.11 Consider a simple server that carries out client requests without accessing other servers. Explain why it is generally not possible to set a limit on the time taken by such a server to respond to a client request. What would need to be done to make the server able to execute requests within a bounded time? Is this a practical option? (page 62)

2.13 The Network Time Protocol service can be used to synchronize computer clocks. Explain why, even with this service, no guaranteed bound is given for the difference between two clocks. (page 64)

3.17 How does a newly installed personal computer connected to an Ethernet discover the IP addresses of local servers? How does it translate them to Ethernet addresses? (page 111)

3.18 Can firewalls prevent denial of service attacks such as the one described on page 112? What other methods are available to deal with such attacks? (page 112, 125)

3.1 A client sends a 200 byte request message to a service, which produces a response containing 5000 bytes. Estimate the total time required to complete the request in each of the following cases, with the performance assumptions listed below:
i) using connectionless (datagram) communication (for example, UDP);
ii) using connection-oriented communication (for example, TCP);
iii) when the server process is in the same machine as the client.

[
	Latency per packet (local or remote, incurred on both send and receive): 5ms
	Connection setup time (TCP only): 5ms
	Data Transfer Rate: 10Mbps
	MTU: 1000 bytes
	Server request processing time: 2ms
	Assume that the network is lightly loaded
]

(page 82, 122)

3.8 Explain how it is possible for a sequence of packets transmitted through a wide area network to arrive at their destination in an order that differs from that in which they were sent. Why can’t this happen in a local network? (page 97, 131)

3.12 Show the sequence of changes to the routing tables in Figure 3.8 that will occur (according to the RIP algorithm given in Figure 3.9) after the link labelled 3 in Figure 3.7 is broken. (page 98-101)

4.1 Is it conceivably useful for a port to have several receivers? (page 148)

4.2 A server creates a port that it uses to receive requests from clients. Discuss the design issues concerning the relationship between the name of this port and the names used by clients. (page 148)

4.12 Why can’t binary data be represented directly in XML, for example, by representing it as Unicode byte values? XML elements can carry strings represented as base64. Discuss the advantages or disadvantages of using this method to represent binary data. (page 164)

11.1 Describe some of the physical security policies in your organization. Express them in terms that could be implemented in a computerized door locking system. (page 464)

11.2 Describe some of the ways in which conventional email is vulnerable to eavesdropping, masquerading, tampering, replay and denial of service attacks. Suggest methods by which email could be protected against each of these forms of attack. Hint: Is e-mail encrypted? Authenticated? What could I do if I control the WiFi Access Point you're using to transmit an e-mail? (page 466)

11.3 Initial exchanges of public keys are vulnerable to man-in-the-middle attacks. Describe as many defenses against it as you can. (pages 473, 511)

11.4 PGP is often used to secure email communication. Describe the steps that a pair of users using PGP must take before they can exchange email messages with privacy and authenticity guarantees. What scope is there to make the preliminary key negotiation invisible to users? (The PGP negotiation is an instance of the hybrid scheme.) (pages 493, 502)

11.10 In the Needham and Shroeder authentication protocol with secret keys, explain why the following version of message 5 is not secure:
*A -> B: {N<sub>b</sub>}K<sub>AB</sub>* (page 504)

A) A client attempts to synchronise with a time server. It records the round-trip times and timestamps returned by the server in the table below. Which of these times should it use to set its clock? To what time should it set it? Estimate the accuracy of the setting with respect to the server's clock. If it is known that the time between sending and receiving a message in the system concerned is at least 8 ms, do your answers change?

B) An NTP server B receives server A's message at 16:34:23.480 bearing a timestamp 16:34:13.430 and replies to it. A receives the message at 16:34:15.725, bearing B's timestamp 16:34:25.7. Estimate the offset between B and A and the accuracy of the estimate.

D) D. Consider this distributed system: There are 3 processes; each process has a state consisting of a single number k. Whenever
a process receives a message, it updates its state k to a randomly chosen new number. If the state k of a process is even, it will transmit messages at random intervals; if odd, it does nothing.

1. Explain how this system may deadlock.
2. Explain how this system may deadlock even if you can make no assumptions on the initial state.
3. Explain how the snapshot algorithm can be used to discover if the system has deadlocked.
NB! Assume that a process in odd state will nonetheless send markers as required by the algorithm. Moreover, a process which receives a message containing only a marker does not update its state k.
4. Summarise the steps of the snapshot algorithm when executed on the system with initial global states 3, 4, 7 and no messages in transit. Will the resulting snapshot allow you to conclude that the system is deadlocked?
5. Summarise the steps of the snapshot algorithm when executed on the system with initial global states 3, 5, 7 and no messages in transit. Will the resulting snapshot allow you to conclude that the system is deadlocked?

E) Alice and Bob wants to send tamperproof messages to each other.
They know that Mallory controls their communication channel.
Bob suggests that they send their messages using TCP since the protocol contains a checksum that handles data corruption.
Argue whether Bobs suggestion solves their problem?

15.4 In the central server algorithm for mutual exclusion, describe a situation in which two requests are not processed in happened-before order. (page 636)

15.6 Give an example execution of the ring-based algorithm to show that processes are not necessarily granted entry to the critical section in happened-before order. (page 637)

15.11 How, if at all, should the definitions of integrity, agreement and validity for reliable multicast change for the case of open groups? (page 647)

15.23 Show that Byzantine agreement can be reached for three generals, with one of them faulty, if the generals digitally sign their messages. (page 665)




The three following packages are either fully or partially encrypted:

1. {M1}KAB

2. {M2}KBpub

3. (M3, {H(M3)}KApriv)

Where M1, M2 and M3 are messages, H is a hash function, and KAB, KApriv and KBpub are cryptographic keys. A represents Alice and B represents Bob.

M1 is encrypted with a symmetric key and M2 is encrypted with asymmetric keys.

Exercise 1: Which keys are needed to decrypt M1 and M2, respectively?





M3 is digitally signed by Alice in package 3.

Warm up: How has Alice signed her message?

Exercise 2: How can the recipient of M3 be sure that it is Alice who has signed the message?

Exercise 3: How do we know that the message has not been tampered with?


Consider a poorly designed hash function, H, which returns the number of characters in M.

Exercise 4: Given package 3, how could an adversary create a message that looks like it was signed by Alice?

Exercise 5: What hash property has been violated?




The following is an example of a certificate:

A. Certificate type: Public key

B. Name: Your Bank (YB)

C. Public key: KYBpub

D. Certifying authority: Faythe

E. Signature: {H(field B + field C)} KFpriv

Exercise 6: If Alice obtains the above certificate, how can she validate that she is communicating with YB?

Exercise 7: What do we trust, when connecting to YB using this certificate?

Practical verification

Exercise 8: Go to wikileaks.org. What is the name of the Certificate Authority for the Wikileaks website certificate?

Exercise 9: Why do you think Wikileaks is interested in having a certificate?



10.7 Explain why using the secure hash of an object to identify and route messages to it is tamper-proof. What properties are required of the hash function? How can integrity be maintained, even if a substantial proportion of peer nodes are subverted? (pages 426, 453, Section 11.4.3)

10.11 In unstructured peer-to-peer systems, significant improvements on search results can be provided by the adoption of particular search strategies. Compare and contrast expanded ring search and random walk strategies, highlighting when each approach is likely to be effective. (page 446)


10.9 Routing algorithms choose a next hop according to an estimate of distance in some addressing space. Pastry and Tapestry both use circular linear address spaces in which a function based on the approximate numerical difference between GUIDs determines their separation. Kademlia uses the XOR of the GUIDs. How does this help in the maintenance of routing tables? Does the XOR operation provide appropriate properties for a distance metric? (Page 435)

10.1 Early file-sharing applications such as Napster were restricted in their scalability by the need to maintain a central index of resources and the hosts that hold them. What other solutions to the indexing problem can you identify? (page 428-430, 435, Section 18.4)

10.4 The guarantees offered by conventional servers may be violated as a result of:
i) physical damage to the host;
ii) Errors or inconsistencies introduced by system administrators or their managers;
iii)successful attacks on the security of the system software;
iv)hardware or software errors.

Give two examples of possible incidents for each type of violation. Which of them could be described as a breach of trust or a criminal act? Would they be breaches of trust if they occurred on a personal computer that was contributing some resources to a peer-to- peer service? Why is this relevant for peer-to-peer systems? (Section 11.1.1)

10.5 Peer-to-peer systems typically depend on untrusted and volatile computer systems for most of their resources. Trust is a social phenomenon with technical consequences. Volatility (i.e., unpredictable availability) also is often due to human actions. Elaborate your answers to Exercise 10.4 by discussing the possible ways in which each of them are likely to differ according to the following attributes of the computers used:

i) ownership;
ii) geographic location;
iii)network connectivity;
iv)country or jurisdiction.

What does this suggest about policies for the placement of data objects in a peer-to-peer storage service?


16.2 A server manages the objects a1, a2, ..., an. The server provides two operations for its clients:

read (i) - returns the value of ai;
write(i, Value) - assigns Value to ai.

The transactions T and U are defined as follows:

T: x = read(j); y = read (i); write(j, 44); write(i, 33);
U: x = read(k); write(i, 55); y = read (j); write(k, 66).

Give three serially equivalent interleavings of the transactions T and U. (page 685)

16.3 Give serially equivalent interleavings of T and U in Exercise 16.2 with the following
properties:
i) that are strict;
ii) that are not strict but could not produce cascading aborts;
iii) that could produce cascading aborts.
(page 689)

16.8 Explain why serial equivalence requires that once a transaction has released a lock on an object, it is not allowed to obtain any more locks.
A server manages the objects a1, a2, ..., an. The server provides two operations for its clients:

read(i) - returns the value of ai
write(i, Value) - assigns Value to ai

The transactions T and U are defined as follows:

T: x = read(i); write(j, 44);
U: write(i, 55); write(j, 66);

Describe an interleaving of the transactions T and U in which locks are released early with the effect that the interleaving is not serially equivalent. (page 693)


16.16 Consider optimistic concurrency control as applied to the transactions T and U defined in Exercise 16.9. Suppose that transactions T and U are active at the same time as one another. Describe the outcome in each of the following cases:

i) T’s request to commit comes first and backward validation is used.
ii) U’s request to commit comes first and backward validation is used.
iii) T’s request to commit comes first and forward validation is used.
iv) U’s request to commit comes first and forward validation is used.

In each case describe the sequence in which the operations of T and U are performed, remembering that writes are not carried out until after validation.  (page 707)

16.18 Make a comparison of the sequences of operations of the transactions T and U of Exercise 16.8 that are possible under two-phase locking (Exercise 16.9) and under optimistic concurrency control (Exercise 16.16). (page 707)



17.4 Give an example of the interleaving, of two transactions that is serially equivalent at each server but is not serially equivalent globally. (page 740)

17.10 A centralized global deadlock detector holds the union of local wait-for graphs. Give an example to explain how a phantom deadlock could be detected if a waiting transaction in a deadlock cycle aborts during the deadlock detection procedure. (page 745)

17.11 Consider the edge-chasing algorithm (without priorities). Give examples to show that it could detect phantom deadlocks. (page 746)

17.12 A server manages the objects a1, a2 , ... an. It provides two operations for its clients:

read(i) - returns the value of ai
write(i, Value) - assigns Value to ai

The transactions T, U and V are defined as follows:

T: x = read(i); write(j, 44);
U: write(i, 55); write(j, 66);
V: write(k, 77); write(k, 88);

Describe the information written to the log file on behalf of these three transactions if strict two-phase locking is in use and U acquires ai and aj before T. Describe how the recovery manager would use this information to recover the effects of T, U and V when the server is replaced after a crash. What is the significance of the order of the commit entries in the log file? (page 753-754)
