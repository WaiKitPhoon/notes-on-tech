# SOLID

#cleancode  #java

### S - Single Responsibility Principle (SRP)

> There should never be more than one reason for a class to change.

The issue on adding to much functionality to a class - it will cause the class to not be conceptually cohesive and it will give it many reasons to change.

It’s important because if too much functionality is in one class and you modify a piece of it, it can be difficult to understand how that will affect other dependent modules in your codebase.

### O - Open/Closed Principle (OCP)

> Software entities should be open for extension, but closed for modifications.

This principle basically states that you should allow users to add new functionalities without changing existing code.

### L - Liskov Substitution Principle (LSP)

> If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of that program

If you have a parent class and a child class, then the base class and child class can be used interchangeably without getting incorrect results.

### I - Interface Segregation Principle (ISP)

No client should be required to implement methods in their classes which they will not use. Large interfaces should be split into smaller interfaces. This ensures that implementing classes only need to be concerned about the methods that are useful to them. 

### D - Dependency Inversion Principle (DIP)

> High-level modules should not depend on the low-level module but both should depend on abstraction

A class should depend on abstraction (interfaces and abstract classes) and not on concretion (classes).


SOLID principles guides a software architecture. 

> Clean architecture comes from clean systems, clean systems comes from clean components, clean components come from clean classes, and clean classes come from clean code.
> 
Boundaries are lines that separate software elements. They separate things that matter from things that don’t, i.e. high-level components from low-level components. If a high-level component depends on a low-level component at the source level, changes in the low-level components will spread to the high-level component. Therefore, we place a boundary between the two, using polymorphism to invert the logic flow. 

<img width="503" alt="image" src="https://user-images.githubusercontent.com/43614605/172988157-e1c7f29d-98ad-4123-aee9-587e17c2c357.png">

For example, let’s say your business rules depend on a concrete class accessing the database. You want to draw a boundary between the business rules and the database. The first thing to do is to define a new interface representing the database in the business rules component. Then, have your business rules depend on the interface instead of the concrete database class. Then, have the concrete database class implement your interface. The wiring between the database interface and implementation will be done in your main method manually or by some dependency injection framework.

In this way, we made the database a plug-in of our business rules. That is, we can change our database, a low-level component, without affecting our business rules, a high-level component. Uncle Bob believes all low-level components should be plug-ins, for example, the GUI, I/O, web frameworks, etc. This leads to the concept of plug-in architecture.


