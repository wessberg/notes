# Coordination and Agreement
> Coulouris Chapter 15 except Section 15.4.

It's about how processes coordinate their actions and agree on shared values in distributed systems despite failures.

### Reliable channel
A reliable channel *eventually* delivers a message to the recipient.

In a *synchronous* system, not only will the message eventually be delivered, it will also be so within a specified time bound.

There are two kinds of failure detectors: *unreliable failure detectors* and *reliable failure detectors*.

### Unreliable failure detectors
An unreliable failure detector may produce one of two values when given the identity of a process *unsuspected* or *suspected*. A result of unsuspected signifies that the detector has recently received evidence suggesting that the process has **not** failed.

For example a message was recently received from it but of course the process may have failed since then.

A value of *suspected* signifies that the failure detector found some indication that the process may have failed. For example, it may be that no message from the process has been received for more than a nominal maximum length of silence. The suspicion may be misplaced. For example, the process could be functioning correctly but be on the other side of a network partition or it could be running more slowly than expected.

### Reliable failure detector
A Reliable failure detector is one that is always accurate in detecting a processes failure. Here, a value of *Failed* means that the detector has determined that the process has crashed.

### Implementing an unreliable failure detector
Use the following algorithm.
- Each process *P* sends a *"P is here"* message to every other process and it does this every *T* seconds.
- The failure detector uses an estimate of the maximum message transmission time of *D* seconds.
- if the local failure detector at process *q* does not receive a *"p is here"* message within *T* + *D* seconds of the last one, then it reports to *q* that *p* is *Suspected*.
- However, if it subsequently receives a *"p is here"* message, then it reports to *q* that *p* is *OK*.

### Setting timeout values
For small values of *T*, the failure detector is likely to suspect non-crashed processes many times and much bandwidth will be taken up with *"P is here"* messages.
If we choose a large timeout value, a week for instance, then crashed processes will often be reported as *Unsuspected*.

A practical solution to this problem is to use timeout values that reflect the observed network delay conditions. If a local failure detector receives a *"P is here"* message in 20 seconds instead of the expected maximum of 10 seconds, it can reset it's timeout value for *P* accordingly. The failure detector remains unreliable and it's answers to queries are still only hints, but the probability of its accuracy increases.

In a synchronous system, the failure detector could be mode reliable by saying that *D* is not an estimate but an absolute bound on message transmission times. The absence of a *"p is here"* message within *T* + *D* seconds entitles the local failure detector to conclude that *p* has crashed.

## Distributed mutual exclusion
When a collection of processes share a resource or a collection of resources, mutual exclusion is often required to prevent interference and ensure consistency when accessing resources.

**This is called the *critical section* problem**
In a distributed system, neither share variables nor facilities, so we need a solution to *distributed mutual exclusion*: One that is based solely on message passing.

## Algorithms for mutual exclusion
The application-level protocol for executing a critical section is as follows:
```java
interface CriticalSection {
	/**
	 * Enter critical section. Block if necessary.
	 */
	void enter();

	/**
	 * Access shared resources in critical section.
	 */
	void resourceAccesses();

	/**
	 * Leave critical section. Other processes may now enter.
	 */
	void exit();
}
```

### Essential requirements:
- ME1 (safety): At most one process may execute in the critical section (CS) at a time.
- ME2 (liveness): Requests to enter and exit the critical section eventually succeed.
- ME3 (ordering): If one request to enter the CS *happened-before* another, then entry to the CS is granted in that order.

We evaluate the performance of algorithms for mutual exclusion according the the following criteria:
- The *bandwidth* consumed, which is proportional to the number of messages sent in each *entry* and *exit* operation.
- The *client delay* incurred by a process at each *entry* and *exit* operation.
- The algorithm's effect upon the *throughput* of the system. This is the rate at which the collection of processes as a whole can access the critical section, given that some communication is necessary between successive processes. We measure the effect using the *synchronization delay* between one process exiting the critical section and the next process entering it. The throughput is greater when the synchronization delay is shorter.

