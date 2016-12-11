# Time
> Coulouris Chapter 14 - 14.5. Also read about UTC, Clock correctness, NTP, the Berkeley variant of *Christian's method*, Totally ordered logical clocks, vector clocks.

### Events

- An *event* `e` happens at a process and has an action.

- Write `e -->i e'` if `e`, `e'` happens at `i`, `e` before `e'`

- Events at process `i` form a total order.

### Synchronization

How do we synchronize clocks?

- External, bound `D>0`: Given a UTC source `S`, for any `1 <= i <= N`:<br>
*| S(t)-C<sub>i</sub>(t) | < D* <br><br> Accurate within bound `D`.

- Internal bound `D>0`: For any `1<=i, j<= N`:<br>*| C<sub>i</sub>(t) - C<sub>j</sub>(t) | < D* <br><br>
In agreement within bound D

### Synchronizing in a *synchronous* distributed system.

About bounds on clock drift, execution steps, message delivery.
Again, remember that we define explicit minimum and maximum bounds on execution time in synchronous distributed systems.

Say we want to synchronize two processes P<sub>1</sub>, P<sub>2</sub>

- *P<sub>1</sub>* sends local time t in message to *P<sub>2</sub>*
- *P<sub>2</sub>* sets time to `u = t + (max - min) / 2 + min`
- Skew bounded by `(max - min) / 2`

### Synchronizing in a *asynchronous* distributed system

Here, we have no lower or upper bound on *max* and *min*. How do we synchronize time then?

We used whats' called *Christian's method*.
The point is that we calculate the latency from the initial transmission and add it to the overall clock drift.

- Estimate round-trip time: *T<sub>round</sub>*.

- *u = t + T<sub>round</sub> / 2*

- Accuracy estimate with known/estimated minimum transmission time (e.g. latency).

- *±(T<sub>round</sub> / 2 - min)*

## Logical time

Logical time is about the order in which things happened.

They key tool is the **Happened-Before** relation.

### Happened-Before

Three properties:

- If *e --><sub>i</sub>*, then *e --> e'*.

- `send(m) --> receive(m)`

- The transitive closure: e.g. `if e -->e' and e' --> e" then e --> e"`.

Events that cannot be related are called **concurrent**. Processes on different threads produce events. These are concurrent in the sense that they have no relation to each other.

### Logical clocks (Lamport)
Here we add a logical clock L<sub>i</sub> to each process.

- *L<sub>i</sub> := L<sub>i</sub> + 1 just* before each event at *p<sub>i</sub>*

- `send(m)` at P<sub>i</sub> piggy-backs *t = L<sub>i</sub>* in *m*.

- *L<sub>i</sub> := max(L<sub>i</sub>, t) just* after `receive(m)` at *P<sub>i</sub>* before doing step 1

## Global state

The history of a process is the set of events that happened in that process.

e.g. *h<sub>i</sub> = e<sub>0</sub>, e<sub>1</sub>...*

The **global history** is all histories of individual processes.

A **prefix** of a history is a subset of the set of events.

A **state** is an object state from the point of a specific event on a specific process.

A **global state** is thus the state of every process on a specific point in time.

<u>Properties of a global state are:</u>

- Consistent. Defined by a consistent cut.

- Some ordering of all events that respects local causality *--><sub>i</sub>*

- Consistent run (Also called *linearization*): Run that respects *Happened-Before*.

### Cut

A choice of *prefix* for each individual process.

### Consistent/inconsistent cut

A consistent cut is one that has a corresponding `send` event for each `receive` event.

An inconsistent cut is not a reasonable global state.

### Snapshot (Chandy & Lamport)

- The graph of processes and channels is strongly connected; There is a path between any two processes.

- Any process may initiate a global snapshot at any time.

- The processes may continue their execution and send and receive normal messages while the snapshot takes place.

- And more ... (already covered by the above model).

Here's the *receiving* rule for a process P<sub>i</sub>:

```
if (Pi has not yet recorded its state) it
	records its process state now;
	records the state of c as the empty set;
	turns on recording of messages arriving over other incoming channels.
else
	Pi records the state of c as the set of messages it has received over c since it saved its state.
end if
```

Here's the *sending* rule for a process P<sub>i</sub>:

```
Pi records its state.
For each outgoing channel C on which a marker has not been sent, i sends a marker along.
```

TODO: Read the chapter in the book!
