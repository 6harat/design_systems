# generic notes for system designs:
* key philosophies for programming:
    + SOLID:
        - Single Responsibility Principle: a class should only have one reason to change
        - Open-Closed Principle: entities should be open for extension, closed for modification
        - Liskov Substitution Principle: objects in a program should be replaceable with the mplementation of their subtypes without altering the correctness of the program
        - Interface Segregation: multiple specific interfaces are better than single generic one
        - Dependency Injection: entities should depend upon abstractions and not concretions
    + KISS: Keep It Simple, Stupid
    + DRY: Don't Repeat Yourself
    + WET: We Enjoy Typing (common in multi-tiered architecture where a new form field needs to be added in multiple places to ensure the integrity of the flow)
    + SSOT: Single Source of Truth (store data only once, any possible linkages are by reference only)
    + SoC: Separation of Concerns (separate a computer program into different broad sections which each handle single concern)
    + RoT: Rule of Three (if a code is duplicated twice, it is fine, but a third duplication should result in moving the repeated code to a common function)
    + Package Principles:
        - Principles of Package Cohesion:
        - Principles of Package Coupling:
        __Note__: more details https://en.wikipedia.org/wiki/Package_principles
    + YAGNI: You Aren't Gonna Need It (do not add functionality until deemed necessary)
    + GRASP: General Responsibility Assignment Software Patterns
        __Note__: more details https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)
* key philosophies for database:
    + ACID:
        - Atomicity: a transaction (with multiple statements) is either all successfully completed or rolled back
        - Conistency: a transaction can only bring the database from one valid state to another maintaining database invariants (constraints, cascades, triggers, etc.)
        - Isolation: ensures that concurrent execution of transactions leave the database in the same state that would have been obtained if the transactions were executed sequentially
        - Durability: ensure that once the transaction has been committed it will remain committed even in the case of power failure
    + CAP:
        - Consistency:
        - Availability:
        - Partition Tolerance:
    + BASE: Basically Available, Soft-state, Eventual consistency
* basic layout: