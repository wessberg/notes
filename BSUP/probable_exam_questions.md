# Recap

## Software Project Management

### Why is SPM important

First, in Software projects, progress is not immediately visible. To be able to track the progress of a software project, we have to use some form of project management.

Second, we want to be able to ensure that we build the right things for our customer, and that we can keep our promises. By using project management, we reduce uncertainty, waste and risk by continuously estimating and monitoring progress.

### What is a project and its characteristics

Formally, a project is a planned activity that is non-routine, in the sense that planning is required.

It has a predetermined life span, whether it be limited by scope or time.

It will involve several activities which may be carried out in multiple phases. It will have constrained resources.

What makes Software projects unique is that:

- **They are invisible**, in the sense that progress is not immediately visible.

- **Their complexity**: They may contain more complexity than other engineered artifacts.

- **Their Flexibility**: Software systems are likely to be subject to a high degree of change.

### Project Lifecycle

#### How is a project initiated

First we have a *feasibility study* where we investigate whether or not a project is worth starting.

To do that, we gather the general requirements of the proposed system.

Multiple things play into this, including whether or not the project fits in with the software houses current portfolio as well as if it will be profitable for them.

To assess that, we would make sure that the estimated costs are exceeded by the estimated income.

We could do that by calculating the ROI (*(average annual profit / total investment) * 100*),

the Net Present Value (NPV), which indicates how much more money we'd earn by investing in the project rather than let the money sit in the bank and grow with the interest rate (calculation: *value in year t / (1 + r)^t*)

or the Internal Rate of Return (IRR) which finds the discount rate that makes the NPV equal to 0. We use it to find out which discount rate would be required for us to break-even. It is calculated: (*0 = up-front investment + (income for year / (1 + R))*). If R > actual r, accept.

#### How is a project planned

We start with an outline plan for the whole project, and then a detailed one for the first stage. We would produce detailed plans for the later stages as they approached.

Based on the kind of application we're building, we would decide on a software process model. This will greatly influence the entire project. We could go for a sequential model such as the waterfall model, which has very clearly defined steps, or we could go for an iterative model as XP or Scrum.

#### How is a project executed

Project execution involves requirements analysis, specification, design, coding, verification, implementation and maintenance. These can be formed sequentially, if following a waterfall life cycle model, or iteratively, for example with XP or Scrum, where all these things happen in parallel.

#### How is a project monitored

This varies from process to process, but we are primarily concerned with ensuring that we can deliver on time. In Scrum, for example, we use burndown and burnup charts to monitor the progress of a sprint. We could also use the Traffic-Light method to check up with developers on a regular basis on how likely the team is to deliver on their tasks.

#### How is a project controlled

Control is very different between traditional Software Project Management and Agile.
In traditional SPM, control is process centric, where there is a clear hierarchic structure. There, the programmers are typically tasked with something from their Team leader who reports to the Project manager. There is a directive, sometimes autocratic approach to controlling activities. In agile, on the other hand, the process is people centric, where teams has to be self-organized and under very little outside control. There, the organization takes on the role as a venture capitalist, and the relationship builds on trust that the team can deliver on the requirements.

## Organizational structures

### What is a Line Organizational structure

A line organization has only direct vertical relationships between different levels in the firm. It is a top-down organization where everyone has a manager.

It may suffer from overloading.

### What is a Staff or Functional Authority Organizational structure

Here, staff personnel who are specialists in some fields are given functional authority. Such staff may control processes, practices, policies or other matters relating to activities undertaken by persons in other departments.

### What is a Line and Staff Organizational Structure

Such an organization have direct vertical relationships between different levels, like a Line organization, but it also has specialists responsible for advising and assisting line managers.

### What is a Divisional Organizational Structure

Here, the organization divides large sections of the company's business into semi-autonomous groups. These are mostly self-managed and focused on a narrow aspect of the company's products or services.

### What is a Project Organizational Structure

Here, dedicated teams are put together to work on projects. **The project manager probably has line management responsibility for the project team members**.

### What is a Matrix Organizational Structure

Here, reporting relationships are set up as a grid, or matrix, rather than in a vertical hierarchy. This means that employees may report to many managers, and some from one section may report tone boss while the rest report to a different boss.

## What are the different roles in SPM

