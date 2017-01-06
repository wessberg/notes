# Time
> Coulouris Chapter 14 - 14.5 (inclusive).

Has to do with topics related to the issue of time in distributed systems.

- Time is a quantity we often want to measure accurately.
- In order to know at what time of day a particular event occurred at a particular computer, it is necessary to synchronize its clock with an authoritative, external source of time.
- Time is also used to check the authenticity of a request sent to a server in some cases.
- Time is used to decide the order in which a series of events occurred.

### Difficulty of measuring time
Due to the existence of multiple frames of reference, measuring time is hard.

#### Einsteins Relativity theory
It was with Einstein and his Special Theory of Relativity based on the observation that the speed of light is constant for all observers, regardless of their relative velocity that he proved, among other things, that two events that are judged to be simultaneous in one frame of reference are not necessarily simultaneous according to observers in other frames of reference that are moving relative to it.

For example, an observer on the Earth and an observer traveling away from Earth in a spaceship will disagree on the time interval between events, the more so as their relative speed increases.

The relative order of two events can even be reversed for two different observers.

**But this cannot happen if one event causes the other to occur**. In that case, the physical effect follows the physical cause for all observers, although the time elapsed between cause and effect can vary.

**There *is* no special physical clock in the universe to which we can appeal when we want to measure intervals of time.**

#### Problems in distributed systems
We are limited in our ability to timestamp events at different nodes sufficiently accurately to know the order in which *any* pair of events occurred, or whether they occurred simultaneously. There is no absolute, global time to which we can appeal.

And yet sometimes we need to observe distributed systems and establish whether certain states of affairs occurred at the same time. For instance, we might need to establish whether references to a particular object no longer exist - whether the object has become garbage.

## Clocks, events and process states
Lets for once assume that we have a collection of processes that do not share memory, each of which operates on a single processor and cannot communicate with each other in any other way than by sending messages through the network.

### Events
We define an event to be the occurrence of a single action that a process carries out as it executes. That could be a communication action or a state-transforming action.

#### Sequence of events (relation ➝<sub>i</sub>)
The sequence of events **within a single process** *p<sub>i</sub>* can be placed in a single, total ordering, which we denote by the relation *➝<sub>i</sub>* between the events: *e ➝<sub>i</sub> e'* if and only if the event *e* occurs before *e'* at *p<sub>i</sub>*.

Now we can define the *history* of process *p<sub>i</sub>* to be the series of events that take place within it, ordered as described by the relation *➝</sub>i</sub>*:

*history(p<sub>i</sub>) = h<sub>i</sub> = <e<sup>0</sup><sub>i</sub>,e<sup>1</sup><sub>i</sub>,e<sup>0</sup><sub>2</sub>, ...>*

But that is just *ordering*. How do we timestamp them?

#### Clocks
Computers each contain their own physical clocks. These are electronic devices that count oscillations occurring in a crystal at a definite frequency. Typically, this count is divided and the result is stored in a counter register.

The operating system reads the node's hardware clock value *H<sub>i</sub>(t)*, scales it and adds an offset so as to produce a software clock *C<sub>i</sub>(t) = ⍺(H<sub>i</sub>(t) + β)* that approximately measures real, physical time *t* for process *p<sub>i</sub>*.

So, when the real time in an absolute frame of reference is *t*, *C<sub>i</sub>(t)* is the reading on the software clock.
That *could* be the 64-bit value of the number of nanoseconds that have elapsed at time *t* since a convenient reference time.

In general, the clock is not completely accurate, so *C<sub>i</sub>(t)* will differ from *t*. But, if *C<sub>i</sub>* behaves sufficiently well, we can use its value to timestamp any event at *p<sub>i</sub>*.

**If the *clock resolution*, the period between updates of the clock value, isn't fast enough, successive events might correspond to the same timestamp!**. The rate depends on such factors as the length of the processor instruction cycle.

