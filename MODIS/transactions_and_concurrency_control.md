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
