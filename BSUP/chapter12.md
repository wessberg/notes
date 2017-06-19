# Chapter 12 - Software Quality

> [SPM] Chapter 12

## Software qualities

There are 3 overall kinds of qualities:

- Product operation qualities
- Product revision qualities
- Product transition qualities

### Product operation quality factors

- **Correctness**: The extent to which a program satisfies its specifications and fulfils the user's objectives.

- **Reliability**: The extent to which a program can be expected to perform its intended function with required precision.

- **Efficiency**: The amounts of computer resources required by the software.

- **Integrity**: The extent to which access to software or data by unauthorized persons can be controlled.

- **Usability**: The effort required to learn, operate, prepare input and interpret output.

### Product revision quality factors

- **Maintainability**: The effort required to locate and fix an error in an operational program.

- **Testability**: The effort required to test a program to ensure it performs its intended function.

- **Flexibility**: The effort required to modify an operational program.

### Product transition quality factors

- **Portability**: The effort required to transfer a program from one hardware/software configuration to another.

- **Reusability**: The extent to which a program can be used in other applications.

- **Interoperability**: The effort required to couple one system to another.

## Quality criteria for each factor

The following should be laid down for each quality:

- **Scale**: The unit of measurement.
- **test**: The practical test of the extent to which the attribute quality exist.
- **Worst**: The worst acceptable value.
- **Plan**: The value that it is planned to achieve.
- **Best**: The best value that appears to be feasible (The "state of the art" limit).
- **Now**: The value that applies currently (if any).

## ISO 9126

ISO 9126 identifies six software quality characteristics.
Each of these characteristics have sub-characteristics:

- **Functionality**: Covers the functions that a software product provides to satisfy user needs.
  - Suitability
	- Accuracy
	- Interoperability
	- Compliance
	- Security

- **Reliability**: The capability of the software to maintain its level of performance.
	- Maturity
	- Fault tolerance
	- Recoverability

- **Usability**: The effort needed to *use* the software.
	-	Understandability
	- Learnability
	- Operability

- **Efficiency**: Physical resources used when the software is executed.
	- Time Behavior
	- Resource Behavior

- **Maintainability**: The effort needed to make changes to the software.
	-	Analysability
	- Changeability
	- Stability
	- Testability

- **Portability**: The ability of the software to be transferred to a different environment.
	-	Adaptability
	- Installability
	- Conformance
	- Replaceability

We can't have everything! So we need to select and prioritize quality metrics.

## Techniques to help enhance software quality

### Increasing visibility

*Egoless programming* encourages programmers to look at each others code.

### Procedural structure

Having a structured approach where he software development cycle has some carefully laid down steps will make sure that the software quality fits the quality requirements.

### Checking intermediate stages

By checking the correctness of work at earlier, more conceptual stages, we make sure to keep the quality on track.

## Inspection

A technique where coworkers will spend time going through the work of another, noting any defects. Then, a meeting is held where the work is discussed and a list of defects requiring rework is produced.

Its good because:

- It removes superficial errors from work.
- It motivates the programmer to "do better" and also write more self-explanatory and well-documented code.
- It helps spread good programming practices.
- Developers get to know the code base very well.
- It enhances team spirit.

### The Fagan method

This is where:

- Inspections are carried out on all major deliverables.
- All types of defects are noted, not just logic or function errors.
- Inspections are carried out using a predefined set of steps.
- Inspection meetings do not last for more than two hours.
- The inspection is lead by a moderator who has had specific training in the technique.

### Conclusion

- Practical quality requirements have to be carefully defined.
- There have to be practical ways of testing for the relative presence or absence of quality.