### Clock skew and clock drift
Computer clocks, like any others, tend not to be in perfect agreement :-(.

#### Clock skew
The instantaneous difference between the readings of any two clocks is called their *skew*.

#### Clock drift
The crystal-based clocks used in computers are, like any other clocks, subject to *clock drift*. That means that they count time at different rates, and so diverge.

The underlying oscillators are subject to physical variations, with the consequence that their frequencies of oscillation differ.

Also, even the same clock's frequency will vary with temperature.

The difference in the oscillation period between two clocks might be extremely small, **but the difference accumulated over many oscillations lead to an observable difference in the counters registered by two clocks, no matter how accurately they were initialized to the same value**.

**A clock's *drift rate* is the change in the offset (difference in reading) between the clock and a nominal perfect reference clock per unit of time measured by the reference clock.**

This is usually about 10<sup>-6</sup> seconds/second, giving a difference of 1 second every 1,000,000 seconds (or 11,6 days).

#### International Atomic Time
There are things such as 'high-precision' quartz clocks which is about 10<sup>-7</sup> or 10<sup>-8</sup>.

However, the most accurate physical clocks use atomic oscillators, whose drift rate is about one part in 10<sup>13</sup>. The output of these atomic clocks is used as the standard for elapsed "real" time, known as *International Atomic Time*.

### Coordinated Universal Time (UTC)
Computer clocks can be synchronized to external sources of highly accurate time.

*Coordinated Universal Time* or UTC for short is an international standard for timekeeping. It is based on atomic time, but a so-called *leap second* is inserted, or in rare cases deleted, occasionally to keep it in step with astronomical time where the period of the Earth's rotation about its axis gradually getting longer.

UTC signals are synchronized and broadcast regularly from land-based radio stations and satellites covering many parts of the world.

Computers with GPS receivers attached can synchronize their clocks with these timing signals.

## Synchronizing physical clocks

#### External synchronization
If we wish to know at what time of day events occur at the processes in a distributed system, we must synchronize the processes' clocks, *C<sub>i</sub>* with an authoritative, external source of time such as UTC. This is called *external synchronization*.

#### Internal synchronization
If the clocks *C<sub>i</sub>* are synchronized with one another to a known degree of accuracy, then we can measure the interval between two events occurring at different computers by appealing to their local clocks, even though they are not necessarily synchronized to an external source of time. This is called *internal synchronization*.

#### Formal definitions
Over an interval of real time *I*:

- *External synchronization*: For a synchronization bound *D > 0*, and for a source *S* of UTC time, *|S(t) - C<sub>i</sub>(t)| < D* for *i = 1,2, ...N* and for all real times *t* in *I*.
	-	Another way of saying this is that the clocks *C<sub>i</sub>* are *accurate* to within the bound *D*.

- *Internal synchronization*: For a synchronization bound *D > 0*, *|C<sub>i</sub>(t) - C<sub>j</sub>(t)| < D* for *i, j = 1, 2, ...N*, and for all real times *t* in *I*.
	-	Another way of saying this is that the clocks *C<sub>i</sub> agree* within the bound *D*.

Obviously, clocks that are internally synchronized are not necessarily externally synchronized since they may collectively drift from an external source of time even though they agree with one another. Remember, what we we care about is the *ordering* of events.

*However*, if the system is externally synchronized with a bound *D*, then the same is internally synchronized with a bound of *2D*.

### Correctness vs faultiness of clocks
#### Correctness
It is common to define a hardware clock *H* to be correct if its drift rate falls within a known bound *ρ > 0*.

**Note that clocks do not have to be accurate to be correct.** The goal usually is internal rather than external synchronization, so the criteria for correctness are only concerned with the proper functioning of the clock's mechanism, not its absolute setting.

#### Faultiness
A clock that does not keep to whatever correctness conditions apply is defined to be *faulty*.

#### Monotonicity
The condition that a clock *C* only ever advances:
*t' > t ⇒ C(t') > C(t)*.

#### Crash failures
A clock that stops ticking altogether is said to experience a *crash failure*.

#### Arbitrary failures
All other failures than crash failures.

## Synchronization in a synchronous system
In a synchronous system, bounds are known for the drift rate of clocks, the maximum message transmission delay as well as the time required to execute each step of a process.

To synchronizes clocks, here's what can be done in the simplest case:
- One process sends the time *t* on its local clock to the other in a message *m*.
- In principle, the receiving process could set its clock to the time *t + T<sub>trans</sub>*, where *T<sub>trans</sub>* is the time taken to transmit *m* between them.
- The two clocks now agree. None of them cares about whether or not the sending process's clock is accurate.

#### Minimum transmission time (min)
In general, other processes are competing for resources with the processes to be synchronized at their respective nodes, and other messages compete with *m* for network resources. This will obviously affect *T<sub>trans</sub>* which of the same reason will be subject to variation.