In traditional SPM, there is a focus on specialization and hierarchy. There, there can be multiple roles within a team. For example, there can be senior developers, who has specialized knowledge about an area that is relevant to the team. There will also be a team leader who reports to a project manager, who reports to the steering committee.

In Agile, this is to be avoided. There, teams should be cross-functional and self-organizing. There are still some trace of roles, for example in Scrum, where there is a Scrum Master, a Product Owner and the Development Team.

## What is an Activity network diagram

An Activity network is a directed graph-representation of activities and their interrelationships.
Here, the edges (links) are *activities*, and the *nodes* (circles) are *events*, representing the outcome of the activity finishing.

They are the basis for Critical Path Models which shows a clear path of the most important activities in the project in terms of delivering on time.

If any activities on the Critical Path are delayed, so will the completion date for the project.

## What is a Gantt Chart

A Gantt Chart indicates scheduled activity dates and durations. It shows the various activities in a horizontal representation where you can clearly see which of them are worked on in parallel and the precedence relationships. The great thing about Gantt Charts is that they provide a really clear overview of where we are in terms of delivering the final product.

## What is the Critical Path

To understand the what the critical path is, let me start by explaining what an activity network is.

An Activity network is a directed graph-representation of activities and their interrelationships.
Here, the edges (links) are *activities*, and the *nodes* (circles) are *events*, representing the outcome of the activity finishing.

What you would then do is to perform a *forward pass*, which is carried out to calculate the earliest date on which an event may be achieved and a *backward pass* to calculate the *latest* date at which an event may be achieved.

If the difference between the earliest date and the latest date for an event (this is called *the slack*) is equal to zero, that event will be on the critical path.

The critical path is a path through all critical nodes in the network. They are critical because any delay to the events along the path would delay the completion date for the entire project!

## What is the project manager's role

In traditional SPM, the project manager ahs the responsibility of planning and controlling the software development process. He or she will then report to the steering committee. In Agile, the project manager's role must be altered to that of a facilitator who directs and coordinates the collaborative efforts of those involved in development. This is because agile teams must be self-organizing. There can be only minimal outside control.

## Step-wise project planning

Step Wise is a framework of basic steps in project planning.

It only covers the planning stages of a project.

- Step 0 is to select a project.

- Step 1 is to identify the project scope and objectives (including defining who the stakeholders are).

- Step 2 is to identify the project infrastructure (how the project team is organized, which procedures to use, etc).

- Step 3 is to analyse project characteristics. Is it objective- or product-driven? What are the high-level risks? **Here, a lifecycle approach is selected**.

- Step 4 is to identify the products and activities. Create activity network and generic product flows.

- Step 5 is to estimate the effort for each activity.

- Step 6 is to identify activity risks and plan risk reduction.

- Step 7 is to allocate resources.

- Step 8 is to review and publicize the plan.

- Step 9 is to execute the plan.

- Step 10 is to do planning on lower levels, as they arrive.

## What are the difference between a project life cycle and a development life cycle

In the project life cycle, we do a feasibility study of the suggested project. We try to assert that it will produce value for the company that exceeds the initial investment. This involves evaluating the risk and resource cost of the project. If we accept the project, we can then later on decide for a development life cycle.

A development life cycle is a detailed plan of the stages involved in the development of the project, and if they should be sequentially or iteratively executed. For example, the Scrum framework is considered to be a life cycle.

## What is the difference between SPM and PM

Software projects have some pretty unique traits:

What makes Software projects unique is that:

- **They are invisible**, in the sense that progress is not immediately visible.

- **Their complexity**: They may contain more complexity than other engineered artifacts.

- **Their Flexibility**: Software systems are likely to be subject to a high degree of change.

This makes it harder to track progress. If you were building a house, you could always inspect it to see how far it was. But with software projects, progress isn't  visible to the naked eye.

And, the requirements for a software project will likely change while it is being built due to the rapid changes in the application domain.

## What is a methodology

A Methodology is a series of related methods guided by some principles. For example, Agile.

## What is a method/process

A method/process is an abstract representation of a systematic way of accomplishing something. For example, Scrum

## What is a plan

A plan is an implementation of a method/process.

## What is a Product

A product is a bit overloaded a term, but a product is the tangible outcome of something, like a deliverable or the system we're building or growing itself.

## What is the Waterfall model

The classical, sequential "one-shot" approach to development with only little space for going back to an earlier stage. Instead, flow should really be downwards with only few exceptions.

