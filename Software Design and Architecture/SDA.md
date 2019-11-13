# Software Design and Architecture Notes

Summary is wriiten by Yixiong Ding  
The University of Melbourne  
November, 2019   

- - -
## 1. Introduction
- - -

### 1.1 What is an enterprise system?

#### Definition 1.1 (Enterprise system)

An enterprise system is a software system that supports organisations in their goals to handle business processes, resource planning & management, service management, and business intelligence

Breakdown:

1. Enterprise systems are software systems that are part of a larger organisational system. Therefore, they are typically not stand-alone packages that run on a desktop PC with a single user. Instead, they support multiple users and often a diverse set of features that are related to the enterprise

2. Enterprise systems are used in enterprises. Therefore, enterprise applications are used by businesses and government and generally not used by individuals

3. Enterprise systems are used to support business in management of their key resources. This means that enterprise systems are typically business oriented. Word processors are not enterprise systems, even though they are used within organisations, because they do not support key businesses processes

4. Enterprise systems support enterprise goals. Therefore, enterprise systems are generally closely related to the domain in which they are deployed, and in many cases, to the organisation in which they are deployed

#### Common properties of enterprise systems

1. Persistent data
2. A lot of data
3. Concurrent access
4. Many user interfaces
5. Build on business logic
6. Integrate with other enterprise systems

### 1.2 Architecture, design, principles, and patterns

#### Definition 1.2 (Software architecture)

A software architecture is a high-level view of a software system’s components, how those components behave, how they relate to each other, and how they relate to their environment, for the purpose of achieving their stated functionality. A software architecture must meet the needs of its intended users over an extended period of time and under different patterns of usage

In enterprise systems, the views considered important can be extracted directly from Definition 1.2:
1. Structural: A static view of the components and how they are related to each other
2. Behavioural: A dynamic view of how the components behave; that is, a specification of the inputs and outputs of a component
3. Interaction: A dynamic view of the flow of control and data between the components

#### Architectural design vs. detailed design

An architecture is a blue print for the system. It is primarily concerned with what the high-level components are, the behaviour of each of these components, and how the components relate to one another

This is one step above the detailed design, which is concerned with class structure, data types, and algorithms. **The line between software architecture and detailed design is blurred**, and there is no clear place in which we stop architectural design and start detailed design: both are iterative activities


#### Principles

OOD Symptoms:

- Rigidity: The design is difficult to change
- Fragility: The design is easy to break
- Immobility: The design is difficult to reuse
- Viscosity: It is difficult to do the right thing
- Needless complexity: The design contains elements that aren’t currently useful
- Needless repetition: Redundant modules/code in the system makes it is difficult to maintain it
- Opacity: The design has modules that are difficult to understand

To help developers eliminating the symptoms of poor software design, Robert Martin defined a set of guidelines that helps to achieve a good software design, which is known as Principles of Object Oriented Class Design. The acronym **SOLID** was coined by Michael Feathers to help remembering them

These five SOLID principles are:

1. **Single Responsibility Principle**: A class should have one, and only one, responsibility. This principle states that if we have two reasons to change for a class this means the responsibilities become coupled, we have to split the functionality in two classes. Because when we need to make a change in a class having more responsibilities,the change might affect the other functionality of the classes

2. **Open Closed Principle**: You should be able to extend a classes behaviour, without modifying it. It means simply this: We should write our modules so that they can be extended, without requiring them to be modified. Indeed, abstraction is the key here

3. **Liskov Substitution Principle**: Derived classes must be substitutable for their base classes. That is, a user of a base class should continue to function properly if a derivative of that base class is passed to it

4. **Interface Segregation Principle**:  Make fine grained interfaces that are client specific. For example, if you have a class that has several clients, rather than loading the class with all the methods that the clients need, create specific interfaces for each client and multiply inherit them into the class

5. **Dependency Inversion Principle**: Depend on abstractions,not on concretions. That is, depend upon interfaces or abstract functions and classes, rather than upon concrete functions and classes

By applying these principles during to design object oriented systems, it can be noticed that some structures appears repeatedly over and over again which are known as design patterns

