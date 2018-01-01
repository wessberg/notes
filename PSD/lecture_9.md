# Lecture 9

> Programming Language Concepts, Chapter 10

## Predictable Lifetime and Stack Allocation

### Lifetime of values on the stack

With the stack it holds that if value `v2` is pushed on the stack after value `v1`, then `v2` is popped off the stack before `v1`.

In other words, it is a LIFO (Last In, First Out).

This also means that stack allocation is really efficient. All we need to do is to increment the stack pointer to leave space for more data. And, deallocation is just as efficient. We just decrement the stack pointer so the next allocation overwrites the old data.

**Thus, a value can *live* no longer than any value created before it**!

But, most modern programming languages do permit the creation of values whose lifetimes cannot be determined at their point of creation. For example, functions, which need closures. And, lists and trees. Basically, objects.

**Values with unpredictable lifetime are stored in another storage data structure, the so-called *heap***.

## Heap Allocation

The *heap* means appriximately "disorderly collection of data".

Data are explicitly allocated in the heap by the program, **but cannot be explicitly deallocated**. Instead, deallocation is done automatically by a so-called *garbage collector*.

In most languages, a heap with automatic garbage collection is used.

In some, like C and C++, the user must manually and explicitly manage data whose lifetime is unpredictable. Such data can be allocated outside the stack using `malloc` in C. But, such data must then be explicitly deallocated by the program using `free` in C.

### Manual deallocation

In practice, this is very difficult. Either data get deallocated too early, creating dangling pointers and causing a program crash, or too late, and the program uses more and more space while running and must be restarted every so often: it has a memory leak.

### Allocation in a Heap

In Java and C# (and many other languages), every new array or object - including strings - is allocated in the heap.

## Garbage Collection Techniques

The whole purpose of the garbage collector is to make room for new data in the heap by reclaiming space occupied by old data that are no longer used!

There are many different garbage collection algorithms.

### Collector vs Mutator

- The *collector* reclaims unused space.
- The *mutator* allocates new values and possibly updates old values.

The collector exists purely for the sake of the mutator, which does the real useful work.

**The mutator is typically the abstract machine that executes the bytecode**!

### Root set

All garbage collection algorithms have a notation of a *root set*. **This is typically the variables of all the active (e.g. not yet returned-from) function calls or method calls of the program**.

In other words, the root set consists of those references to the heap found in the action records (stack frames) on the stack and in machine registers (if any).

## The Heap and the Freelist

The heap typically contains *allocated blocks* of different sizes mixed with *unused blocks* of different sizes.

### Allocated blocks

Every allocated block contains a header with a size field and other information about the block, and possibly a description of the rest of the blocks contents.

### Unused Blocks and the Freelist

Some garbage collectors make sure that unused blocks are linked together in a so-called *freelist* (which is basically a LinkedList).

Here, each unused block has a header with a size field, and its first field contains a pointer to the next unused block on the freelist!

**A pointer to the first block on the freelist is kept in a special freelist register by the garbage collector**.

### Allocating new values

A new value (object, closure, string, array, anything that goes in the heap) can be allocated from a freelist by traversing the freelist until a large enough free block is found!

If no such block is found, then a garbage collection may be initiated. After collection, if there still is no block large enough, the heap must be extended by requesting more memory from the OS. Otherwise, the program fails because of insufficient memory!

### Drawbacks of allocation from a freelist

This is obviously that searching the freelist for a large enough free block may take a long time, if there are many too-small blocks on the list.

Also, the heap may become fragmented. For example, we may be unable to allocate a block of 36 bytes although there are thousands of unused but non-adjacent 32-byte blocks on the freelist.

To reduce fragmentation, one may trty to find the smallest block, instead of the first block, on the freelist that is large enough for the requested allocation, but if there are many small free blocks, that may be even slower.

### Improving on the Freelist approach

There are some things that can be done to improve the situation. We can keep distinct freelists for distinct sizes of free blocks. This speeds up allocation and reduces fragmentation but also introduces new complexity. How many distinct freelists should we maintain? When do we move blocks from one freelist to another?

## Garbage Collection by Reference Counting (with a Freelist)

One way to implement garbage collection is by associating a reference count with each object on the heap, which counts the number of references to the object from other objects and from the stack.

This involves:

- Each object is created with a reference count of 0
- When the mutator (abstract machine) performs an assignment `x = null`, it must decrement the reference count of the object previously referred to by `x`, if any.
- When the mutator performs an assignment `x = o` of an object reference to a variable or a field, it must:
	1. Increment the reference count of the object `o`
	2. Decrement the reference count of the object previously referred to by `x`, if any
- Whenever the reference count of an object `o` gets decremented to zero, the object may be deallocated (by putting it on the freelist), and the reference counts of every object that `o`'s fields refer to must be decremented too.

### Advantages of Reference Counting