**But, there is always a minimum transmission time, *min*, that would be obtained if no other processes executed and no other network traffic existed. *min* can be measured or conservatively estimated.**

#### Maximum transmission time (max)
In a synchronous system, by definition, there is also a maximum transmission time, *max*.

If we define the uncertainty in the message transmission time as *u*, we define it as:

*u = (max - min)*

If the receiver sets it clock to be *t + min*, then the clock skew may be as much as *u*, since the message may in fact have taken time *max* to arrive.

Similarly, if it sets its clock to *t + max*, the skew may again be as large as *u*.

In general for a synchronous system, the optimum bound that can be achieved on clock skew when synchronizing *N* clocks is *u(1 - 1 / N)*.

**Thing is, most distributed systems found in practice are asynchronous where have no clue about the maximum time it takes for a message to arrive. It may take forever. Christian's method for synchronizing clocks is our savior**.

## Christian's method for synchronizing clocks
Uses a time server that is connected to a device that receives signals from a source of UTC, to synchronize computers **externally**.

Upon request, the server process *S* supplies the time according to its clock.

<img src="assets/clock_synchronization_time_server.png" />

The main observation (and also the requirement) of Christian's method is, that while there is no upper bound on message transmission delays in an asynchronous system, the round-trip times for messages exchanged between pairs of processes are often reasonably short - a small fraction of a second.

In order to achieve synchronization using Christian's method, the round-trip times between client and server must be sufficiently short compared with the required accuracy.

Here's how it works:

1. A process *p* requests the time in a message *m<sub>r</sub>* and receives the time value *t* from the time server *S* in a message *m<sub></sub>*.
	-	*t* is inserted in *m<sub>t</sub>* as the last possible point before transmission from *S* to maximize accuracy.

2. Process *p* records the total round-trip time *T<sub>round</sub>* taken to send the request *m<sub>r</sub>* and receive the reply *m<sub>t</sub>*.
	- **It can measure this time with reasonable accuracy if its rate of clock drift is small!**

3. *p* can now set its clock to *t + T<sub>round</sub> / 2* which assumes that the elapsed time is split equally before and after *S* placed *t* in *m<sub>t</sub>* and is a simple, but reasonably accurate estimate (unless the two messages are transmitted over different networks).

#### Determining accuracy
If the value of the minimum transmission time, *min*, is known or can be conservatively estimated, then we can determine the **accuracy** of this result.

Alright, so the earliest point at which *S* could have placed the time in *m<sub>t</sub>* was *min* after *p* dispatched *m<sub>r</sub>*, right? It took at the very least *min* time for the request from *p* to get to *S*. And, the *latest* point at which it could have done this was *min* time before *m<sub>t</sub>* arrived at *p*.

So, the time by <em>S</em>'s clock when the reply message **arrives at p** is therefore in the range: *[t + min, t + T<sub>round</sub> - min]*, right? It can at the very least be the time value it sent, *t* including the minimum transmission time, and it can at the very most be the time value it sent, *t*, including the observed total round-trip time minus the minimum transmission time.

**This gives is us an accuracy of *±(T<sub>round</sub> / 2 - min)*.**

#### TL;DR of accuracy
If you have *min*, you can compute the accuracy of the result with: *±(T<sub>round</sub> / 2 - min)*.

**The greater accuracy required, the smaller the probability of achieving it, because the most accurate results are those in which both messages are transmitted in a time close to *min* - an unlikely event in a busy network**.

### Problems with Christian's algorithm
Like all other centralized distributed systems, Christian's algorithm suffers the problems of the dependency on a single server which might fail or produce arbitrary failures. For crash failures, that would render synchronization temporarily impossible. But for arbitrary failures, like incorrect time values, that could really wreak havoc in a system.

You could add more time servers into the mix to reduce the risk of this problem, but from a theoretical standpoint, there is still the risk of failure.

The problem of dealing with faulty clocks is partially addressed by the Berkeley algorithm.

## The Berkeley algorithm
Here, a *coordinator* computer is chosen to act as the *master*. All other computers are called *slaves*.

This one periodically polls the other computers whose clocks are to be synchronized. The slaves send back their clock values to it.

The master then estimates their local clock times by observing the round-trip times, and it then averages the values obtained - including its own clock's reading.