It is one that makes a lot of sense intuitively and that we all wish we could use in practice, but in reality, requirements change all the time, and we need to be able to back and forth.

## What is the Spiral model

The Spiral is an iterative, risk-oriented model.

The model goes through these four phases, iteratively:

1. Determine objectives.
2. Identify and resolve risks.
3. Development and Test.
4. Plan the next iteration.

## What is an Incremental/Iterative model

This is a model in which we repeat the steps of the model for the duration of the project.

Instead of building a product, we grow it. Each iteration brings us closer to the final deliverable product. Or, we may increment on an already deliverable product, each iteration producing a new one.

## What is prototyping

A *prototype* is a working model of one or more aspects of the projected system. It is constructed and tested quickly and inexpensively in order to test out assumptions.

Prototypes can be classified as throw-away, evolutionary or incremental. We only consider software prototypes here (not paper-and-pen prototypes)

**The Incremental/Iterative approach is based on the Spiral model and is the basis for almost all agile methods**!

**Which each iteration, the system scope is extended. The system is not built, it is grown**.

## What are agile methods

These are incremental methods where we value people over processes, working software over comprehensive documentation and customer collaboration over contact negotiation. Agile methods are highly tailored to the fact that requirements change and have short iterations to reduce risk and uncertainty.

## What are Rich Processes such as RUP

Rich processes are ones where we don't focus on production of large paper documents, but instead focus on rich representations of the software system through diagrams.

RUP, or Rational Unified Process, is based on UML and is an example of a Rich Process. It is an iterative process.

Each iteration have 4 phases:

1. Inception
	- Models the scope of the system. We perform initial cost and budget estimations
2. Elaboration
	- Here we focus on domain analysis. Define the basis architecture for the system.
3. Construction
	- Here the bulk of development happens.
4. Transition
	- The phase where the system goes from development to production.

## What is the history of Agile Software Project Management

In the late 80s we found that the classical engineering approach to SPM just didn't hold for the nature of the current state of business where change happened at an incredible pace. Within the space of three years, requirements, systems and even entire businesses were likely to change. (freestyle the rest). And so Agile was born.

## Why agile and SPM perspective

Agile is most likely not the end of the road. In order to understand the benefits of agile, we must be able to compare it with the traditional approach. And, to take this field further, we need to be able to see the whole picture.

## Agile methods and techniques

The main principles of agile are:

1. **Individuals and interactions** over processes and tools.

2. **Working software** over comprehensive documentation.

3. **Customer collaboration** over contract negotiation.

4. **Responding to change** over following a plan.

Examples of agile methods are Scrum, Kanban and Extreme Programming.

And in more words,

- Satisfy the customer through early and continuous delivery of valuable software.

- Welcome changing requirements. Harness change for the customers competitive advantage.

- Deliver working software frequently, from a couple of weeks to a couple of months - with a preference to the shorter timescale.

- Business people and developers must work together daily throughout the project.

- Build projects around motivated individuals. Give them the environment and support they need and trust them to get the job done.

- The most efficient and effective method of conveying information to and within a development team is face-to-face conversation.

- Working software is the primary measure of progress.

- Agile processes promote sustainable development. The sponsors, developers and users should be able to maintain a constant pace indefinitely.

- Continuous attention to technical excellence and good design enhances agility.

- Simplicity - the art of maximizing the amount of work not done - is essential.

- The best architectures, requirements and designs emerge from self-organizing teams.

- At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.

## What holds Agile adoption back

It is hard to change the culture or mindsets within an organization. If the management style is directive and autocratic, people will most likely be motivated by individual rewards whereas a permissive democratic style, which agile very much requires (permissive in the sense that the organization holds only lists the requirements and its up to the team to fulfil them as they please and democratic in the sense that groups are self-organizing and making decisions on their own), will favor team rewards.

Also, the project manager's traditional role of planner and controller must be altered to that of a facilitator who directs and coordinates the collaborative efforts of those involved in development. This too will require changing that managers mind.

Also, in terms of going from people (resources) being assigned directly to tasks to a collaborative social process, programmers will be accustomed to solitary activities or working with a homogenous group of analysts and designers and have a hard time adjusting. It may be overwhelming to suddenly do shared learning, reflection workshops, pair-programming and collaborative decision-making.

#### Management and organizational issues

- Organizational culture
- Management style
- Organizational form
- Management of software development knowledge
- Reward systems

#### People