### The central server algorithm
It employs a server that grants permission to enter the critical section.

- To enter a critical section, a process sends a request message to the server and awaits a reply from it. Conceptually, the reply constitutes a token signifying permission to enter the critical section.
	-	If no other process has the token at the time of the request, the server responds immediately, granting the token.
	- If the token is currently held by another process, the server does not reply, but queues the request.
- When a process exits the critical section, it sends a message to the server, giving it back the token.
	-	If the queue of waiting processes is not empty, the server chooses the oldest entry in the queue, removes it and replies to the corresponding process. The chosen process then holds the token.

#### Performance
It takes two messages to enter the critical section, even if no process currently occupies it.
1. A *request*.
2. A *grant*.

These delay the requesting process by the time required for a round-trip.

Exiting the critical section takes one *release* message. This one should not delay the process. It doesn't need to wait for the exit message to arrive before continuing to do work.

### A ring-based algorithm
It *is* possible to arrange mutual exclusion between *N* processes without requiring an additional process is to arrange them in a logical ring. This requires only that each process *p<sub>i</sub>* has a communication channel to the next process in the ring, *p<sub>(1+i)modN</sub>*.

The idea is that exclusion is conferred by obtaining a token in the form of a message passed from process to process in a single direction around the ring.

- If a process does not require to enter the critical section when it receives the token, then it immediately forwards the token to its neighbor.
- If a process requires the token, it will wait until it receives it, and then retains it.
- To exit the critical section, the process sends the token on to its neighbor.

This, again, doesn't respect the M3-requirement (ordering). Processes may exchange messages independently of the rotation of the token, and a process may gladly accept the token when it arrives at it, even though a process later in the chain actually asked for it before it).

#### Performance
The algorithm continuously consumes network bandwidth (except when a process is inside the critical section). The processes send messages around the ring even when no process requires entry to the critical section.

- The delay experienced by a process requesting entry to the critical section is between 0 messages (when it has just received the token) and *N* messages (when it has just passed on the token).
- To exit the critical section requires only one message.

### An algorithm using multicast and logical clocks
The basic idea is that processes that require entry to a critical section *multicast* a request message, and can enter it only when all other processes have replied to this message.

**It meets all the mutual exclusion requirements (ME1 - ME3).**

- Each process bear distinct numeric identifiers.
- Each process keeps a Lamport clock.
- Each process has a communication channel to all other processes.
- Messages requesting entry are of the form *<T, p<sub>i</sub>>* where *T* is the sender's timestamp and *p<sub>i</sub>* is the sender's identifier.
- Each process records its state of being outside the critical section (*RELEASED*), wanting entry (*WANTED*) or being in the critical section (*HELD*) in a variable *state*.
- If a process requests entry and the state of all other processes is *RELEASED*, then all processes will reply immediately to the request and the requester will obtain entry. If some process is in the state *HELD*, then that process will not reply to requests until it has finished with the critical section, and so the requester cannot gain entry in the meantime.
- If two or more processes request entry at the same time, then whichever process's request bears the lowest timestamp will be the first to collect *N-1* replies, granting it entry next. If the requests bear equal Lamport timestamps, the requests are ordered according to the processes corresponding identifiers.
- When a process requests entry, it defers processing requests from other processes until its own request has been sent and it has recorded the timestamp *T* of the request. This is so that processes make consistent decisions when processing requests.

#### Performance
Gaining entry takes *2(N - 1)* messages. *N - 1* to multicast the request, followed by *N - 1* replies. Client delay in requesting entry is again a round-trip time.

### Maekawa's voting algorithm
States that for a process to enter a critical section, it is not necessary for all of its peers to grant it access. Processes need only obtain permission to enter from subsets of their peers, as long as the subsets used by any two processes overlap.

We can think of processes as voting for one another to enter the critical section. A '*candidate*' must collect sufficient votes to enter.

