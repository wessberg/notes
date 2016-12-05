# Transactions & Concurrency control
> Coulouris 16, except 16.6.
> Coulouris 17, excluding 17.3.2.

## Simple synchronization (without transactions)

### Atomic operations
Operations that are free from interference from concurrent operations being performed in other threads.

#### The `synchronized` keyword in Java
If a method signature has the `synchronized` keyword, for instance:
```java
public synchronized void deposit (int amount) {};
```
...Only one thread at a time can access the object containing the method. The other thread(s) that invokes one of its synchronized methods will then be blocked until the lock is released.

**Simple synchronization involves singular atomic operations. That means, single methods that is synchronized, not sets of methods which combined makes up a complex action.**

## Transactions
Sometimes clients require a sequence of separate requests to a server to be atomic.

Here, either all of the operations must be completed successfully or they must have no effect at all in the presence of crashes/errors.

Also, they are free from interference by operations being performed on behalf of other concurrent clients.

A *Transaction* could be the following set of atomic operations:
```java
// Transaction T:
a.withdraw(100);
b.deposit(100);
c.withdraw(200);
b.deposit(200);
```

The principles are:
- All or nothing
	-	A Transaction either completes in which case the effects of all of its operations are recorded in the objects, or in case of a failure or deliberate abort, it has no effect at all.
- Isolation
	-	Each Transaction must be performed without interference from other transactions.

### How to ensure Isolation
One way is for the server to perform transactions serially - one at a time, in some arbitrary order.

This works, but shouldn't always be done. We should strive to maximize concurrency. **Instead, Transactions should be allowed to execute concurrently if this would have the same effect as serial execution.**

That is, if the transactions are *serially equivalent*.

### Implementing transactions.
Imagine the following *Coordinator* interface:
```java
/**
 * Starts a new transaction and delivers a GUID. Will be used
 * in the other transaction-related methods.
 */
public TransactionID openTransaction();
/**
 * Ends a transaction. Returns a boolean indicating whether
 * or not the changes has been committed or aborted.
 */
public boolean closeTransaction(TransactionID trans);
/**
 * Aborts the given transaction.
 */
public void abortTransaction(TransactionID trans);
```

Then imagine adding the following to the `deposit` method signature from previously:
```java
public void deposit (TransactionID trans, int amount) {};
```
This makes it possible to backtrace all transaction actions as well as control whether or not the deposit operation can execute immediately (for instance, if another Transaction is currently "locking" the object).

### ACID Properties
The 'ACID' properties of transactions are:

<strong>A</strong>tomicity: A transaction must be all or nothing.

<strong>C</strong>onsistency: A transaction takes the system from one consistent state to another consistent state.

<strong>I</strong>solation.

<strong>D</strong>urability.

## Concurrency control

### The lost update problem
**TL;DR: The Lost Update problem occurs when two transactions concurrently read the old value of a variable and then use it to calculate the new value**