- Working effectively in a team
- High level of competence
- Customer relationships - commitment, knowledge, proximity, trust, respect

#### Process

- Change from process-centric to feature-driven, people-centric approach
- Short, iterative, test-driven development that emphasizes adaptability
- Managing large, scalable projects
- Selecting an appropriate agile method

#### Technology

- Appropriateness of existing technology and tools
- New skill sets - refactoring, configuration management.

## Agile KPIs and how to measure success

KPI = Key Performance Indicator

The Agile approach to KPIs is - keep it simple!
Agile KPIs are:

- Velocity (in Scrum)
  - How many story points we can handle per iteration.
	- Burndown chart to visualize progress.
- Number of defects per release
- Code coverage
- Number of releases versus rollbacks

How do we measure success? If the customer is happy, we're happy! The customer gets a new release every 2 weeks or so. Feedback is rapid.

## How does Scrum work

Scrum is a framework based on the agile principles.

It has the ceremonies:

- Sprint planning
- Sprint execution
- Sprint Review
- Sprint Retrospective

Every sprint is short (less than or equal to one month) to keep producing value for the customer as well as decrease uncertainty and risk.

Freestyle the rest.

## What are the Scrum artifacts

- Product Backlog
- Sprint Backlog
- Release Burndown
- Sprint Burndown
- Product Increment

## What is a user story

A user story is a short, simple description of a feature **told from the perspective of the person who desires the new capability**. This would most likely, at least preferably be the customer of the system.

**This user-centric approach makes sure that we always focus on what the user wants to have, not what the user wants the system to do**.

The form of a user story: *As a <type of user>, I want to <perform some task>, so that I can <achieve some goal>*.

The user story clearly describes the goal or benefit the user is trying to achieve through it.

## What are the 3Cs of user stories

- A **Card**: A post-it note giving some tangible form to what would otherwise have been an abstraction.
- A **Conversation**: Taking place at different time and places during a project between the various people concerned with a given feature. That could be customers, users, developers, testers, etc.
- A **Confirmation**: The objectives the conversation revolved around have been reached.

## Agile estimation

Agile estimation is a team sport. Everyone will hear all questions that will arise about requirements and user stories. And, the cross-functional nature of agile teams suggests that everyone, be it developers, designers and anyone else, should participate since they have different perspectives on the product.

Practically, the product owner keeps the product backlog, which is a list of user stories, prioritized, and then the Scrum team will take out individual product backlog items during sprint planning and convert them into Sprint backlog items. To figure out how many they can handle (their *capacity*, taking into account people going on vacation, etc), they use their historical *velocity*. Their velocity is simply the amount of story points they can deliver per sprint iteration.

To estimate this, they will use historical data from their previous iterations.

Story points rate the **relative effort of work in a Fibonacci-like format**: 0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100.

At least this is what many teams decide to do, partly because numbers are intuitively understood as a great estimate and a fibonacci sequence is great for addressing uncertainty.

### Why Story points

- Dates don't account for the non-project related work that inevitably creeps into our days such as emails, meetings, etc.

- Dates have an emotional attachment to them. Relative estimation removes the emotional attachment.

## How to assign Story Points

The team use an exercise called **Planning Poker**.

## What is Planning Poker

1. The team takes an item from the backlog.
2. The item is discussed **briefly**.
3. Each team member formulates a mental estimate.
4. Every team member holds up a card with the number that reflects their estimate.
5. If everyone is in agreement, then assign that amount of story points to the user story.
6. Otherwise, take a couple of minutes to understand the rationale behind different estimates. Decide on an estimate together.

## What is Velocity and Capacity

The Velocity is the amount of story points a Development Team can handle for each iteration. It is then averaged.

If a scrum team delivered 20 story points in a sprint, their velocity is 20.
If a scrum team delivered 30 story points in the next one, their velocity would be the average of those two: 25.

*Capacity*, is the availability, in terms of hours, the team has for the upcoming Sprint. If there are team members on vacation or something like it, they should probably take on fewer product backlog items - since the capacity is expected to be less for the sprint.

## What is the difference between elapsed time and ideal time

**Ideal time** is how long something will take if **it is all you work on, everything you need is available and no one interrupts you**.

But in reality, each day has several hours of meetings and emails, effectively leaving you with 4 hours for the project.

We measure with ideal time - because its easier.

## What is the confidence interval in Agile planning