**The balance of probabilities is that this average cancels out the individual clock's tendencies to run fast or slow**.

The accuracy of the protocol depends upon a nominal maximum round-trip time between the master and the slaves.

**The master eliminates any occasional readings associated with larger times than this maximum.**

Instead of sending the updated current time back to the other computers, which would introduce further uncertainty due to the message transmission time, the master instead sends the amount by which each individual slave's clock requires adjustment. This can be a positive or negative value.

#### As an algorithm
So, periodically, in a pool of processes where *P* is the master and *p<sub>i</sub>* is any of the slaves:

1. *P* requests all processes *p<sub>i</sub>* for their local clocks.

2. All processes *p<sub>i</sub>* sends back their local clock values.

3. Upon receiving the local clocks, *P* estimates the current local clocks of all processes *p<sub>i</sub>* by observing the round-trip times and calculating averages of the values obtained as well as its own clock's reading.
	-	If a reading is associated with a larger time than a nominal maximum round-trip time, it is eliminated. This is to ensure that faulty clocks cannot have a significant adverse effect on the computed average.

4. The master sends the amount by which each process *p<sub>i</sub>*'s clock requires adjustment. It can be a positive or a negative value.

#### Advantages
It eliminates readings from faulty clocks. Such clocks could have a significant adverse effect if an ordinary average was taken, so instead the master takes a *fault-tolerant average*, - a subset is chosen of clocks that do not differ from one another by more than a specified amount, and the average is taken of readings from only these clocks.

Should the master fail, then another can be *elected* to take over and function exactly as its predecessor.

## The Network Time Protocol (NTP)

Christian's method and the Berkeley algorithm are intended primarily for use within intranets.

The *Network Time Protocol (NTP)* defines an architecture for a time service and a protocol to distribute time information over the internet.

It aims to:

- *Provide a service enabling clients across the internet to be synchronized accurately to UTC*: Even though large and variable message delays are encountered in internet communication, NTP employs statistical techniques for the filtering of timing data and it discriminates between the quality of timing data from different servers.

- *Provide a reliable service that can survive lengthy losses of connectivity*: There are redundant servers and redundant paths between servers. The servers can reconfigure so as to continue to provide the service if one of them becomes unreachable.

- *Enable clients to resynchronize sufficiently frequently to offset the rates of drift found in most computers*: The service is designed to scale to large numbers of clients and servers.

- *Provide protection against interference with the time service, whether malicious or accidental*: Authentication techniques are used to check that timing data originate from the claimed trusted sources. It also validates the return addresses of messages sent to it.

### Primary and secondary servers
NTP is provided by a network of servers located across the Internet:
- *Primary servers*: Connected directly to a time source such as a radio clock receiving UTC.
- *Secondary servers*: Synchronized, ultimately, with primary servers.

Servers are connected in a logical hierarchy called a *synchronization subnet* whose levels are called *strata*.

#### Primary servers
Primary servers occupy stratum 1: they are at the root.

#### Secondary servers
Secondary servers occupy stratum 2 and are directly synchronized with the primary servers.

#### Stratum 3 and up
And then we have stratum 3 servers which are synchronized with stratum 2 servers and so on. The lowest level (leaf) severs execute in user's workstations.

**The clocks belonging to servers with high stratum numbers are liable to be less accurate than those with low stratum numbers because errors are introduced at each level of synchronization**.

NTP takes into account the total message round-trip delays to the root in assessing the quality of timekeeping data held by a particular server.

The synchronization subnet can reconfigure as servers become unreachable or failures occur. So, if a stratum 1 server's UTC source fails, it can become a stratum 2 server. If a stratum 2 server's normal source of synchronization fails or becomes unreachable, then it may synchronize with another server.

<img src="assets/ntp_subnet.png" />

### Modes of synchronization in NTP servers
NTP servers can synchronize in one of 3 modes:
- Multicast
- Procedure-call
- Symmetric-mode

#### Multicast mode
Is intended for use on a high-speed LAN. One or more servers periodically multicasts the time to the servers running in other computers connected by the LAN which then set their clocks assuming a small delay.

This mode can achieve only relatively low accuracies, but ones that are nonetheless considered sufficient for many purposes.

#### Procedure-call mode
Similar to Christian's algorithm. Here, one server accepts requests from other computers, which it processes by replying with its own current clock reading.