If two concurrent Transactions read and write to the same object, one Transaction may read the value to, say, 200, and then set the value to that times 2. However, another Transaction may in the meantime also set the value (for instance, because of a deposit). It may even commit the change. However, when the time comes for the Transaction that set the value to 200 * 2 to commit its results, it doesn't know about the deposit that just happened and then "throw away" that update. For instance:
<table>
	<tr>
		<td><strong>Transaction T</strong></td>
		<td><strong>Transaction U</strong></td>
	</tr>
	<tr>
		<td>balance = obj.getBalance(); // $200</td>
		<td>balance = obj.getBalance(); // $200</td>
	</tr>
	<tr>
		<td>obj.setBalance(balance * 2) // $400</td>
		<td></td>
	</tr>
	<tr>
		<td><strong>COMMIT</strong></td>
		<td>obj.setBalance(balance * 1.1 // $220)</td>
	</tr>
	<tr>
		<td></td>
		<td><strong>COMMIT</strong></td>
	</tr>
</table>

The value would then be $220. However, Transaction U should really have made the `balance * 1.1` operation on the new amount, $400, but didn't since it called `getBalance()` before Transaction T had committed.

### Serial equivalence
If each of several Transactions is known to have the correct effect when done on its own, we can infer that if these Transactions are done one at a time *in some order*, the combined effect will also be correct.

**What having the same effect means is that the *read* operations return the same values and that the instance variables of the objects have the same values at the end.**

An interleaving of the same operations of transactions in which the combined effect is the same as if the transactions had been performed one at a time in some order is a **serially equivalent interleaving**.

Using serial equivalence prevents occurrence of lost updates and inconsistent retrievals.

**For two Transactions T and U that writes to objects *i* and *j* to be serially equivalent, one of the following must be true:**

1. *T* accesses *i* before *U* and *T* accesses *j* before *U*

OR

2. *U* accesses *i* before *T* and *U* accesses *j* before *T*.

### Conflicting operations
When a pair of operations *conflicts*, we mean that their combined effect depends on the order in which they are executed.

#### *Read* and *Write* conflict rules
<table>
	<tr>
		<td><strong>Operations of different transactions</strong></td>
		<td><strong>Conflict</strong></td>
		<td><strong>Reason</strong></td>
	</tr>
	<tr>
		<td><em>Read</em>&#09;<em>Read</em></td>
		<td>No</td>
		<td>The effect of a pair of <em>Read</em> operations does not depend on the order in which they are executed.</td>
	</tr>
	<tr>
		<td><em>Read</em>&#09;<em>Write</em></td>
		<td>Yes</td>
		<td>The effect of a <em>Read</em> and <em>Write</em> operation depends on the order of their execution.</td>
	</tr>
	<tr>
		<td><em>Write</em>&#09;<em>Write</em></td>
		<td>Yes</td>
		<td>The effect of a pair of <em>Write</em> operations depends on the order in which they are executed.</td>
	</tr>
</table>

### Declaring Transactions
You can declare a Transaction like this:
`T: x = read(i); write(i, 10); write(j, 20);`

It translates to the following table:
<table>
	<tr>
		<td><strong>Transaction T</strong></td>
	</tr>
	<tr>
		<td><code>x = read(i);</code></td>
	</tr>
	<tr>
		<td><code>write(i, 10);</code></td>
	</tr>
	<tr>
		<td><code>write(j, 20);</code></td>
	</tr>
</table>

### Recoverability from aborts
Servers must record all the effects of committed transactions and none of the effects of aborted transactions.

A Transaction should be abortable without affecting other concurrent Transactions.

Two things can/will cause problems for this rule:
- Dirty reads
- Premature writes

### Dirty reads
The **Isolation property** of Transactions requires that transactions do not see the uncommitted state of other transactions. Dirty reads happens when a Transaction reads from an object that is currently being written to in another Transaction. This breaks the Isolation property.

Imagine if Transaction *T* writes to *i* and Transaction *U* then reads from *i* and use that value for something. *T* then aborts the Transaction. However, *U* still use the value it read previously. If *U* ends up committing the changes, it cannot be undone.

### Recoverability of Transactions
If a Transaction *U* has committed after it has seen the effects of a transaction *T* that subsequently aborted, **the situation is *not* recoverable**.

For recoverability, we can then delay committing *U* until *T* has committed. If *T* aborts/fails, so must *U*.

### Cascading aborts
Like stated before, if *U* delays committing until after *T* commits. If *T* then aborts, so must *U*. If *any* other Transactions have seen the effects due to *U*, they too must be aborted. The aborting of these latter transactions may cause still further transactions to be aborted. That's called **cascading aborts**.

If you want to avoid using cascading aborts, transactions are only allowed to read objects that were written by committed transactions.

### Premature writes
*Write* operations must be delayed until earlier Transactions that updated the same objects have either committed or aborted to avoid premature writes.

### Strict Transactions.
Generally, it is required that transactions delay both their *Read* and *Write* operations so as to avoid both dirty reads and premature writes. If Transactions does this, they are **strict**.

## Nested Transactions
Nested transactions allows or Transactions to be composed by other Transactions.

The *Top-level* Transaction is the outermost one.
All other Transactions is then called *subtransactions*.

*Subtransactions* then becomes the *top-level* Transaction of their own *"tree"* of *subtransactions*.

<img src="assets/nested_transactions.png"/>

### Advantages
Subtransactions at one level may run concurrently with other subtransactions at the same level in the hierarchy. This means additional concurrency.

Subtransactions can commit or abort independently. With a flat transaction (an atomic one, you might say), one transaction failure would cause the whole transaction to be restarted.

The rules are:
- A transaction may commit or abort only after its child transactions have completed.
- When a subtransaction completes, it makes an independent decision either to commit provisionally or to abort. Its decision to abort is final.
- When a parent aborts, all of its subtransactions are aborted.
- When a subtransaction aborts, the parent *can* decide whether to abort or not.
- If the top-level transaction commits, then all of the subtransactions that have provisionally committed can commit too, **provided that none of their ancestors has aborted**.

## Locks
One way to archive a serializing mechanism is through the use of **exclusive locks**.

### Exclusive Locks
With a exclusive lock, the server attempts to lock any object that is about to be used by any operation of a client's transaction.

If a client requests access to an object that is locked, the request is suspended and the client must wait until the object is unlocked.

Locks are lazy in the sense that they aren't activated when the Transaction starts, but immediately before reading/writing from/to the object.

<img src="assets/transaction_locks.png" />

### Two-phase locking
**To ensure that all pairs of conflicting operations of two transactions should be executed in the same order, a transaction is NOT allowed any new locks after it has released a lock.**

There are two phases:

- The growing phase
	-	The first phase of each transaction during which new locks are acquired.
- The shrinking phase
	-	The second (and final) phase of each transaction where locks are released.

This is called *two-phase locking*.

### Strict Two-phase locking
Like stated, strict transactions delay both their *Read* and *Write* operations until other transactions have committed or aborted. In Strict two-phase locking, **any locks applied during the progress of a transaction are held until the transaction commits or aborts.**

### Many readers/single writer scheme
There are different kinds of locking schemes. In this one, instead of having only exclusive locks, we have *Read locks* and *Write locks*. This means more concurrency as a *Read* operation doesn't conflict with another *Read* operation on the same object. So why block it?

In this scheme, an attempt to place a *Read* lock on an object will always be successful. All Transactions reading the object share its read lock. For this reason, read locks are sometimes called **shared locks**.

**The request for a *Write* lock on an object is delayed by the presence of a *Read* lock belonging to another Transaction.**

**The request for a *Read* lock *OR* a *Write* lock on an object is delayed by the presence of a *Write* lock belonging to another Transaction.**

### Lock compatibility table
<table>
	<tr>
		<td><strong>Existing lock</strong></td>
		<td><strong>Read-lock request</strong></td>
		<td><strong>Write-lock request</strong></td>
	</tr>
	<tr>
		<td>none</td>
		<td>OK</td>
		<td>OK</td>
	</tr>
	<tr>
		<td>Read</td>
		<td>OK</td>
		<td>wait</td>
	</tr>
	<tr>
		<td>Write</td>
		<td>wait</td>
		<td>wait</td>
	</tr>
</table>

### Lock promotion
When a lock is converted to a stronger lock, it is "promoted" to a lock that is more exclusive.

A *Write* lock is more exclusive than a *Read* lock (and thus it is called an *Exclusive lock*.)

### Use of locks in Strict 2PL
Here's what should happen:

1. When an operation accesses an object within a transaction:
	1. If the object is not already locked, lock it and proceed.
	2. If the object has a conflicting lock set by another Transaction, wait until it is unlocked.
	3. If the object has a non-conflicting lock (a *Shared/Read* lock) set by another Transaction, The lock is shared and proceed with the operation.
	4. If the object has already been locked in the same Transaction, promote the lock if necessary and proceed (if there is a conflicting lock, wait until it is unlocked.)
2. When a transaction is committed or aborted, the server unlocks all objects it locked for the transaction.