Depending on how much historical data we have over our velocity for previous sprints, the confidence interval gives us the interval of likely velocities for the upcoming sprint based on a sorted list of all sprints we've had - sorted by velocity.

For example, say we had the following historical velocities, sorted: [27, 34, 35, 38, 39, 40, 40, 41, 45]. We had then have a 90% confidence in that our velocity would be in the range [34, 35, 38, 39, 40, 40, 41]. It appears to be distributed around the mean. It's just the most likely velocities.

## What are the different sourcing and shoring arrangements when working with teams

- Outsourcing: Transferring a portion of work or an entire operation to outside providers or suppliers rather than completing it internally.

- Offshoring: Relocating a business process to another country.

- Insourcing: Like outsourcing, but with oversight from a manager who moves to the other location and inspects processes, etc.

## What are the 3Cs collaboration model when working with teams

- **Collaboration** requires
- **Coordination** which facilitates
- **Communication**

TODO: Read more about this crap. There is [this](http://gcc.upb.de/www/WI/WI2/wi2_lit.nsf/0/5098c20fcf549d15412564ca00333bc2/$FILE/Groupwar.pdf) document.

## What is Collaboration

Collaboration is working together directly. It is through individual effort that results happen, but through a shared vision.

## What is Coordination

Coordination is sharing information and resources so that each party can accomplish their part in support of a mutual objective. It is about teamwork.

## What is Cooperation

Cooperation is where relevant information and resources is exchanged, **but the goal may not be the same** for the two parties.

## What is communication

It is an interaction.

## What is Awareness

Awareness is an understanding of the activities of others, which provides a context for ones own activities.

Awareness facilitates communication. Communication generates awareness. Coordination generates awareness. Awareness is required for good team dynamics!

## What is Tools for teams

- For a single person in a single site team, CASE Tools and automation tools would be a great idea (diagram tools, process modeling tools, etc) to keep track of the complexity.

- For several people in a single site environment, collaboration tools is required (such as Slack)

- For several people in a multi-site environment, we additionally need:
  - Coordination tools. Such as an online knowledge database or kanban board.
	- Communication tools. Such as instant messaging.
	- Awareness tools. Could be lots of things, video chat maybe?

## What are some communication dichotomies

We can have *direct* communication between two parties, or we can have *indirect* communication between two parties through artifacts.

We can have *formal* communication, where the interchange of information is done through pre-defined channels or we can have *informal* communication in which the interchange does not follow any predefined channel.

## What are computer-mediated communication theories (CMC)

Media can be characterized along three dimensions of information exchange:

- Time (when)
- Space (where)
- Richness (how much)

**Synchronous collocated communication** could be face-to-face conversation.

**Synchronous disperse communication** could by phone, chat or video conferencing

**Asynchronous collocated communication** could be a billboard.

**Asynchronous disperse communication** could be an email or a letter.

One key theory in this field are the *Media Richness Theory*.
It states that:

- the more complex the task, the richer the media to use.
- Lean media is better for uncertain tasks. Rich media is better for imprecise tasks.

Another one is Task/Technology Fit (TTF).
It states that:

- The greater the *fit* between what a task requires and what some technology provides, the higher the likelihood of us using it. And, if we have a successful (and pleasant) experience with it, the likelihood of us using it again in the future increases.

Other points of TTF include:

- Rich media do not always provide the best solution for any given task.

- Too much or too few media richness for a given task represents a poor TTF.

## What are Mintzberg's coordination mechanisms

"An understanding of the activities of others provides a context for your own activity".

1. Mutual adjustment
	- Coordination of work is made possible by a process of informal communication between people conducting interdependent work.
2. Direct supervision
	- Coordination is achieved by one individual taking responsibility for the work of others.
3. Standardization of work processes
	- Coordination is made possible by specifying the work content in rules or routines to be followed. Coordination occurs before the activity is undertaken. Procedures are usually specified by work-study analysis.
4. Standardization of output
	- Coordination is obtained by the communication and clarification of expected results.
5. Standardization of skills and knowledge
6. Standardization of norms

## What are awareness in CSCW (Computer Supported Cooperative Work)

We have:

- Workspace awareness: The up-to-the-minute knowledge of other participants interactions with the shared workspace.
- Informal awareness: The general sense of who's around and what they are up to.
- Group-Structural awareness: knowledge about such things as peoples roles and responsibilities, their positions on an issue, their status and group processes.
- Social awareness: Information that a person maintains about others in a social or conversational context.

## What is Global Software Development

Global Software Development is the development of software where the distribution of the members exceeds the borders of a country.

## Why Global Software Development

It has some benefits:

- Most talented developers.
- Cheaper costs
- Proximity to market.
- Time to market (e.g, follow-the-sun)

## What are the different kinds of distribution in Global Software Development

We can have:

- **Globally Distributed teams**: Here, there are teams across multiple locations.
- **Globally Dispersed teams**: Here, the team members are working from different locations.
- **Globally Partially-dispersed teams**: Here, some team members are working from different locations, and some teams are collocated.

## What are virtual teams

A group of people working on a project, but residing in different locations.

*"A group of geographically, organizationally, and/or time dispersed workers brought together by information and telecommunication technologies to accomplish one or more organizational tasks"*.

In virtual teams, there are a high level of interdependence and cooperation among team members, in comparison with other outsourcing/insourcing of work.

Especially in the case of self-organizing teams, it is crucial to have as many functions performed in all locations to avoid silos which can affect team dynamics and performance.

Advantages of virtual teams over loosely coupled distributed teams are also:

- It's easier to create a common spirit and a shared goal across locations.

- The network of virtual teams is more stable, which increases constancy.

- Know-how is shared and transferred across locations as a part of teamwork that reduces risk, especially for long-term projects.

- The risks regarding immoderate competition or blockade situations are lower.

## What are some of the challenges of Global Software Development

With GSD, oftentimes tasks take a lot more time to complete than in collocated teams. one of the reasons is the relative lack of communication and coordination.

But a great benefit can be productive hours of a day which can be extended from the 8-10 hour norm to 24-hours by working across multiple timezones.

Of course, this wouldn't be a great thing if we were using virtual teams where synchronous communication is key.

## Issues with physical separation

Whether the separation is very local - sitting in two adjacent office buildings - or very global - working across national borders, the issues are very similar.

### Strategic issues

Deciding how to divide up work across sites is difficult. Also, the organization's fundamental resistance to GSD can be a challenge. Many believe their jobs are threatened and experience a loss of control.

### Cultural issues

Cultures differ on many critical dimensions, such as the need for structure, attitudes toward hierarchy, sense of time and communication styles. This can be difficult because misunderstandings can happen.

### Knowledge management issues

Promptly and uniformly sharing information from customers and the market among the development teams can be difficult. And, in terms of agile where knowledge is tacit - it can be especially difficult, since the knowledge is shared through "osmosis".

It might be that the distributed teams aren't aware of which tasks are currently on the critical path.

### Project and process management issues

If two distributed teams define a process differently, we have an issue. Synchronization requires commonly defined milestones and a clear entry and exit criteria!

### Technical issues

I wouldn't call it as relevant anymore, but this goes for sharing data and communicating across multiple sites where connectivity may be limited.

## What are the impacts of distances and Global Software Development

Here are the kinds of distances we use:

- Geographic distance: Being physically separate from each other.

- Temporal distance: Living in different time zones make synchronous communication very difficult.

- Cultural distance: Negatively impacts the level of understanding and appreciation of the activities and efforts of remote colleagues.

- Linguistic distance: The lack of a common native language creates further barriers to communication.

## How do we alleviate the effect of distances

In general, the less need there is to communicate and clarify requirements, the better. If the tasks are relatively stable and the requirements are relatively limited, it can be done.

**Rule 1: Reduce intensive collaboration**.

The organizational culture differences between the two organizations will also play a role in terms of the unit's norms and values. Which methodologies and project management practices are used can vary greatly. And, national cultural practices can also be very different. Cultural norms or language differences play a big part in how we collaborate professionally.

**Rule 2: Reduce cultural distance**.

Asynchronous communication are fine for many cases, but synchronous communication such as a phone call or face-to-face meeting has some advantages: A small issue can take days of back-and-forth discussions over email or Slack, but a brief conversation will often quickly clarify and hopefully resolve the issue. So speaking with colleagues that are not sitting in the same room as you can greatly reduce the time taken to solve issues.

Synchronous communication can be difficult if the time zones are very different.

If you use the *follow-the-sun* approach where another team takes over when the team leaves, it will be near to impossible to do synchronous work due to the large difference in time zones.

**Rule 3: Reduce temporal distance**.