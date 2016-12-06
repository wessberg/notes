# Distributed Transactions
> Coulouris 17, excluding 17.3.2.

A distributed is a flat or nested transaction that accesses objects managed by multiple servers.

When a distributed transaction comes to an end, either all of the servers involved commit the transaction *or* all of them abort the transaction.

To do so, one of the servers take on a *coordinator* role, which involves ensuring the same outcome at all of the servers.

## Flat and nested distributed transactions
A Transaction becomes distributed if it invokes operations in several different servers.