Each process holds a *Voting set*, *V<sub>i</sub>*, all of which are of the same length. The optimal solution has size *K = √N*.

- To obtain entry to the critical section, a process *p<sub>i</sub>* sends *request* messages to all *K* members of *V<sub>i</sub>* (including itself). *p<sub>i</sub>* cannot enter the critical section until it has received all *K* reply messages.
- When a process *p<sub>j</sub>* in *V<sub>i</sub>* receives *p<sub>i</sub>*'s *request* message:
 - it sends a *reply* message immediately unless either its state is *HELD* or it has already replied ('voted') since it last received a *release* message.
 - Otherwise it queues the message (in the order of its arrival) but does not yet reply.
- When a process receives a *release* message, it removes the head of its queue of outstanding requests (if the queue is nonempty), and sends a *reply* message (a 'vote') in response to it.
- To leave the critical section, *p<sub>i</sub>* sends *release* messages to all *K* members of *V<sub>i</sub>* (including itself).

The algorithm does *not* respect the ME3 requirement (ordering) *and* it is deadlock prone.

An adaption has been made (Sanders, 1987) which makes it deadlock free and makes it respect the ME3 requirement. Here, processes queue outstanding requests in *happened-before* order.

#### Performance
The bandwidth utilization is 2√N messages per entry to the critical section and √N messages per exit.

### Fault tolerance
We need to consider:
- What happens when messages are lost?
- What happens when a process crashes?

None of the previous algorithms would tolerate the loss of messages if the channels were unreliable.

- The ring-based algorithm cannot tolerate a crash failure of any single process.
- Maekawa's algorithm can tolerate *some* process crash failures: If a crashed process is not in a voting set that is required, then its failure will not affect the other processes.
- The central server algorithm can tolerate the crash failure of a client process that neither holds nor has requested the token.

## Elections
An algorithm for choosing a unique process to play a particular role is called an *election algorithm*.

An example is a variant of the central-server algorithm where the 'server' is chosen from among the processes that need to use the critical section. An election algorithm is needed for this choice.

**It is essential that *all* processes agree on the choice.**

If the process that plays the role of server wishes to retire, then another election is required to choose a replacement.

#### Calling an election
We say that a process *calls the election* if it takes an action that initiates a particular run of the election algorithm. An individual process does not call more than one election at a time, but in principle the *N* processes could call *N* concurrent elections.

At any point in time, a process *p<sub>i</sub>* is either a *participant* (meaning that it is engaged in some run of the election algorithm) or a *non-participant* (meaning that it is not currently engaged in any election).

An important requirement is for the choice of elected process to be unique, even if several processes call elections concurrently.

It *is* possible that two processes could decide independently that a coordinator process has failed and both call elections.

**We require that the elected process be chosen as the one with the largest identifier**.

Each process has a variable *elected<sub>i</sub>* which will contain the identifier of the elected process.
When the process first becomes a participant in an election, it sets this variable to a special value, for instance `undefined` to denote that it is not yet defined.

### Requirements during any particular run of the algorithm
- E1 (safety): A participant process *p<sub>i</sub>* has *elected<sub>i</sub> = `undefined`* or *elected<sub>i</sub> = P* where *P* is chosen as the non-crashed process at the end the run with the largest identifier.
- E2 (liveness): All processes *p<sub>i</sub>* participate and eventually either set *elected<sub>i</sub> != undefined* or crash.

### A ring-based election algorithm
The algorithm of **Chang and Roberts** arranges processes in a logical ring.

Each process has a communication channel to the next process in the ring, *p<sub>(i + 1)mod N</sub>*, and all messages are sent clockwise around the ring.

**The goal is to elect a single process called the *coordinator*, which will be the process with the largest identifier.**

