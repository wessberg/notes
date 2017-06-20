# No Silver Bullet - Essence and accidents of Software Engineering

> [No Silver Bullet - Essence and Accidents of Software Engineering](https://learnit.itu.dk/pluginfile.php/166159/course/section/88124/1987%20-%20Brooks%20-%20No%20Silver%20Bullet.pdf)

This article compares Software projects to werewolves where they appear normal but all of a sudden transform into this giant monster. To defeat it, you need a silver bullet. But for Software, there are no silver bullet, it states.

In other words, there are no easy way to put a faulty software project at ease. And the reason is that the errors surrounding a software project almost always lies in the requirements.

The point of the article is that developer errors can be solved by higher-level languages, IDEs, automatic- and graphical programming, but the main issue is requirements errors. And those can only be resolved by rapid prototyping.

It lists up some properties of the *essence* of modern development:

1. **Complexity**: They are complex for their size because they have so many possible states. The complexity of an application grows much more than linearly. **The complexities are the essence** and thus we cannot abstract them away to make it simpler.

2. **Conformity**: Much of the complexity that the software engineer must master is arbitrary: Forced without rhyme or reason. The differ from interface to interface. It must conform to other interfaces.

3. **Changeability**: Things like cars and computers change too, but in the sense that they are superseded by later models. With software, it is the software itself that must change. And, successful software survives beyond the normal life of the machine for which it is first written. It must be conformed to support new environments.

4. **Invisibility**: Software is invisible. It has no geometric representation, and visualizing it is hard.

## What has reduced or solved difficulties of Software Development

### 1. High-level languages

High-level languages improved reliability, simplicity and comprehensibility. It frees up a program from much of its accidental complexity (for example, automatic garbage collection).

As well as time-sharing (I assume they refer to shorter compile-times here or JIT) and integrated development environments (IDEs).

## Hopes for the future (in terms of programmer fault)

The article is from the 80s, but it appears to be further modularization, object-oriented programming and AI.

It also covers "Automatic programming" and "Graphical programming". Basically, making it as high-level and non-human as possible.

## But the biggest problems are in specification faults

We can have the best techniques for writing good code and even if our code fits the requirements to perfection, the requirements themselves may be faulty.

## Rapid Prototyping

The article thinks that rapid prototyping is the hope for the future, because *the client does not know what he wants*. It requires extensive iteration between the client and the designer as part of the system definition.

By creating prototypes for the client to test iteratively, we get much more precise requirements.

## Incremental development - grow, don't build software

"The *building* metaphor has outlived its usefulness". The article thinks that *building* something speaks to sequential development, and by thinking about it as *growing* software, we then see it as something that always exists, but evolves.