# Design Pattern

Design patterns are common architectural approaches to solve common problems

## SOLID Design Principles

Frequently references in Design Pattern literature

| Principles                        | Description                                                                |                        Link                         |
| --------------------------------- | -------------------------------------------------------------------------- | :-------------------------------------------------: |
| _Single Responsibility Principle_ | A class should only have one reason to change                              | [[Single Responsibility Principle \| :SiSolidity:]] |
| _Open-Closed Principle_           | Classes should be open for extension but closed for modification           |      [[Open-Closed Principle \| :SiSolidity:]]      |
| _Liskov Substitution Principle_   | You should be able to substitute a base type for a subtype                 |  [[Liskov Substitution Principle \| :SiSolidity:]]  |
| _Interface Segregation Principle_ | Don't put too much into an interface; split into separate interfaces       | [[Interface Segregation Principle \| :SiSolidity:]] |
| _Dependency Inversion Principle_  | High-level modules should not depend upon low-level ones; use abstractions | [[Dependency Inversion Principle \| :SiSolidity:]]  |

---

## The Patterns

### Gamma Categorization

Design Patterns are typically split into three categories
This is called Gamma Categorization after Erich Gamma, one of GoF authors

- _Creational Patterns_
  - Deal with the creation (construction) of objects
  - Explicit (constructor) vs. implicit (DI, reflection, etc.)
  - Wholesale (single statement) vs. piecewise (step-by-step)
- _Structural Patterns_
  - Concerned with the structure (e.g., class members)
  - Many patterns are wrappers that mimic the underlying class' interface
  - Stress the importance of good API design
- _Behavioral Patterns_
  - They are all different; no central theme

[[The Patterns.png]]

---

| Pattern   | Problem                 |            Link             |
| --------- | ----------------------- | :-------------------------: |
| `Builder` | complex object creation | [[Builder \| :SiSolidity:]] |
| `Factory` |                         |                             |

---
