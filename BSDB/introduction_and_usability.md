# Introduction and Usability

> Lauesen 1.1-1.3
>
> Schneiderman 1.1-1.3

## Usability

Usability has to do with how easy a software system is to use.
With a low-usability system, you wonder whether the system is stupid or you are.

Our job is to always think that only the system can be stupid (even though Trump won the 2017 election). We can make the system easier to use, but we cannot change human nature.

### User Interface

The User Interface is the part of a system that you **see**, **hear** and **feel**.

But, also the imagination or understanding of what goes on "behind the curtain" is often quite important for the user to deal with a system that doesn't work as usual (among other things).

### Human-computer interaction

Has to do with the interaction between the human using a system and the system itself. In some cases, the human gives orders by means of the mouse and keyboard. The system then replies by doing something. Other times, the system gives an order which the human must then fulfill. It's a two-way street.

### Interactive systems

When we talk about Human-computer interaction, it implies that there are interaction going on. Thus, we refer to *interactive* systems.

### Non-interactive systems

There are systems, such as a cinema, that are non-interactive. The customer doesn't interact with the movie (yet).

### Interface

The interaction between human and computer takes place through the *user interface*. A keyboard, mouse and even a loudspeaker are also parts of the user interface. All points for interaction are parts of the user interface, even though we tend to think of it as a purely visual thing.

## Technical interfaces

When a system has an interface to another system, we're talking about a technical interface. A system can also have interfaces to physical processes such as temperature sensors and whatnot.

These technical interfaces are **not** user interfaces since the user doesn't interact **directly** across them. Instead **the user interacts indirectly with them through the user interface**.

## Design of user interfaces

While it is easy to design a user interface - just provide buttons and whatnot for all user-facing features - it is definitely *not* easy to design a user interface that is **easy to use**, which is the main ingredient of Usability.

It is how we define ease-of-use that are interesting - and how we design for it using the relevant methodologies.

## Quality factors in IT systems

Ease-of-use is of course not the only quality factor of a system.
Others include (but not limited to):

- Correctness
- Availability
- Performance
- Security
- Ease of use
- Maintainability

These are also called non-functional requirements in Object-Oriented analysis.
Of course, the counterpart to those are the functional requirements - that the system supports the necessary features.

You *could* say that quality factors beside ease-of-use are a matter of usability - in terms of the developers point of view. For example, maintainability is important for users to be able to correct and enhance a system. And that could be called a matter of usability.

But, the book only concerns itself with usability in terms of:

- **Functionality**: Providing the system features that allow the user to carry out his tasks.

- **Ease of use**: Providing the features in such a way that it is easy to learn the system and easy to carry out the tasks.

## Usability factors

Usability consists of six factors:

1. **Fit for use (functionality)**: That the system can support the tasks that the user's tasks.

2. **Ease of learning**: How easy is the system to learn for various groups of users?

3. **Task efficiency**: How *efficient* is it for the frequent user?

4. **Ease of remembering**: How easy is it to remember for the occasional user?

5. **Subjective satisfaction**: How satisfied is the user with the system?

6. **Understandability**: How easy is it to understand what the system does? This factor is particularly important in unusual situations, for instance error situations or system failures. Only an understanding of what the system does can help the user out.

**Ease-of-use is a combination of factors 2-6**. So, Ease-of-use comes from the non-functional requirements whereas the functional requirements giving in *Fit for use* has nothing to do with *how* the tasks are performed and perceived.

And, just like weighing design goals in OO design, these must be weighed too and prioritized differently according to the type of system that you're building.

### Game program usability

Games are interesting. They need not be *task efficient* (well, became that after Call of Duty :-D). It shouldn't be too easy to beat the component.

This is a great example of how usability factors must be weighed in terms of the system at hand. Here, *subjective satisfaction* is of greatest importance.

### Why are usability important?

There are many reasons. Users save time, more people can use the system, customer satisfaction === $$$ and so on.

## Usability problems

Usability problems are anything that hampers the user. For example, that he cannot figure out how to carry out his task or finds it too cumbersome.

It is important to identify the usability problems of a system to improve the usability of it!

### (Usability) Defects

Usability problems are a special kind of system *defects*. The system works as intended by the developer, yet the user interface is inconvenient or hard to understand.