Reference counting is fairly simple to implement (and understand). Once allocated, a value is never moved, which is important if a pointer to the value has been given to external code, such as a input-output routine.

### Disadvantages of Reference Counting

Additional memory is required to hold each object's reference count. The incrementing, decrementing and testing of reference counts slow down *all* assignments of object references.

And, when decrementing an object's reference count to zero, the same must be done recursively to all objects that it refers to. This can take a long time, causing a long pause in the execution of the program.

But, most importantly, **it cannot collect cyclic object structures!**.

And, if it uses a freelist, it suffers all the weaknesses of allocation with a freelist.

## Garbage Collection by Mark-Sweep Collection

With mark-sweep garbage collection, the heap contains allocated objects of different sizes, and unused blocks of different sizes.

The unallocated blocks are typically managed with a freelist.

Mark-sweep garbage collection is done in two phases:

1. The *mark phase*
2. The *sweep phase*

### The mark phase

Here, all blocks that are reachable from the root set are marked. This is done by first marking all those blocks pointed to from the root, and then recursively marking the unmarked blocks pointed to from marked blocks.

This works even when there are pointer cycles in the heap. After this phase all live blocks are marked!

### The sweep phase

Here, we go through all blocks in the heap, unmark the marked blocks and put the unmarked blocks on the freelist, joining adjacent free blocks into a single larger block.

### Advantages of mark-sweep collection

It is faily simple to implement. Once allocated, a value is never moved, which is important if a pointer to the value has been given to external code, such as a input-output routine.

### Disadvantages of mark-sweep collection

Whereas the mark phase will visit only the live objects, the *sweep phase* must look at the **entire** heap, including the potentially many dead objects that are to be collected. A complete cycle of marking and sweeping may take much time, causing a long pause in the execution of the program.

And, if it uses a freelist, it suffers all the weaknesses of allocation with a freelist.

## Garbage Collection by Two-Space Stop-and-Copy Collection

Here, the heap is divided into two equally large *semispaces*. At any time, one semispace is called the *from-space*, and the other is called the *to-space*.

After each garbage collection, the two semispaces swap roles. There is no freelist.

Instead an *allocation pointer* points into the from-space; all memory from the allocation pointer to the end of the from-space is unused.

Allocation is done in the from-space, at the point indicated by the allocation pointer. The allocation pointer is simply incremented by the size of the block to be allocated!

If there is not enough available space, a garbage collection must be made.

Garbage collection moves all live values from the from-space to the to-space which is initially empty. Then it sets the allocation pointer to point to the first available memory cell of the to-space. It ignores whatever is in the from-spce, and swaps from-space and to-space.

At the end of a garbage collection, the new from-space contains all **live** values and hos rooms for new allocations. The new *to-space* is empty and remains empty until the next garbage collection.

### Advantages by Two-Space Stop-and-Copy Collection

No stack is needed for garbage collection, only a few pointers. By copying the collection, we automatically perform *compaction* (moving live objects next to each other, leaving no unused holes between them). This also avoids fragmentation of the heap.

### Disadvantages by Two-Space Stop-and-Copy Collection

Copying can use at most half of the available memory space for live data. If the heap is nearly full, then every GC will copy almost all live data, but may reclaim only very little unused memory.

Thus, as the heap gets full, performance may degrade and get arbitrarily bad. Since the address of a value may be moved at any time, a pointer to a value cannot be passed to external code without extra safeguards.

## Garbage Collection by Generational Garbage Collection

This is the one used in V8!

It starts from the observation that most allocated values die young! Therefore it is wasteful to copy all the live, mostly old, values in every garbage collection cycle only to reclaim the space occupied by some young, now dead, values.

Instead, it divides the heap into several generations numbered between *1, ..., N*. Always allocate in generation 1. When generation 1 is full, do a *minor garbage collection*, promote (move) all live values from generation 1 to generation 2. Then generation 1 is empty and new objects can be allocated into it. When generation 2 is full, promote live values from generation 2 to generation 3, and so on.

Generation *N*, the last generation, may then be managed by a mark-sweep garbage collection algorithm.

A *major garbage collection* is a collection that frees all unused space in all generations. where a *minor garbage collection* is one that frees all unused space in a single generation.

When there are only two generations, as there are in V8, generation 1 is called the young generation, and generation 2 is called the old generation

### Advantages of Generational Garbage Collection

It reclaims short-lived values very efficiently. If desirable, it can avoid moving old data (which is important if pointers to heap-allocated data needs to be passed to external code)

### Disadvantages of Generational Garbage Collection

It is more complex to implement. It imposes a certain overhead on the mutator because of the so-called *write barrier* between old and young generations. Whenever a pointer to a young generation data object is stored in an old generation data object, that pointer must be recorded in a separate data structure so that the GC knows all pointers into the young generation.

## Garbage Collection by Conservative Garbage Collection