- Initially, every process is marked as a *non-participant*.
- Any process can begin an election.
- It proceeds by marking itself as *participant*, placing its identifier in an *election* message and sends it to its clockwise neighbor.
- When a process receives an *election* message, it compares the identifier in the message with its own.
- **On forwarding an election message in *any* case, the process marks itself as a *participant**.
	-	If the arrived identifier is greater, it forwards the message to its neighbor.
	- If the arrived identifier is smaller:
		- If the receiver is *not* a participant, it substitutes its own identifier in the message and forwards it.
		- If it *is* a participant, it does not forward the message.
	- If the arrived identifier is identical to the receivers own, then this process's identifier must be the greatest, and it becomes the coordinator.
- The Coordinator marks itself as a *non-participant* and sends an *elected* message to its neighbor, announcing its election and enclosing its identity.
- When a process *p<sub>i</sub>* receives an *elected* message, it marks itself as a *non-participant*, sets its variable *elected<sub>i</sub>* to the identifier in the message and, unless it is the new coordinator, forwards the message to its neighbor.

#### Performance
If only a single process starts an election, the worst-performing case is when its anti-clockwise neighbor has the highest identifier. A total of *N-1* messages are then required to reach this neighbor which will not announce its election until its identifier has completed another circuit, taking a further *N* messages. The *elected* message is then sent *N* times, making *3N - 1* messages in all.

**The ring-based algorithm is useful from a theoretical standpoint, but not of much practical value since it doesn't tolerate any failures**.

### The bully algorithm
The bully algorithm allows processes to crash during an election. It does, however, assume that message delivery between processes is reliable.

**The bully algorithm assumes that the system is synchronous: It uses timeouts to detect a process failure**.

All processes can communicate with all processes that has higher identifiers (thus, processes must know which processes has higher identifiers than their own).

There are 3 types of messages in this algorithm:
- An *election* message: Sent to announce an election.
- An *answer* message: Sent in response to an *election* message.
- A *coordinator* message: Sent to announce the identity of the elected process (the new coordinator).

A process begins an election when it notices (through timeouts) that the coordinator has failed. Several processes may discover this concurrently.

#### Failure detection
Since it is synchronous, we can construct a reliable failure detector:
Maximum message transmission delay: *T<sub>trans</sub>*.
Maximum processing delay: *T<sub>process</sub>*.
Upper bound: *T = 2T<sub>trans</sub> + T<sub>process</sub>*.
If no response arrives within time *T*, then the local failure detector can report that the intended recipient of the request has failed.

The process that knows it has the highest identifier can elect itself as the coordinator simply by sending a *coordinator* message to all processes with lower identifiers.

A process with a lower identifier can begin an election by sending an *election* message to those processes that have a higher identifier and awaiting *answer* messages in response. If none arrives within time *T*, the process considers itself the coordinator and sends a *coordinator* message to all processes with lower identifiers announcing this. Otherwise, the process waits a further period *T'* for a *coordinator* message to arrive from the new coordinator. If none arrives, it begins another election (this is what makes it fault tolerant: a process might fail before getting to send the *coordinator* message).

- If a process *p<sub>i</sub>* receives a *coordinator* message, it sets its variable *elected<sub>i</sub>* to the identifier of the coordinator contained within it and treats that process as the coordinator.
- If a process receives an *election* message, it sends back an *answer* message and begins another election unless it has begun one already.
- When a process is started to replace a crashed process, it begins an election. If it has the highest process identifier, it will decide that it is the coordinator and announce this to the other processes. It will then become the coordinator, even though the current coordinator is functioning. **It is for this reason that the algorithm is called the 'bully' algorithm**.

**It is not guaranteed to meet the safety condition E1 if processes that have crashed are replaced by processes with the same identifiers or if the assumed timeout values turn out to be inaccurate**.

#### Performance
In the best case, the process with the second-highest identifier notices the coordinators failure. Then it can immediately elect it self and send *N-2* coordinator messages.

In the worst case, *O(N<sup>2</sup>)* messages are required. This will happen when the process with the lowest identifier first detects the coordinators failure. Then, *N-1* processes altogether begins election, each sending messages to processes with higher identifiers.

