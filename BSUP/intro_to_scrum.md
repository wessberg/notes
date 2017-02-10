# (Lecture notes) Intro to scrum
> February 10th, 2017.

### What is scrum
"A team sport". Comes from Japan. Go the distance as a unit, passing "the ball" back and forth rather than as in the waterfall model. Each team member has a specialized function in the team *in this specific moment (sprint)* of the scrum.

- Products get worked on in parallel.
- No discrete sequential development stages.

- Scrum is an agile process that allows us to focus on delivering the highest business value in the shortest time.

- It allows us to rapidly and repeatedly inspect actual working software (every two weeks to one month).

- The business sets the priorities. Teams self-organize to determine the best way to deliver the highest priority features.

- Every two weeks to a month, anyone can see real working software and decide to release it as is or continue to enhance it for another sprint.

### Characteristics
- Self-organizing Teams
- Product progresses in a series of 1-4 week "sprints".
- Requirements are captured as items in a list of **"product backlog"**.
- No specific engineering practices prescribed. (Extreme programming).
- Uses generative rules to create an agile environment for delivering projects.
	-	Typically means a **daily** standout meaning (maximum 15 minutes) where everyone speak about what they did yesterday, what they are going to do today and what problems they might have or have had. It is typically during this daily standout that problems arise. So many people use it.
- One of the "agile processes".

### Hierarchy-less
Scrum does not impose a hierarchy. It is hierarchy-less.

### Product owner
The product owner is the shield between the team and the stakeholders.
It is *not* necessarily true that what the product owner states as the most important/highest prioritized feature is the feature that the development team will be working on in the next sprint. The development team decide to do another part first.

### Scrum team
A scrum team has a
- **development team** (~5-9 members),
- a **product owner** (sometimes does more than just being the link between the team and stakeholders. *Not* a boss.) and
- a **ScrumMaster** (**Usually part of the development team, takes on a leading role**).

### Trust
Trust is a core-focus of scrum. As a ScrumMaster or product owner, you need to trust the individual developers (or the development team as a whole) to do their job. That can be hard.

## The Agile Manifesto
Contains the core essence of why plan-driven development was rejected in favor of the values that are the basis for all agile-driven development methodologies like scrum:

- Individuals and interactions over process and tools.
- Working software over comprehensive documentation.
- Customer collaboration over contract negotiation.
- Responding to change over following a plan.
## Scrum framework

We have a daily scrum meeting.

### Backlog
You have a
- Product backlog
- Sprint backlog. A sprint contains a subset of the product backlog. Takes 1-4 weeks (typically 2-4).

### Potentially shippable product increment
Is the work product of each sprint cycle.

### Sprint goal
Here you decide the goals for a concrete sprint. It has a subset of the product backlog which becomes work items/tasks (individual units of work) for the sprint.

A sprint should generally include something that will be visible to the user (e.g. they should contain visible value for the stakeholders).

### Changing the iteration of a sprint
You can do it, but should avoid it if possible. **A fixed sprint length leads to a better rhythm**.

### Sequential versus overlapping development with relation to sprint length (Why short sprints are important)
Where sequential development do requirements, design, code and test in sequence, scrum does a little bit of everything in parallel.
Thus, the longer the length of the sprint, the easier it is to fall for the pitfalls of plan-driven development. Then you'd be able to say *"Okay, let's do requirements in week 1, design in week 2, code in week 3 and test in week 4"*. We want to encourage parallel development.

### NO change inside a sprint
Scrum welcomes change. **But a sprint is locked!**

The contract states that developers promise stakeholders that they *will* deliver whatever items are on the sprint backlog.
In return, the stakeholders promise not to give new requirements that needs to go with the current sprint before completion (these items will instead be put on the product backlog).

Of course, in the real world, sometimes urgent matters arise. Typically, if the ScrumMaster is a developer him/herself, that would typically be the person to do it, as he/she is (usually) most decoupled from the team dynamics.

## Key parts of scrum
- Roles
	-	Product owner
	- ScrumMaster
	- Development team
- Ceremonies
	-	Sprint planning
	-	Daily scrum meeting
	- Sprint Review meeting
	- Sprint retrospective meeting
- Artifacts
	-	Product backlog
	- Sprint backlog
	- Burndown charts

### Product owner
- An interface to the "client" (real or hypothetical - sometimes the client is the company itself).
- Defines the feature of the product.
- Decides on release date and content.
- Is responsible for the profitability of the product (ROI)
- Prioritizes features according to market value.
- Adjusts features and priority every iteration, as needed.
- Accepts or reject work results.