All of the other algorithms are called *precise* (in contrast to conservative).

A conservative garbage collector is one that assumes that if a bit pattern looks like a reference, then it *is* a reference, and the pointed-to object will survive the collection.

It may say that if the bit pattern looks like an address inside the allocated heap, and the memory it points at has the proper structure of a heap-allocated object, then it is probably a reference.

This also means that stuff like an integer, representing maybe a customer number, may look like a reference into the heap. If such an integer is mistakenly assumed to be a reference, then some arbitrary memory data may be assumed to be a live object and never be collected. That may cause a memory leak.

## Garbage Collectors used in Existing Systems

JVM previously used generational GC with three generations: the young, the old and the permanent. The young generation is subdivided into the *eden* and two *survivor spaces*.

Now, they use something called the *Garbage-First Garbage Collector*. It is also a generational garbage collector built with parallelism in mind to support multi-core CPUs.

.NET CLI also uses a generational garbage collector.

The Mono implementation of CLI actually uses a conservative garbage collector.

## Finalizers

A *finalizer* is a method (a disposer) that is associated with an object, that gets called when the GC discovers that the object is dead and collects it. Finalizers should generally be avoided since a long time may pass since an object is dead and the GC actually removes it and calls the finalizer.

## Calling the GC

You can often force-trigger GC by invoking something like `System.gc()` in JVM or `System.CG.Collect()` in .NET. Usually, though, the GC is better informed than the programmer and you should never use those methods.

## Cons cells

A *cons cell* is a pair of values. In terms of list-C, the example language used in this part of course (an extension of micro-C), a value is iehter a primitive value such as an integer or a reference to a cons cell, or `nil`, which denotes the absence of a reference.

With these cells, we can build lists, trees and other data structures.

### `car` and `cdr`

- `car(e)` evaluates to the first component of the cons cell referred to by `e`.
- `cdr(e)` evaluates to the second component of the cons cell referred to by `e`.

### `nil`

Nil evaluates to a null pointer

### `cons`

Cons creates a new cons cell with the given `car` and `cdr` values

## Memory Structures in the Garbage Collector

The heap of the list-C Abstract Machine is composed of blocks of one or more 32-bit words.

The first word is always a header that describes the block.

A cons cell is a block that consists of three 32-bit words, namely:

- The block header `ttttttttnnnnnnnnnnnnnnnnnnnnnngg` that contains:
  - 8 tag bits (`t`)
	- 22 length bits (`n`)
	- 2 garbage collection bits (`g`)

For a cons cell, the tag bits will always be *00000000* and the length bits will be **00...0010*, indicating that the cons cell has two words, not counting the header word.

The garbage collection bits `gg` will be interpreted as colors:

- *00* means white
- *01* means grey
- *10* means black
- *11* means blue

- A first field, called the `car` field, which can hold any abstract machine value
- A second field, called the `cdr` field, which can hold any abstract machine value.

It also maintains a `freelist`. A block on the freelist consists of at least two words:

- A block header `ttttttttnnnnnnnnnnnnnnnnnnnnnngg` just as for a cons cell. In a free cell, the tag bits do not matter, whereas the length bits indicate the number *N* of words in the free block in addition to the header word, and the garbage collection bits must be 11 (blue).

- A field that either is all zeroes, meaning that this is the last block on the freelist, or contains a pointer to the next block on the freelist.
- Further *N - 1* words that belong to this free block.

## Actions of the Garbage Collector

When the mutator asks the GC for a new object (a block), the GC inspects the freelist register. If it is non-null, the freelist is traversed to find the first free block that is large enough for the new object.

If that free block is of exactly the right size, the freelist register is updated to point to the free block's successor. Otherwise, the allocated object is cut out of the free block.

In case the free block is one word larger than the new object, this may produce an orphan word that is neither in use as an object nor on the freelist. **An orphan must be blue to prevent the sweep phase from putting it on the freelist**.

GC is performed in two phases, mark and sweep.

### (1) The mark phase

The mark phase traverses the mutator's stack to find all references into the heap. A value in the stack is either:

1. A tagged integer, sucha s a return address, an old base pointer value or an array address in the stack. Or
2. A heap reference

In case it is a heap reference, when we encouter a heap reference to a white block, we mark it black and recursively process all the block's words in the same way.

After the mark phase, every block in the heap is either:

- *black* because it is reachable from the stack by a sequence of references
- *white* because it is **NOT** reachable from the mutator stack, or
- *blue* because it is on the freelist but was too small to satisfy the most recent allocation request.

### (2) The Sweep Phase

Here all blocks on the heap are visited.

- If a block is white, it is painted blue and added to the freelist.
- If a block is black, then its color is reset to white

After the sweep pase, the freelist contains only all blocks that are not reachable from the stack. And, all blocks in the heap are white except those on the freelist, which are blue.