### Types of Defects

- **Program Error (or bug)**: If the system doesn't work as intended by the programmer, we have a program error.

- **Missing functionality**: If it is impossible to carry out the task, the system is not *fit for use*.

- **Ease of use problem**: If the system works as intended by the programmer and can support the task, yet the user cannot figure out how to do it or doesn't like the system, **the system is not easy to use**. We classify them according to their severity to the user, for instance a *task failure* or a *minor problem*.

### Examples of usability problems (ease of use problems)

- **P1**: The user cannot figure out how to start the search. The screen says that A, but the user doesn't see it until B.

- **P2**: The user believes that he has completed the task and that the result has been saved, but he should have pressed *Update* before closing the window.

- **P3**: The user cannot figure out which discount code to give the customer, although he knows which field to use.

- **P4**: The user says that it is completely crazy having to go through six screens in order to fill in ten fields.

- **P5**: The user wants to print out the list of discount codes but cannot find out how to. He calls hot-line who says that the system cannot do that.

### Classification of ease-of-use problems according to severity classes

We typically use some predefined *severity classes* to describe how severe the identified usability problems are. These are:

- **Missing functionality**: The system cannot support the user's task.

- **Task failure**: The user cannot complete the task on his own or he erroneously believes that it is completed. (e.g. the system supports the task, but it wasn't fulfilled successfully by the user).

- **Annoying**: The user complains that the system is annoying or cumbersome. Or, we observe that the user doesn't work in the optimal way.

We can also use the following three classes, some of which may include the 3 above:

- **Medium problem**: The user finds the solution after lengthy attempts.

- **Minor problem**: The user finds the solution after a few short attempts.

- **Critical problem**: Missing functionality for an important task, or task failure that occurs for many users, or annoying to many users.

**We usually focus on the critical problems and handle them before considering any other problems**.

Unfortunately, we cannot ensure that *all* users succeed with everything. People are different.

### Schneiderman's two cents on achieving high-quality user interfaces

Schneiderman states that in order to achieve high-quality user interfaces, we must provide quality features such as usability, universality and usefulness.

These goals are achieved by thoughtful planning, sensitivity to user needs, devotion to requirements analysis and diligent testing, all while keeping within budget and on schedule.

### Difference between Lauesen's and Schneiderman's definitions of Usability factors

They are mostly similar, but Schneiderman lists *Rate of errors by users* as a factor whereas Lauesen exchanges that with *Understandability*.

## Usability motivations

I already covered this a bit earlier, but the motivations for usability and the weighing of the usability factors differ greatly from system to system.

Let's consider a few examples.

#### Life-critical systems


In Life-critical systems, subjective satisfaction isn't really important. Here, there are probably lots of training happening before the actual users of the system gets their hands on it. Also, ease-of-learning is not that important since the budget and time taken to train the end-user is probably pretty high. Instead, *fit for use* and *task efficiency* are the two most important things, in fact they are critical for this kind of systems.

#### Industrial and commercial uses

These include banking, insurance, order entry, production management, airline and hotel reservations, etc.

Here, operator training time is expensive, so *ease-of-learning* is important. Retention is obtained by frequent use. Beside these two, it really depends on the kind of industry as well as budget.

### Home and entertainment applications

These include, for example, e-mail clients, search engines, cellphones, cameras, music players, computer games and such.

For these, ease of learning and subjective satisfaction are paramount. If the users cannot succeed quickly, they will give up and move on to a competitor.

### Exploratory, creative and collaborative interfaces

These include music composition tools, video-editing tools and collaborative text editors.

Here, two or more people may be able to work together through use of text, voice, video and whatnot. The users may be highly knowledgeable in the task domains ("application domain" in OOA) but novices in the underlying computer concepts.

Their motivation is high, but so are their expectations. These kind of interfaces should "fade away" and let the user keep focus on the task with minimal distraction.

### Sociotechnical systems

These include systems for health support, identity verification, disaster response and crime reporting. Often created by governmental organizations.

These have to deal with trust, privacy and responsibility and limit the harmful effects of malicious tampering, deception and incorrect information.

Here, designers have to take into account the diverse levels of expertise of users with different roles. Ease-of-remembering and ease-of-learning are hugely important here.