## Coordination and agreement in group communication
Has to do with how to achieve the desired reliability and ordering properties across all members of a group.

**We are particularly seeking reliability in terms of the properties of validity, integrity and agreement, and ordering in terms of FIFO ordering, causal ordering and total ordering**.

Here, processes are members of groups, which are the destinations of messages sent with the *multicast* operation.

- The operation `multicast(g: Group, m: Message)` sends the message *m* to all members of the group *g* of processes.
- The operation `deliver(m: Message)` delivers a message sent to a group by multicast to the calling process.
- Every message *m* carries the unique identifier of the process `sender(m)` that sent it, and the unique destination group identifier `group(m)`.

### R-multicast (Reliable multicast)

A reliable multicast is one that satisfies the following properties:
- *Integrity*: A correct process *p* delivers a message *m* at most once. Also, *p is an element of `group(m)`* and *m* was supplied to a *multicast* operation by `sender(m)`.
- *Validity*: If a correct process multicasts message *m*, then it will eventually deliver *m*.
- *Agreement*: If a correct process delivers message *m*, then all other correct processes in `group(m)` will eventually deliver *m*.

The *agreement* condition is related to atomicity (*"all or nothing"*), applied to delivery of messages to a group. If a process that multicasts a message crashes before it has delivered it, then it is possible that the message will not be delivered to any process in the group, but if it is delivered to *some* correct process(s), then *all* other correct processes will deliver it.

### B-multicast (Basic multicast)
B-multicast is a type of multicast that ensures that a correct process will eventually deliver the message, as long as the multicaster does not crash. We call is delivery primitive *B-deliver*.

It can be implemented by using a reliable one-to-one *sed* operation:
- To `B-multicast(g: Group, m: Message)`: for each process where p is an element of g, `send(p, m)`;
- On `receive(m)` at *p*: `B-deliver(m)` at *p*.

**B-multicast is not IP-multicast**.

### B-multicast vs R-multicast
- Basic multicast does *not* consider failures.
- Reliable multicast tolerates certain types of failures.

### Implementing reliable multicast over B-multicast
To R-multicast a message, a process B-multicasts the message to processes in the destination group (including itself). When the message is B-delivered, the recipient in turn B-multicasts the message to the group (if it is not the original sender), and then R-delivers the message.

Since a message may arrive more than once, duplicates of the message are detected and not delivered.

Validity is obtained since a correct process will eventually B-deliver the message to itself.

By the integrity property of the underlying communication channels used in *B-multicast*, the algorithm also satisfies the integrity property.

Agreement follows from the fact that every correct process B-multicasts the message to the other processes after it has B-delivered it. If a correct process does not R-deliver the message, then this can only be because it never B-delivered it. That in turn can only be because no other correct process B-delivered it either: therefore none will R-deliver it.

### Reliable multicast over IP multicast
We can also use a combination of IP multicast, acknowledgements attached to other messages and negative acknowledgements.

This R-multicast protocol is based on the observation that IP multicast communication is often successful.

Processes attach acknowledgements onto the messages they send to the group. Processes only send a separate response message when they detect that they missed a message. A response indicating the absence of an expected message is known as a *negative acknowledgement*.

**The description assumes that groups are closed**.

- Each process *p* maintains a sequence number (*S<sup>p</sup><sub>g</sub>*) for each group *g* to which it belongs. The sequence number is initially zero.
- Each process records the sequence number (*R<sup>q</sup><sub>g</sub>*) of the latest message it has delivered from process *q* that was sent to group *g*.
- For *p* to R-multicast a message to group *g*, it attaches the value *S<sup>p</sup><sub>g</sub>* and acknowledgements of the form *<q, R<sup>q</sup><sub>g</sub>*> onto the message. An acknowledgement states, for some sender *q*, the sequence number of the latest message from *q* destined for *g* that *p* has delivered since it last multicast a message.
- The multicaster *p* then IP-multicasts the message with its attached values to *g* and increments *S<sup>p</sup><sub>g</sub>* by one.

