# Lecture 8

> Programming Language Concepts, Chapter 9

## An Overview of Abstract Machines

An Abstract machine is one that can execute programs in an *intermediate instruction-oriented language*. Sort of like machine-code, but one level higher.

**This intermediate language is often called bytecode**, because the instruction codes are short and simple compared to the instruction set of "real" machines such as x86, PowerPC or ARM.

Abstract machines are also known as *virtual machines*.

Examples are:

- Postscript
- Java Virtual Machine (JVM)
- Microsoft's Common Language Infrastructure (CLI)

## Purpose of Abstract Machines

The main purpose is typically to **increase the portability and safety of programs in the source language**.

Having the Abstract Machine as the target means that the developer only need a single compiler, rather than having to compile to different targets (such as 32-bit, 64-bit, x86, PowerPC, etc). The Abstract Machine (for example JVM) can then run *on* any platform that supports JVM (practically anything supports JVM). Yes, it requires the host machine to install JVM - and yes it is super annoying. But in theory, it makes a lot of sense.

Microsoft's Common Language Infrastructure has very much the same goals as JVM and serves the same purpose. It is a part of .NET.

Where CLI is more general than JVM in all-around, it is still visibly slanted towards class-based, statically typed, single-inheritance OO languages such as Java and C#.

Also, CLI is build with JIT in mind. Thus, CLI bytecode instructions are **not** explicitly typed! The JIT compilation phase must infer the types anyway, so there is no need to give them explicitly.

LLVM is a compiler infrastructure that offers an abstract instruction set and hence a uniform view of different machine architectures. Just like with JVM and CLI, this means that by using LLVM, the developer can target multiple architectures. Apple does just that in order to target the iPhone (using the ARM architecture) and macOS (using the x86 architecture).

## The Java Virtual Machine (JVM)

### JVM Run-Time State

JVM has the following components:

- Classes that contains methods, where methods contain bytecode
- A heap that stores objects and arrays
- A frame stack for each executing thread
- Class loaders, security managers and other things that are somewhat unimportant components in this context.

### Heap (in JVM)

The heap is used for storing **values that are created dynamically and whose lifetimes are hard to predict**.

This means things like all arrays and objects - including strings (since strings are arrays of chars).

### Garbage Collector (in JVM)

THe heap is managed by a *garbage collector*, which makes sure that unused values are thrown away so that the memory they occupy can be reused for new arrays and objects!

### Frame stack (in JVM)

This is a stack of frames.

### Stack frame (in JVM)

This includes:

- Local variables for the current method
- The local evaluation stack for the current method
- The program counter (pc) for the current method

The local variables also includes the current object reference (`this`), if the method is non-static. This will be the first local variable, followed by the method's parameters and the method's local variables.

### JVM sizes of values

The size of a value in JVM is:

- a 32-bit word for booleans, bytes, characters, shorts, integers, floats and references to an array or object.
- Two 32-bit words for longs and doubles. **A local variable holding a value of this kind occupies two local variable indexes**

### What can be stored in the local evaluation stack

In JVM, only primitive type values such as `int`, `char`, `boolean`, `double`, etc, and references, can be stored in a local variable or in the local evaluation stack.

All else, such as objects and arrays and even strings are stored in the heap! A local variable and the local evaluation stack can of course hold a reference to a heap-allocated object or array.

### JVM Bytecode

Here, a local variable is named **by its index**. This is essentially the local variable's declaration number!

At all times (in a non-static method), the current object reference (`this`) has a local variable index 0. The first method parameter has index 1, and so on.

In a static method, the local variable with index 0 would be the first argument, and so on.

In the bytecode, an instruction name prefix indicates the argument type. For example, addition of integers is done by the instruction `iadd`. And, addition of single-precision floating-point numbers is done by `fadd`.

### Bytecode Verification (in JVM)

