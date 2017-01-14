## SOLID Principles
>[OOSE] ch. 6, 7, 8

## SOLID Principles
### **Single Responsibility Principle**
- A class should have one, and only one reason to change.
- A class should have only a single responsibility.

### **Open/Closed Principle**
- Software entities should be open for extension, but closed for modification.
- So, make sure that a class can be easily extended (e.g. subclassed) without breaking anything.

### **Liskov Substitution Principle**
- Derived classes should be usable through the base class interface, without the need for the user to know the difference.
- Functions that has references to base classes must be able to use objects of derived classes without knowing it.

Read about implementation/specification inheritance in `object_design.md` for more explanation.

### **Interface segregation principle**

- Many client-specific interfaces are better than one general-purpose interface.

- Instead of having lots of logic in the base class that may not be useful to all derived classes, place that logic in one of them or if some of them share the logic, provide a class that derives from the base class and them have the classes that depend on it derive from the new one.

### **Dependency inversion principle**
- Depend upon abstractions, do not depend upon concretions.
- High level modules should not depend upon low level modules. Both should depend upon abstractions.
- Use interfaces. Don't depend on submodules

## Bad code which SOLID attempts to solve

These principles are based on not running into the following bad symptoms of **high coupled code**:

- **Confusing software**

- **Rigidity**: To change something, you need to touch a lot of other stuff.

- **Fragility**: You change something somewhere, and it breaks the software in unexpected areas that are entirely unrelated to the change you made.

- **Immobility (no-reusability)**: The software does more than what you need. The desirable parts of the code are so tightly coupled to the undesirable ones that you cannot use the desirable parts somewhere else.
