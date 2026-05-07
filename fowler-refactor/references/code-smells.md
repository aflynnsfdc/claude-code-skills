# Reference: Code Smells (Martin Fowler, 2nd Ed)

Use this list to diagnose issues before selecting a refactoring technique.

| Smell | Description | Typical Cure |
| :--- | :--- | :--- |
| **Mysterious Name** | Variables, functions, or classes with names that don't reveal intent. | Rename Variable/Function |
| **Duplicated Code** | The same code structure appears in more than one place. | Extract Function, Slide Statements |
| **Long Function** | A function that tries to do too many things (usually > 20 lines). | Extract Function |
| **Long Parameter List** | Functions with too many arguments, making them hard to use. | Replace Parameter with Query, Introduce Parameter Object |
| **Global Data** | Data accessible from anywhere, leading to unpredictable bugs. | Encapsulate Variable |
| **Mutable Data** | Data that changes in place, making it hard to track state. | Split Variable, Replace Derived Variable with Query |
| **Divergent Change** | One module is commonly changed in different ways for different reasons. | Split Phase, Extract Class |
| **Shotgun Surgery** | Making a single change requires small edits to many different classes. | Move Function/Field, Inline Class |
| **Feature Envy** | A function that seems more interested in the data of another module. | Move Function |
| **Primitive Obsession** | Using primitives (strings, ints) for domain concepts (money, phone). | Replace Primitive with Object |
| **Repeated Switches** | The same `switch` logic appears in multiple places. | Replace Conditional with Polymorphism |
| **Loops** | Procedural loops that could be more expressive. | Replace Loop with Pipeline |
| **Lazy Element** | A class or function that isn't doing enough to justify its existence. | Inline Function/Class, Collapse Hierarchy |
| **Speculative Generality** | "Just in case" code that isn't actually needed yet. | Remove Dead Code, Collapse Hierarchy |
| **Message Chains** | Navigating long paths of objects (e.g., `a.b().c().d()`). | Hide Delegate |
| **Middle Man** | A class that does nothing but delegate to another. | Remove Middle Man |
| **Large Class** | A class that has too many fields or responsibilities. | Extract Class/Superclass |