Before it executes some bytecode, it will perform so-called *bytecode verification*. This is a load-time check. This is to improve security. The program should not be allowed to crash the JVM or to perform illegal operations.

This verification process checks:

- That all bytecode instructions work on stack operands and local variable of the right type
- That a method uses no more local variables than it claims to
- That a method uses no more local stack positions than it claims to
- That a method throws no other checked exceptions than it claims to
- That for every point in the bytecode, the local stack has a fixed depth at that point (and thus the local stack does not grow without bounds)
- That the execution of a method ends with ar eturn or throw instruction and does not "fall off the end of the bytecode"
- That execution does not try to use one half of a two-word value (a long or double) as a one-word value.

## The Common Language Infrastructure (CLI)

This is very similar to JVM in that it has a heap, a frame stack, bytecode verification, etc.

Its' bytecode is called Common Intermediate Language (CIL or MSIL).

### Common Intermediate Language

It differs from JVM's bytecode in the following respects:

- It has a more advanced type system than that of JVM to better support source languages that have parametric polymorphic types (generics).
- It includes several kinds of pointer, native-size integers (32 to 64-bit).
- It supports tail calls. (What, doesn't JVM do that?)
- Permits the execution of unverified code to support languages such as C and C++

### CIL Bytecode

Rather than have individual instructions for adding integers or single-precision floating-point numbers, CIL uses overloaded instructions on type. There is only one `add` instruction. Load-time type inference determines whether it is an `int` add, `float` add, `double` add, and so on.

This, again, aligns with the JIT nature of CIL, rather than relying on bytecode interpretation. Since a JIT compiler will have to traverse the bytecode anyway, where types can be inferred, there is no need to add the complexity in the bytecode.

### Generic types in CLI vs JVM

In CLI, generics are supported even at the bytecode level. In JVM, the bytecode has no notion of generics. Instead, generic arguments are simply replaced by the type `Object` in the bytecode. This ultimately means that generics in JVM is basically just sugar.

For Java, there are some consequences of this limit in JVM:

- Since type parameters are replaced by type Object in the bytecode, a type argument in Java must be a reference type such as `Double`. It simply cannot be a primitive type such as `double`. Makes sense!
- Since type parameters do not exist in the bytecode, you cannot use the type parameter in an `instanceof` test or in reflection.

## Decompilation

Since bytecode for JVM and .NET CLI includes a lot of info, including *metadata* which is class names, interfaces, types of fields, etc, one can usually *decompile* the bytecode files to obtain source programs that are really similar to the original programs.

This is a bit controversial since this means that people can "steal" algorithms and other intellectual property by reversing the compiled bytecode into a program. Something called "obfuscators" has been developed (like mangling) which are tools that transform bytecode files to make it harder to decompile them.

## Just-In-Time Compilation

An Abstract Machine is not as efficient as compiling bytecode to "real" machine code. One of the reasons is the added overhead by `switch` statements in the interpreter(s) for bytecode instructions.

So, for performance, one typically compiles the bytecode to real machine code, for instance for x86 or ARM.

This can be done immediately when the abstract machine loads the bytecode program, or it may be done *adaptively*, so that only bytecode fragments that are executed frequently, for example stuff inside a loop, will be compiled to real machine code.

Both of these things are callde *JIT compilation*.

Such a compiler meets conflicting demands:

- It should be fast (as in: It should start fast)
- It should also generate fast high-quality machine code

For a server application, it makes sense to aim for faster, high-quality machine code, but in an interactive application that should start immediately, it must load really fast and may not be able to generate as fast machine code.

Some abstract machines may be given argments to dictate their optimization settings.

### JVM JIT flags

Given the flag `-server`, the JIT will have slower code generation, but generate faster code.

Given the flag ´-client`, the JIT will have faster code generation, but slower generated code.

### JIT compilers vs C/Fortran

Even though modern JIT compilers generally produce good machine code, it is not quite as good as a highly optimizing C or Fortran Compiler!