### The ScrumMaster
- Represents management to the project.
- Responsible for enacting Scrum values and practices. He's normally the one who knows scrum the best.
- Removes impediments (anything that block the team.). To:
- Ensure that the team is fully functional and productive:
	-	Coaches the team to their best possible performance.
	- Helps improve team productivity in any way possible.
- Enable close cooperation across all roles and functions.
- Shield the team from external interferences.
- The best ScrumMasters are leaders by nature. They need to take care of the team members and their well-being.

### The (development) team
- Typically 5-9 people.
- Cross-functional:
	-	Programmers, testers, UX designers, etc.
- Members should be full-time
	-	There may be exceptions (e.g. database administrator).
- Teams are Self-organizing
	-	Ideally, no titles but rarely a possibility (people want careers).
- Membership should change only **between** sprints.

### Sprint planning
A long meeting between team, ScrumMaster and product owner.
The agenda:
- Discuss top priority product backlog items.
- The team selects what to do.

Why:
- So that everyone knows what will be worked on.
- So that everyone *understands* the backlog items enough to actually work on them.

Work results:
- Sprint goal
	-	A one-liner that sums up the purpose of the sprint.
- Sprint backlog
	-	The set of backlog items (units of work) that will be worked on during the sprint to reach the sprint goal.
	- Each item is assigned an **estimate** (in terms of how long it'll take. Like, 8 hours.) It doesn't have to be hours. They shouldn't be higher estimated than 16 hours.

So, you do product design and technical design and *plan* them during sprint planning.

### Daily scrum meeting (standup meetings)
It's all about the team synchronizing. Knowing what everyone is doing, who needs help (pair-programming) and so on.

**Standup meetings are not closed. Everyone can join**. But:
**Only team members, ScrumMaster and product owner can talk**.

- It *must* be daily (and therefore):
- It *must* be 15 minutes maximum.
- People should stand-up. That is an incentive to be fast. If people are already standing, the meeting will be shorter.

These help avoid other unnecessary meetings.

Everyone answers 3 questions:

1. What did you do yesterday?

2. What will you do today?

3. Is anything in your way?

### Sprint review meeting
Is about presenting what is accomplished during the sprint.
**It typically takes the form of a demo of new features or underlying architecture**.

- The whole team participates.
- Everyone is invited.

It takes approximately 2 hours. Reason: Stakeholders might participate.

### Sprint retrospective meeting
Here we take a look and what is working or not **after each sprint**. We look inward.
- Typically takes 15-30 minutes.
- They are done after every sprint.

During these meetings, we typically divide things into:
- What should we start doing
- What should we stop doing
- What should we continue doing

This allows us to speak about actionable things.

### Product backlog
A prioritized list of **requirements**. **They are prioritized by the product owner**.
No need for much detailing. It is a list of desired work that the product might benefit from.

The list is reprioritized at the start of each sprint.

### Sprint backlog
Handled by the team.
**Individuals sign up for work of their own choosing**. Work is *never* assigned to a team member. The team member himself picks the item.
- **Estimated work remaining is updated daily**. Typically by the ScrumMaster.
- A sprint backlog item *can* be removed from the sprint backlog (put back in the product backlog) by the development team as long as the goal of the Sprint is not affected.

#### Example
<table>
	<tr>
		<td>Tasks</td>
		<td>Monday</td>
		<td>Tuesday</td>
		<td>Wednesday</td>
		<td>Thursday</td>
		<td>Friday</td>
	</tr>
	<tr>
		<td>Code the user interface</td>
		<td>8</td>
		<td>4</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td>Code the middle tier</td>
		<td>16</td>
		<td>12</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td>Test the middle tier</td>
		<td>8</td>
		<td>16</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
</table>

### Burndown chart
Shows how the work is going throughout the spring. Shows the amount of remaining work hours in the y-axis and the date on the x-axis. As time progresses, it will (hopefully) go toward zero. When it reaches zero, there is no more remaining work items.

You can extract some interesting data from such burndown charts. The different generalized shapes they might take can indicate something about the sprint that may be useful.

For example, if all items are so highly estimated that they cannot be closed along the sprint, the burndown chart will have a straight line until the last day where it just goes to zero since all developers work on their individual items until the last day where they commit and close the issues.
This is also another motivation for keeping estimates small - so that you can better track how the sprint is going.
