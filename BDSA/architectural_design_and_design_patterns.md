## Architectural Design & Design Patterns
> [OOSE] Chapter 6, 7, 8.

### What is software architecture
- The design process for identifying the sub-systems making up a system and the framework for sub-system control and communication is <u>Architectural design</u>
- The output of this design process if a description of <u>software architecture</u>

### Architectural design
- An early stage of the system design process.
	-	Represents the link between spec and design processes.
	- Often carried out in parallel with some spec activities.
	- Involves identifying major system components and their communications.

- <u>Advantages:</u>
	- **Stakeholder communication.**
		-	architecture may be used as a focus of discussion by system stakeholders.
	- **System analysis**
		-	Means that analysis of whether the system can meet its non-functional requirements is possible.
	- **Large-scale reuse**
		-	The architecture may be reusable across a range of systems.

### Architectural design decisions
- Architectural design is a *creative process* so the process differs depending on the type of system.
- *However*, a number of common decision span all design processes.

- **Important questions:**
	-	Is there a generic application architecture that can be used?
	- How will the system be distributed?
	- What architectural styles are appropriate?
	- What approach will be used to structure the system?
	- How will the system be decomposed into modules?
	- What control strategy should be used?
	- How will the architectural design be evaluated?
	- How should the architecture be documented?

### Architecture reuse

- Systems in the same domain often have similar architectures that reflect domain concepts.
- Application product lines are built around a core architecture with variants that satisfy particular customer requirements.

### Architectural views
- It is impossible to represent all relevant information about a system's architecture in one single architectural model.
	-	Each model shows one view or perspective.

- **Krutchen (1995) 4+1 views:**
	-	Logical view - Concerned with the functionality that the system provides to end-users. Shows key abstractions in the system as objects or object classes.
		-	E.g. class diagram, communication diagram, sequence diagram.
	- Process view - Shows how, at run-time, the system is composed of interacting processes.
		- E.g. Activity diagram.
	- Development view - Shows how the software is decomposed for development.
		-	E.g. Component package diagram.
	- Physical view - Shows the system hardware and how software are distributed across the processors in the system. E.g. deployment diagram.
	- Use Case view

## Object-Oriented Design

### Purpose
Transforming an analysis model into a system design model.

Comes after analysis.

### Typical architectural concerns and system characteristics

- Performance
- Security
- Safety
- Availability
- Maintainability
- Others

### System design tasks
1) Identify design goals
	-	First step in ODD.
	- Identify software qualities.
	- Inferred from non-functional requirements (and the client/user)
	- **Trade-offs:**
		-	Some design goals can go against other design goals.
		- For instance, if a concern is both space and speed, but speed requires additional space, a decision must be made between the two non-functional requirements.
		- Another example is delivery time vs functionality. Some features may require additional delivery time.
	- **Typical design goals:**
		-	Performance
		- Dependability
		- Cost
		- Maintenance
		- End-user criteria

### System design concepts
- **Subsystems:**
	-	A **replaceable** part of the system with **well-defined** interfaces that encapsulates the state and behavior of its contained classes.
- **Identifying subsystems:**
	- initially derived from functional requirements.
	- Keep functionally related objects together (cohesion)
- **Services & subsystem interfaces:**
	-	**Service:**
		-	A set of related operations that share a common Purpose
		- interface of a sub-system
			-	Main focus during design - not implementation
		- API

### Coupling & cohesion
- **Coupling:**
	-	The number of dependencies between subsystems.
	- *"Loose"* or *"strong"* coupling.
	- We want **loose** coupling to enable easier changing of sub-system dependencies.
- **Cohesion:**
	-	The number of dependencies within a subsystem.
	- *"High"* or *"Low"* cohesion.
	- We want **high** cohesion because things in a sub-system should be closely related to each other.

## Patterns

Patterns are ways to describe best practices, good design and capture experience in a way that is possible for others to reuse.

- Stylized, abstract descriptions of good practice.
- Tried and tested in different environments.
- Has proven to be successful.
- Describes when to use it (and when not to)

### Reporting patterns
When reporting patterns, you should provide:
- A pattern name.
- A description.
- Example
- Advantages
- Disadvantages

### Architectural patterns/styles

An architectural pattern is for instance **Model-View-Controller (MVC)**

We know enough about that one already, so let's just move on.

#### Layered Architecture
- Used to model the interfacing of sub-systems.
- Organizes the system into a set of layers (or abstract machines), each of which provide a set of services.
- Supports the incremental development of sub-systems in different layers. When a layer interface changes, only the adjacent layer is affected.
- However, often artificial to structure systems in this way.

#### Repository architecture
- Sub-systems must exchange data in any of the following two ways:
	1) Shared data is held in a central db or Repository and may be accesses by all sub-systems
	2) Each sub-system maintains...

#### Pipes and Filters architecture

- Functional transformations process their inputs to produce outputs.
- May be referred to as a pipe and filter model (as in UNIX shell).
- Variants of this approach are very common. When transformations are sequential, this is a batch sequential model which is extensively used in data processing systems.
- Not really suitable for interactive systems.

## Design pattern overview
- **Structural:**
	-	Adapter
	- Facade
	- Composite
	- Decorator
	- Bridge
	- Singleton
	- Proxy
	- Flyweight
- **Behavioral:**
	-	strategy
	- State
	- Command
	- Observer
	- Memento
	- Interpreter
	- Iterator
	- Visitor
	- Meditator
	- Template method
	- Chain of responsibility
- **Creational:**
	-	Builder
	- Prototype
	- Factory method
	- Abstract factory
