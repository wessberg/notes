# Goals, Environments and Tools

> CS:APP / Section 1 p38-64

## System

A system is a set of interconnected components with a well-defined behavior and that interfaces with its environment.

### History

#### Modularity

With time, it has become more modular and interconnected. Many different players come up with components.

## Fundamental Abstractions

3 fundamental abstractions for computer systems

- Interpreter
- Memory
- Communication

![Abstractions](asset/abstraction_examples.png)

We can map these into:

- Processor (*Interpreter*)
- Main Memory (*memory*)
- I/O devices (*Communication*)

The operating system builds on top of this (the layer that operates the hardware).

Finally, at the highest level, application programs.

### Memory abstraction

We `WRITE(name, value)` and `READ(name)` to/from memory.
You could say the same thing about writing to disk or the file system.

![Memory Abstraction](asset/memory_abstraction.png)

### Interpreter abstraction

Has:

- A pool of instructions
- and some data from memory

Executes instructions one-by-one

![Interpreter Abstraction](asset/interpreter_abstraction.png)

### Communication abstraction

We `SEND(link_name, outgoing_message_buffer` and `RECEIVE(link_name, incoming_message_buffer)`.

It's about sending and receiving data, over locally over some port (between a keyboard, printer, e.g.).

![Communication Abstraction](asset/communication_abstraction.png)

## A bus

A Bus is a **shared** communication channel across hardware components.

### System Bus

Is a bus that connects to an I/O Bridge and the memory bus.

### Memory Bus

Is the bus that Main memory is connected to

### I/O Bridge

The I/O Bridge connects to the I/O bus

### I/O Bus

The I/O Bus is the bus that stuff like USB controllers, graphics adapter and disk controllers are connected to.

**The CPU accesses all other devices than main memory indirectly through the System bus -> I/O Bridge -> I/O Bus**.

## CPU Cache

The L1, L2 and L3 cache are levels of memory closer to the processor, so you don't have to go RAM for *every* task (since that in itself introduces latency). Also, the processor will be faster than main memory - so it will be equally as fast to use the embedded L1/L2/L3 memory.

## How does main memory work

- Main memory is an array of bytes.
- Each byte has a unique address
- The address space is linear

## Memory hierarchy

![Memory pyramid](asset/memory_pyramid.png)

## Address space

For 64-bit processors the address-space is *2<sup>64</sup>*

## Latency

![Latency](asset/latency.png)

## Operating System

Manages the hardware.

Has:

- Processes
- Virtual Memory
- Files

![OS abstraction](asset/os_abstractions.png)

## Context switching

Only a **OS Kernel** can decide when to perform a context switch on the CPU-level.

![Context switching](asset/context_switching.png)

## OS Kernel

- Starts when the computer boots
- Manages all the computer's (hardware) resources
- Partitions the memory into *kernel space* (reserved to the kernel) and *user space* (all applications)
- The OS kernel exposes an interface to the user space applications (*"the system calls"*).

## Virtual Memory

![Virtual memory](asset/virtual_memory.png)

## I/O Devices

In Linux/UNIX, files are a universal abstraction for all I/O Devices

**A File is an array of bytes**.

## System Programming

How to write programs that manage computer hardware?

- OS Kernel
- Embedded systems
- Infrastructure software that must tightly control its use of hardware resources:
		- Compilers
		- Database Systems
		- Version Control

## Why C

- More portable than Assembly ("high-level" assembly)
- Efficient

### C as a programming language

- An *imperative* language
- A *permissive statically typed language*.

## Take-aways

- Processes are interpreters
- Memory is an array of bytes
- I/O Devices are seen as files