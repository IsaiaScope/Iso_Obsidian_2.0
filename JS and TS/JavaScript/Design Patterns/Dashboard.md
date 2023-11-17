# Design Pattern

Design patterns are common architectural approaches to solve common problems

[Design patterns in javascript](https://medium.com/globant/design-patterns-in-javascript-creational-2a02726e4e71)

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

| Pattern                   |                       Link                       |
| ------------------------- | :----------------------------------------------: |
| _Creational Patterns_     |                                                  |
| `Builder`                 |         [[Builder \| :FasBabyCarriage:]]         |
| `Factory`                 |         [[Factory \| :FasBabyCarriage:]]         |
| `Prototype`               |        [[Prototype \| :FasBabyCarriage:]]        |
| `Singleton`               |        [[Singleton \| :FasBabyCarriage:]]        |
| _Structural Patterns_     |                                                  |
| `Adapter`                 |         [[Adapter \| :FasBabyCarriage:]]         |
| `Bridge`                  |         [[Bridge \| :FasBabyCarriage:]]          |
| `Composite`               |        [[Composite \| :FasBabyCarriage:]]        |
| `Decorator`               |        [[Decorator \| :FasBabyCarriage:]]        |
| `Façade`                  |         [[Façade \| :FasBabyCarriage:]]          |
| `Flyweight`               |        [[Flyweight \| :FasBabyCarriage:]]        |
| `Proxy`                   |          [[Proxy \| :FasBabyCarriage:]]          |
| _Behavioral Patterns_     |                                                  |
| `Chain of Responsability` | [[Chain of Responsability \| :FasBabyCarriage:]] |
| `Command`                 |         [[Command \| :FasBabyCarriage:]]         |
| `Interpreter`             |       [[Interpreter \| :FasBabyCarriage:]]       |
| `Iterator`                |        [[Iterator \| :FasBabyCarriage:]]         |
| `Mediator`                |        [[Mediator \| :FasBabyCarriage:]]         |
| `Memento`                 |         [[Memento \| :FasBabyCarriage:]]         |
| `Observer`                |        [[Observer \| :FasBabyCarriage:]]         |
| `State`                   |          [[State \| :FasBabyCarriage:]]          |
| `Strategy`                |        [[Strategy \| :FasBabyCarriage:]]         |
| `Template Method`         |     [[Template Method \| :FasBabyCarriage:]]     |
| `Visitor`                 |         [[Visitor \| :FasBabyCarriage:]]         |

---

## Summary

- _Builder_
  - Separate component for when object construction gets too complicated
  - Can create mutually cooperating sub-builders
  - Often has a fluent interface
- _Factories_
  - Factory method more expressive than constructor
  - A separate class with factory methods is a Factory
  - Class hierarchies can have corresponding hierarchies of factories (Abstract Factory)
- _Prototype_
  - Creation of object from an existing object
  - Requires either explicit deep copy or copy through serialization
  - Additional work required to preserve type
- _Singleton_
  - When you need to ensure just a single instance exists
  - Can return same object from constructor on every call
  - Direct dependence on a Singleton is dangerous
- _Adapter_
  - Converts the interface you get to the interface you need
- _Bridge_
  - Decouple abstraction from implementation
- _Composite_
  - Allows clients to treat individual objects and compositions of objects uniformly
- _Decorator_
  - Attach additional responsibilities to objects without modifying those objects or inheriting from them
  - Decorators are composable with each other
- _Façade_
  - Provide a single unified interface over a set of systems/interfaces
- _Flyweight_
  - Memory saving technique
  - Efficiently support very large numbers of similar objects
- _Proxy_
  - Provide a surrogate object that forwards calls to the real object while performing additional functions
  - E.g., access control, communication, logging, etc.
- _Chain of Responsibility_
  - Allow components to process information/events in a chain
  - Each element in the chain refers to next element; or
  - Make a list and go through it
- _Command_
  - Encapsulate a request into a separate object
  - Good for audit, replay, undo/redo
  - Part of CQS/CQRS
- _Interpreter_
  - Transform textual input into object-oriented structures
  - Used by interpreters, compilers, static analysis tools, etc.
  - Compiler Theory is a separate branch of Computer Science
- _Iterator_
  - Provides an interface for accessing elements of an aggregate object
  - Objects can be made iterable (for loop)
- _Mediator_
  - Provides mediation services between two objects
  - E.g., message passing, chat room
- _Memento_
  - Yields tokens representing system states
  - Tokens do not allow direct manipulation, but can be used in appropriate APIs
- _Observer_
  - Allows notifications of changes/happenings in a component
- _State_
  - We model systems by having one of a possible states and transitions between these states
  - Such a system is called a state machine
  - Special frameworks exists to orchestrate state machines
- _Strategy & Template Method_
  - Both define a skeleton algorithm with details filled in by implementor
  - Strategy uses ordinary composition, template method uses inheritance
- _Visitor_
  - Allows non-intrusive addition of functionality to hierarchies

---