The attached values in a multicast message enables the recipients to learn about messages that they have not received.

- A process R-delivers a message destined for *g* bearing the sequence number *S* from *p* if and only if *S = R<sup>p</sup><sub>g</sub> + 1* and it increments *R* by one immediately after delivery.
- If an arriving message has *S <= R<sup>p</sup><sub>g</sub>*, then *r* has delivered the message before and it discards it.
- If *S > R<sup>p</sup><sub>g</sub> + 1* or if *R > R<sup>q</sup><sub>g</sub>* for an enclosed acknowledgement *<q, R>*, then there are one or more messages that is not yet received (and which are likely to have been dropped in the first case).
- It keeps any message for which *S > R<sup>p</sup><sub>g</sub> + 1* in a *hold-back queue*. Such queues are often used to meet message delivery guarantees. It requests missing messages by sending negative acknowledgements, either to the original sender or to a process q from which it has received an acknowledgement *<q, R<sup>q</sup><sub>g</sub>>* with *R<sup>q</sup><sub>g</sub>* no less than the required sequence number.

The **integrity** property follows from the detection of duplicates and the underlying properties of IP multicast (which uses checksums to expunge corrupted messages).

The **validity** property holds because IP multicast has that property.

For agreement we require that a process can always detect missing messages and that it will always receive a further message that enables it to detect the omission. We therefore assume that processes retain copies of the message they have delivered - indefinitely.

### Uniform properties
Any property that holds whether or not processes are correct is called a *uniform property*. The previous algorithms expect processes to be *correct*.

We define uniform *agreement* as follows:
*If a process, whether it is correct or fails, delivers a message m, then all correct processes in `group(m)` will eventually deliver m.*

Uniform agreement allows a process to crash after it has delivered a message, while still ensuring that all correct processes will deliver the message.

### Ordered multicast
Basic multicast delivers messages to processes in an arbitrary order.
For some applications, it may be important that different kinds of events are observed in the same order by all processes in the system.

#### Common ordering requirements.
- *Total ordering*: If a correct process delivers message *m* before it delivers *m'*, then any other correct process that delivers *m'* will deliver *m* before *m'*.
- *Causal ordering*: If `multicast(g, m) -> multicast(g, m')`, where `->` is the *happened-before* relation induced only by messages sent between members of *g*, then any correct process that delivers *m'* will deliver *m* before *m'*.
- *FIFO ordering*: If a correct process issues `multicast(g, m)` and then `multicast(g, m')`, then every correct process that delivers *m'* will deliver *m* before *m'*.
- Hybrid solutions such as total-causal and total-FIFO.

Causal ordering implies FIFO ordering.

### Overlapping groups
In general, processes need to be members of multiple overlapping groups. For instance, a process may be interested in events from multiple sources and thus join a corresponding set of event-distribution groups.

We can extend the ordering definitions to **global orders** in which we have to consider that if message *m* is multicast to *g* and if message *m'* is multicast to *g'*, then both messages are addressed to the members of the set of processes that *g'* has, but *g* doesn't (the relative compliment).

- *Global FIFO ordering*: If a correct process issues `multicast(g, m)` and then `multicast(g', m')`, then every correct process in the set of elements found in *g'* that isn't in *g* that delivers *m'* will deliver *m* before *m'*.
- *Global Causal ordering*: If `multicast(g, m) -> multicast(g', m')` where `->` is the *happened-before* relation induced by any chain of multicast messages, then any correct process in the set of elements in *g'* that isn't in *g* that delivers *m'* will deliver *m* before *m'*.
- *Pairwise total ordering*: If a correct process delivers message *m* sent to *g* before it delivers *m'* sent to *g'*, then any other correct process in the set of elements found in *g'* that isn't in *g* that delivers *m'* will deliver *m* before *m'*.
- *Global total ordering*: Let'<' be the relation of ordering between delivery events. We require that '<' obeys pairwise total ordering and that it is acyclic - under pairwise total ordering, '<' is not acyclic by default.