This mode is suitable where higher accuracies are required than what can be achieved with multicast or where multicast is not hardware-supported.

#### Symmetric mode
Is intended for use by the servers that supply time information in LANs and by the higher levels (lower strata) of the synchronization subnet, **where the highest accuracies are to be achieved**.

A pair of servers operating in symmetric mode exchange messages bearing timing information. Timing data are retained as part of an association between the servers that is maintained in order to improve the accuracy of their synchronization over time.

#### Transportation
In all modes, messages are delivered unreliably using UDP.

In procedure-call mode and symmetric mode, processes exchange pairs of messages, each of which bears timestamps of recent message events:
1. The local times when the previous NTP message between the pair was sent and received.
2. The local time when the current message was transmitted.

That amounts to a total of four times, *T<sub>i - 3</sub>, T<sub>i - 2</sub>, T<sub>i - 1</sub>, T<sub>i</sub>*.

<img src="assets/ntp_synchronization_messages.png" />

#### Offset
For each pair of messages sent between two servers, the NTP calculates an *offset o<sub>i</sub>*, which is an estimate of the actual offset between the two clocks.

#### Delay
For each pair of messages sent between two servers, the NTP calculates a *delay d<sub>i</sub>*, which is the total transmission time for the two messages.

Given servers *A* and *B*, if the true offset of the clock at *B* relative to that at *A* is *o*, and if the actual transmission times for *m* and *m'* are *t* and *t'* respectively, then we have:

*T<sub>i - 2</sub> = T<sub>i - 3</sub> + t + o and T<sub>i</sub> = T<sub>i - 1</sub> + t' - 0*.

Which leads to the following computation for the delay *d<sub>i</sub>*:

*d<sub>i</sub> = t + t' = T<sub>i - 2</sub> - T<sub>i - 3</sub> + T<sub>i</sub> - T<sub>i - 1</sub>*

And to compute the offset *o*:

