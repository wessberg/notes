## System design and object design
> [OOSE] ch. 9+10
>
> (Optional) [SE9] ch. 7

### Mapping subsystems to processors and components
- Most systems run on multiple computers/VMs
- Hardware/VM mapping has significant impact on performance and complexity (is done early in a project)
- HW/VM mapping often leads to design of more components for data transport/mapping
- HW/VM distribution allow for exploit parallel processors, but comes with a cost in terms of handling
	-	concurrency
	storing, replicating, transferring data.

### Identifying and storing persistent data

#### Identifying persistent objects
-	Typically entity objects/classes.
- Data that should survive system shutdown/restart/crash
	-	Can be quite a lot

#### Storage management strategy

-	**Files**:
	- Binary data stored in the local filesystem.
	- No finer grained access/read/write control
	- Used for large/temporary data.

- **Relational DB (SQL)**:
	-	Data stored in table(s) in large, distributed RDMS system.
	- Fine-grained concurrency control.
	- Used for fine-grained data management in stable data models.

- **noSQL (Document DB, KeyValue)**:
	-	Data stored in large key-value pairs (essentially JSON)
	- Less constrained, simple design, horizontally scalable.

### Providing access control

- In a multi-user system, different actors have access to different functionality and data.
	-	Modelled during OOA in the use case diagram.
	- In OOD, we model how actors can control/access objects.
	- ... + authentication mechanisms.

- Access control list/matrix
	- Rows and actors of the system.
	- Columns are classes
	- Cells list the access rights.

### Access matrix
#### Global Access table
- global for a whole system, controlling access globally.
- (`actor`, `class`, `operation`)
- If no match, access denied.

#### Access Control list
- List pr. class controlling access on a class level
- (`actor`, `operation`)
- Every time an object is accessed, its access control list is checked.
- If no entry found, access denied.

#### Capability list
- list pr. actor controlling what an actor can do (“invitation card”)
- (`class`, `operation`)
- if no capability, no access


![Access matrix example](assets/access_matrix.png "Access matrix example")

![Proxy pattern example](assets/proxy_pattern.png "Proxy pattern example")

#### Procedure-driven control
- operations wait for input from actor(s)
- “old school” procedural, legacy systems
- pipe-and-filters architectures
- Event-driven control
- main loop wait for external event and dispatch them to interested parties
- modern UI / OOP systems
- MVC / event architectures
- Threads
- each event spawn a concurrent thread / event handler
- modern UI / web systems
- client/server architectures