One way of implementing these orders would be to multicast each message *m* to the group of *all* processes in the system (which would effectively be a *broadcast*). Each process either discards or delivers the message according to whether it belongs to `group(m)`. This would be very inefficient! **A Multicast should involve as few processes as possible beyond the members of the destination group**.

## Consensus and related problems
Has to with problems of *agreement*. Roughly speaking, the problem is for processes to agree on a value after one or more of the processes has proposed what that value should be.

There are three related agreement problems:
- Byzantine generals
- Interactive consistency
- Totally ordered multicast

### Definition of the consensus problem
To reach consensus, every process *p<sub>i</sub>* begins in the *undecided* state and *proposes* a single value *v<sub>i</sub>*, drawn from a set *D = {1, 2, ..., N}*. The processes communicate with each other, exchanging values.

Each process then sets the value of a *decision variable*, *d<sub>i</sub>*. In doing so, it enters the *decided* state, in which it may no longer change *d<sub>i</sub>*.

### Requirements of a consensus algorithm
- *Termination*: Eventually, each correct process sets its decision variable.
- *Agreement*: The decision value of all correct processes is the same: if *p<sub>i</sub>* and *p<sub>j</sub>* are correct and have entered the *decided* state, then *d<sub>i</sub> = d<sub>j</sub> (i, j = 1, 2, ..., N)*.
- *Integrity*: If the correct processes all proposed the same value, then any correct process in the *decided* state has chosen that value.

### The Byzantine generals problem
In the informal statement of the *Byzantine generals problem*, three or more generals are to agree to attack or to retreat.

One, the commander, issues the order. The others, lieutenants to the commander, must decide whether to attack or retreat. But one or more of the generals may be *'treacherous'* (faulty).

- If the commander is treacherous, he proposes attacking to one general and retreating to another.
- If a lieutenant is treacherous, he tells one of his peers that the commander told him to attack and another that they are to retreat.

The Byzantine generals problem differs from consensus in that a distinguished process supplies a value that the others are to agree upon, instead of each of them proposing a value.

The requirements are:
- *Termination*: Eventually, each correct process sets its decision variable.
- *Agreement*: The decision value of all correct processes is the same: if *p<sub>i</sub>* and *p<sub>j</sub>* are correct and have entered the *decided* state, then *d<sub>i</sub> = d<sub>j</sub> (i, j = 1, 2, ..., N)*.
- *Integrity*: If the commander is correct, then all correct processes decide on the value that the commander proposed.

**Integrity implies agreement when the commander is correct, but the commander need not be correct**.

### Interactive consistency
The interactive consistency problem is another variant of consensus, in which every process proposes a single value. The goal of the algorithm is for the correct processes to agree on a *vector* of values, one for each process. We call this the *decision vector*. For example, the goal could be for each of a set of processes to obtain the same information about their respective states.

The requirements for interactive consistency are:
- *Termination*: Eventually each correct process sets its decision variable.
- *Agreement*: The decision vector of all correct processes is the same.
- *Integrity*: If *p<sub>i</sub>* is correct, then all correct processes decide on *v<sub>i</sub>* as the <em>i</em>th component of their vector.

### The Byzantine generals problem in a synchronous system
Here we assume that processes can exhibit arbitrary failures. That is, a faulty process may send any message with any value at any time; and it may omit to send any message. Up to *f* of the *N* processes may be faulty. Correct processes can detect the absence of a message through a timeout, but they cannot conclude that the sender has crashed, since it may be silent for some time and then send messages again.

If three processes send unsigned messages to one another, there is no solution that guarantees to meet the conditions of the Byzantine generals problem if one process is allowed to fail.

The generalized version of this result is that no solution exists if *N <= 3f*.
