### Coupling, Cohesion and Encapsulation

Low coupling, high cohesion and encapsulation results in a system that is easier to understand and to change.

1. Coupling:
    - Coupling is a measure of how interconnected one element (function, class, module) is to another. Tight coupling creates dependencies and ties elements together, making a system harder to understand, test and change. Changes to one element require subsequent changes that can ripple through the system. We can reduce coupling by defining standard connections or interfaces between elements of a system, removing their knowledge of each other, and allowing interaction with an element only through it's defined interface.
    - Pure functions are inherently decoupled as they are deterministic and don't produce side effects. They can be dependant on other pure functions only. A class or module can expose public properties and methods that act as their interface and define their interactions (API).
2. Cohesion:
    - Cohesion is measure of how closely related all the data and responsibilities of an element are to each other. It is measure of how well-defined an element's role is. Elements that have a single responsibility and focus are easier to understand and utilise, in isolation and within a larger system.
    - It's important that an element's properties and methods work together to define a sense of purpose. Low cohesion leads to confusing systems that are difficult to change and test, potentially resulting in unwanted defects. One of the best ways to improve cohesion in a system is to eliminate duplication.
3. Encapsulation:
    - Encapsulation refers to the ability to use an element through it's interface alone, and limit direct access to or hide it's internal data and implementation. This stops the responsibilities of one element from leaking into another, and protects the internal state from undesirable external access and modification.

---

### Simple Design

1. Run all the tests:
    - The design must produce a system that acts as intended. A system that is comprehensively tested and passes all of its tests is verifiable. Arguably, a system that isn't verifiable shouldn't be deployed. Writing tests pushes us towards a design where our functions and classes are small and single purpose. Tight coupling makes it difficult to write tests. By applying principles like single responsibility, dependency inversion and tools like interfaces and abstraction, we minimise coupling and make the system more testable. Writing tests leads to better designs.
2. No Duplication (DRY):
    - Duplication is the primary defect of a well designed system, it represents additional work, additional risk and additional unnecessary complexity. Duplication can exist in various forms. Repeated lines of code are unnecessary and should be abstracted to a shared function. Repeated implementation (one or more pieces of code with the same or overlapping functionality) can be eliminated by applying the single responsibility principle, abstracting commonality, and encouraging reuse and composition. By making these abstractions on small pieces of code, we reduce the complexity of the larger system and improve testability. Everything should be said once and only once.
3. Expressive:
    - When working on complex projects, in large teams, over a long period of time, it's critical for us to be able to easily understand what a system does and the problem it solves. If the code is written in a convoluted and confusing way, it is easy to introduce defects when making changes. As a system becomes more complex, it takes more time for a developer to understand, and there is more opportunity for misunderstanding. Code should clearly express the intent of the author. We can keep functions and classes small, and choose good names that clearly define their responsibility. By using standard nomenclature we can communicate common functionality to other developers. Well written unit tests are also expressive, acting as documentation by example.Spend enough time on, and give sufficient thought to making your code expressive. Care is a precious resource.
4. Fewest Elements:
    - In a effort to make our functions and classes small, we may create too many small elements in the code. A balance needs to found. Our goal is to keep our overall system small, whilst also keeping our functions and classes small. Be pragmatic.

---

### Dependency Inversion Principle

- High level classes should not be dependant on low level classes, both should depend on a standard interface. The interface represents the essential functionality that doesn't change, and removes the need for tight coupling between classes. In this way classes can easily be changed or substituted.
- Analogy: a plug socket provides a standard interface for electrical appliances. We do not want to solder our kettle directing into the wiring of the house.