*o = o<sub>i</sub> + (t' - t) / 2* where *o<sub>i</sub> = (T<sub>i - 2</sub> - T<sub>i - 3</sub> + T<sub>i - 1</sub> - T<sub>i</sub>) / 2*

So, *o<sub>i</sub>* is an **estimate** of the offset, and *d<sub>i</sub>* is a measure of the accuracy **of this estimate**.

## Logical time and logical clocks
Obviously, from the point of view of any single process, events are ordered uniquely by times shown on the local clock.

However, since we cannot synchronize clocks perfectly across a distributed system, we cannot in general use physical time to find out the order of any arbitrary pair of events occurring within it.

### Happened-before relation
We can, however, use a scheme to order *some* of the events that occur at different processes based on two points:

- If two events occurred at the same process *p<sub>i</sub> (i = 1, 2, ..., N)*, then they occurred in the order in which *p<sub>i</sub>* observes them (which is the *➝<sub>i</sub>*) order defined earlier.

- Whenever a message is sent between processes, the event of sending the message occurred before the event of receiving the message.

This partial ordering obtained by generalizing these two relationships are called the *happened-before* relation (also called a *causal ordering* or *potential causal ordering*).

#### Informal definition
Is denoted: *➝*

Defined as follows:
- HB1: *if Ǝ process p<sub>i</sub>: e ➝<sub>i</sub> e', then e ➝ e'*.

- HB2: For any message *m*, *send(m) ➝ receive(m)*
	-	Where *send(m)* is the event of sending the message and *receive(m)* is the event of receiving it.

- HB3: If *e, e'* and *e"* are events such that *e ➝ e'* and *e' ➝ e"*, then *e ➝ e"*.

So, if *e* and *e'* are events, and if *e ➝ e'*, then we can find a series of events *e<sub>1</sub>, e<sub>1</sub>, ..., e<sub>n</sub>* occurring at one or more processes such that *e = e<sub>1</sub>* and *e' = e<sub>n</sub>*, and for *i = 1, 2, ..., N - 1* either HB1 or HB2 applies between *e<sub>i</sub>* and *e<sub>i + 1</sub>*. **That is, either they occur in succession at the same process, or there is a message *m* such that *e<sub>i</sub> = send(m)* and *e<sub>i + 1</sub> = receive(m)*.**

<img src="assets/events_three_processes.png" />

From the image it can be seen that *a ➝ b* since the events occur in this order at process *p<sub>1</sub>* (*a ➝<sub>i</sub> b*).

We can also see that  *c ➝ d* of the same reason.

What is more interesting is that we can see that *b ➝ c* since these events are the sending and reception of message *m<sub>1</sub>*.

We can do the same thing with *d ➝ f*.

**Combining these relations, we may also say that, for example, *a ➝ f*.**

#### Events that are unrelated
As seen from the image, we can see that *a ↛ e* and *e ↛ a* (*a* doesn't necessarily come before *e* and *e* doesn't necessarily come before *a*) since they occur at different processes and there is no chain of messages intervening between them.

**We say that events such as *a* and *e* that are not ordered by ➝ are *concurrent* and write it as *a || e*.**

### Logical clocks
A simple mechanism for capturing the happened-before relation numerically is called a *logical clock*.

A logical clock is a monotonically increasing software counter whose value need bear no particular relationship to any physical clock.

Each process *p<sub>i</sub>* keeps its own logical clock *L<sub>i</sub>* which it uses to apply so-called *Lamport timestamps* to events.

We denote the timestamp of event *e* at *p<sub>i</sub>* by *L<sub>i</sub>(e)* and we denote the timestamp of event *e* at whatever process it occurred at as *L(e)*.

Processes update their local clocks and transmit the values of their logical clocks in a message as follows:

- LC1: *L<sub>i</sub>* is incremented before each event is issued at process *p<sub>i</sub>*: *L<sub>i</sub> := L<sub>i</sub> + 1*.

- LC2:
	- (a) When a process *p<sub>i</sub>* sends a message *m*, it piggybacks on *m* the value *t = L<sub>i</sub>*.
	- (b) On receiving *(m, t)*, a process *p<sub>j</sub>* computes *L<sub>j</sub> := max(L<sub>j</sub>, t)* and then applies LC1 before timestamping the event *receive(m)*.

So here is the image from before, except with Lamport timestamps:

<img src="assets/lamport_timestamps.png" />

### Totally ordered logical clocks
Some pairs of distinct events, generated by different processes, have numerically identical Lamport timestamps. For example, in the image above, *a* and *e* both have the timestamp 1.

However, we can create a **total order** on a set of events which means one for which all pairs of distinct events are ordered by taking into account the identifiers of the processes at which events occur.

If *e* is an event occurring at *p<sub>i</sub>* with local timestamp *T<sub>i</sub>* and *e'* is an event occurring at *p<sub>j</sub>* with local timestamp *T<sub>j</sub>*, we define the global logical timestamps for these events to be *(T<sub>i</sub>, i)* and *(T<sub>j</sub>, j)* respectively.

And we define *(T<sub>i</sub>, i) < (T<sub>j</sub>, j)* if and only if either *T<sub>i</sub> < T<sub>j</sub>* or *T<sub>i = T<sub>j</sub>* and *i < j*.

This has no general physical significance, but it *is* sometimes useful, for instance, when ordering the entry of processes to a critical section.

### Vector clocks
With Lamport clocks, even though *L(e) > L(e')*, we cannot conclude that *e ➝ e'*. A *vector clock* is designed to overcome this shortcoming.

A vector clock for a system of *N* processes is an array of *N* integers.

Each process keeps its own vector clock, *V<sub>i</sub>*, which it uses to timestamp local events. Just like Lamport timestamps, processes piggyback vector timestamps on the messages they send to one another, and there are simple rules for updating the clocks:

- VC1: Initially, *V<sub>i</sub>[j] = 0* for *i, j = 1, 2 ..., N*

- VC2: Just before *p<sub>i</sub>* timestamps an event, it sets *V<sub>i</sub>[i] := V<sub>i</sub>[i] + 1*.

- VC3: *p<sub>i</sub>* includes the value *t = V<sub>i</sub>* in every message it sends.

- VC4: When *p<sub>i</sub>* receives a timestamp *t* in a message, it sets *V<sub>i</sub>[j] := max(V<sub>i</sub>[j], t[j])*, for *j = 1, 2 ..., N*.
	-	Taking the component-wise maximum of two vector timestamps in this way is known as a *merge* operation.

For a vector clock *V<sub>i</sub>*, *V<sub>i</sub>[i]* is the number of events that *p<sub>i</sub>* has timestamped and *V<sub>i</sub>[j] (j ≠ i)* is the number of events that have occurred at *p<sub>j</sub>* that have potentially affected *p<sub>i</sub>*.

Here the image is again, except with vector timestamps instead:
<img src="assets/vector_timestamps.png" />

#### Disadvantages
Compared with Lamport timestamps, Vector timestamps have the disadvantage that they take up an amount of storage and message payload that is proportional to *N*, the number of processes.

## Global states

### Distributed garbage collection
An object is considered to be garbage if there are no longer any references to it anywhere in the distributed system.

The memory taken up by that object can then be reclaimed once it is known to be garbage. So, to check that it is garbage, we must verify that there are no references to it anywhere in the system.

Imagine a process *p<sub>1</sub>* that has two objects that both have references:
- One has a reference within *p<sub>1</sub>* itself.
- *p<sub>2</sub>* has the other reference.

Imagine also that *p<sub>2</sub>* has an object that are referenced neither by *p<sub>1</sub>* or *p<sub>1</sub>* **but**, a reference to it is on its way in a message that is in transit between the processes.

**We must include the state of communication channels as well as the state of the processes when designing distributed garbage collection!**

### Distributed deadlock detection
Happens when each of a collection of processes waits for another process to send it a message, and where there is a cycle in the graph of this *waits-for* relationship. Imagine if *p<sub>1</sub>* waits for *p<sub>2</sub>*, but *p<sub>2</sub>* also waits for *p<sub>1</sub>*. That's a cycle - the system will never make process and is in a deadlock.

<img src="assets/waits_for_graph_cycle.png" />

### Distributed termination detection
How do we detect that a distributed algorithm has terminated?

### Distributed debugging
Pretty complex stuff to do.

### What do these problems have in common?
They all illustrate the need to observe a global state, and so motivate a general approach.

### Global states and consistent cuts
Its (super) hard to ascertain a global state of a system - the state of the collection of processes.

**The essential problem is the absence of global time**. If all processes had perfectly synchronized clocks, then we could agree on a time at which each process would record its state. That would be actual global state of the system.

We could use that one to figure out whether or not the processes were deadlocked, for instance.

**But we cannot achieve perfect clock synchronization!**

But can we instead assemble a meaningful global state from local states recorded at different times? Yeah!

Remember,
**Each event either is an internal action of the process (for instance, updating one of its variables) or is the sending or receipt of a message over the communication channels that connect the processes**.

#### Global history
The *global* history of a distributed system is the union of the individual process histories.

Remember, the history of a single process *p<sub>i</sub>* is:
*history(p<sub>i</sub>) = h<sub>i</sub> = <e<sup>0</sup><sub>i</sub>,e<sup>1</sup><sub>i</sub>,e<sup>0</sup><sub>2</sub>, ...>*.

So the history of the entire system is:

*H = h<sub>0</sub> 𖼓 h<sub>1</sub> 𖼓 ... 𖼓 h<sub>N - 1</sub>*

### Global state
Notation: *S*.
For multiple global states we may characterize the execution of a distributed system as a series of transitions between global states of the system:

*S<sub>0</sub> ➝ S<sub>1</sub> ➝ S<sub>2</sub> ➝ ...*

### Cuts
<img src="assets/cuts.png" />
A *cut* of the system's execution is a subset of its global history that is a union of prefixes of process histories:

*C = h<sup>c<sub>1</sub></sup><sub>1</sub> 𖼓 h<sup>c<sub>2</sub></sup><sub>2</sub> 𖼓 ... 𖼓  h<sup>c<sub>N</sub></sup><sub>N</sub>*

where *c* are the cuts of the individual process's histories.

#### Inconsistent cuts
If a cut includes the receiving event of a message but does not include the sending event, we have an *inconsistent cut*, just like seen in the image where we take *e<sup>0</sup><sub>2</sub>* but doesn't take *e<sup>1</sup><sub>1</sub>*. That doesn't work!

**It shows an effect without a cause, and that's not good**.

#### Consistent cut
Here, we *can* include the sending event in our cut but not the receiving event. That is still a *consistent cut*.
A cut *C* is consistent if, for each event it contains, it also contains all the events that *happened-before* that event:

*For all events e ∈ C, f ➝ e ⇒ f ∈ C*

#### Consistent global state
A consistent global state is one that corresponds to a consistent cut.

### Run
A *run* is a total ordering of all the events in a global history that is consistent with each local history's ordering.

#### Linearization or consistent run
Is an ordering of the events in a global history that is consistent with this happened-before relation *➝* on *H*. A linearization is the same thing.

## Global state predicates, stability, safety and liveness

### Uses of global states
Detecting a condition such as deadlock or termination requires evaluating a global state predicate.

### Global state predicates
A function that maps from the set of global states of processes in the system to *{True, False}*.

One of the useful characteristics of the predicates associated with the state of an object being garbage, of the system being deadlocked or the system being terminated is that they are all *stable*: Once the system enters a state in which the predicate is *True*, it remains *True* in all future states reachable from that state.

### Safety & liveness
*Safety* with respect to a property *a*, such as being deadlocked, is the assertion that *a* evaluates to *False* for all states *S* reachable from *S<sub>0</sub>*.

### Liveness
Liveness with respect to a property *b* such as reaching termination, is the property that, for any linearization *L* starting in the state *S<sub>0</sub>*, *b* evaluates to *True* for some state *S<sub>L</sub>* reachable from *S<sub>0</sub>*.

## The snapshot algorithm
The 'snapshot' algorithm determines global states of distributed systems.

The goal is to record a set of process and channel states (a 'snapshot') for a set of process *p<sub>i</sub>* (i = 1, 2, ..., N) such that, even though the combination of recorded states may never have occurred at the same time, the recorded global state is consistent.

It records state locally at processes. It does not give a method for gathering the global state at one site.

The algorithm assumes that:

- Neither channels nor processes fail. Communication is reliable so that every message sent is eventually received, intact, exactly once.

- Channels are unidirectional and provide FIFO-ordered message delivery.

- The graph of processes and channels is strongly connected: There is a path between any two processes.

- Any process may initiate a global snapshot at any time.

- The processes may continue their execution and send and receive normal messages while the snapshot takes place.

### The algorithm

For each process *p<sub>i</sub>*, let the *incoming channels* be those at *p<sub>i</sub>* over which other processes sends it messages. Let the *outgoing channels* of *p<sub>i</sub>* be those on which it sends messages to other processes.

Each process records its state and also, for each incoming channel, a set of messages sent to it.

The process records, for each channel, any messages that arrived after it recorded its state and before the sender recorded its own state.

This allows us to record the states of processes at different times but to account for the differentials between process states in terms of messages transmitted but not yet received.

### Marker messages
The algorithm proceeds through the use of *marker* messages which processes may send and receive while they proceed with their normal execution.

The marker has a dual role:

- It is a prompt for the receiver to save its own state, if it has not already done so.

- It is a means of determining which messages to include in the channel state.

#### Marker rules

- *Marker receiving rule*: Obligates a process that has not recorded its state to do so. In that case, this is the first marker that it has received. It notes which messages subsequently arrive on the other incoming channels. When a process that has already saved its state receives a marker on another channel, it records the state of that channel as the set of messages it has received on it since it saved its state.

- *Marker sending rule*: Obligates processes to send a marker after they have recorded their state, but before they send any other messages.

### Informal algorithm
*Marker receiving rule for process p<sub>i</sub>
	On receipt of a marker message at p<sub>i</sub> over channel c:
		if (p<sub>i</sub> has not yet recorded its state) it
			records its process state now;
			records the state of c as the empty set;
			turns on recording of messages arriving over other incoming channels;

		else
			p<sub>i</sub> records the state of c as the set of messages it has received over c since it saved its state;
		end if

Marker sending rule for process p<sub>i</sub>
	After p<sub>i</sub> has recorded its state, for each outgoing channel c:
		p<sub>i</sub> sends one marker message over c (before it sends any other message over c);*

#### Starting the algorithm
Any process may begin the algorithm at any time. it acts as though it has received a marker over a nonexistent channel and follows the marker receiving rule.

Several processes may initiate recording concurrently in this way.

#### Characterizing the observed state
The snapshot algorithm achieves consistent cut.

#### Pre-snap and post-snap events
A *pre-snap* event at at a process *p<sub>i</sub>* is one that occurred at *p<sub>i</sub>* before it recorded its state.

All other kinds of events are *post-snap* events.

Obviously, a *post-snap* event can occur before a *pre-snap* event if the events occur at different processes.
