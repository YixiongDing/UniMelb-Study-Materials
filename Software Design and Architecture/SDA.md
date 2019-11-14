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

- - -
## Chapter 2: Organising Domain Logic – Principle and Patterns
- - -

Typically, an enterprise system is divided into the presentation, domain logic, and data source layers

In this chapter, we will look at three architectural design patterns and principles for use in the domain logic layer in enterprise systems

### Patterns for domain logic layer

1. Transaction script
2. Domain model
3. Table module
4. Service layer

### The problem

#### How do we organise the domain logic of an enterprise system to make it understandable and maintainable?

The domain layer design:
1. should be structured make it clear how to identify the transactions that it supports
2. and the effects these transactions have on the domain
3. be extensible so that new types of transactions and new business rules can be added while minimising the change impact on the rest of the layer, and the system in general

### The patterns

#### Transaction Script

Organises business logic by procedures where each procedure handles a single requirement from the presentation

##### **Transaction**: 

Read some information from the system, modify some information in the system, calculate some new information, or any combination of the three. Captures some business logic that is used to support an enterprise

##### **Transaction script pattern**: 

Organises the domain layer such that each transaction is organised into a single procedure (or script), with common behaviour factored into shared subprocedures. In general, these scripts should not interact, and there should be no dependencies between them

##### **Implementation**

The common theme is:
1. A single method/procedure for each transcript
2. This method/procedure can use other methods/procedures to execute the transaction
3. Each transaction is served by exactly one script, and each script serves exactly one transaction

##### **Pros**

1. *Simplicity*:
    - Major attraction of the transaction script pattern is its simplicity
    - Simple to model and understand
    - Knows the transactions of the system, straightforward to find the corresponding behaviour
    - Maintenance is straightforward because one needs only to learn about a single transaction to modify it

2. *Transaction boundaries*:
    - Straightforward to identify the transaction boundaries (where a transaction starts and ends)
    - Each transaction is opened at the start of its corresponding transaction script, and closed at the end of its transaction script

##### **Cons**

1. *Scalability*:
    - Does not scale well. As the business logic increases in complexity, the complexity of the domain layer increases exponentially

2. *Duplication:*:
    - Tends to result in duplicate code. Because a transaction script can be understood in isolation, and indeed implemented by different develops, there tends to be no over-arching view of the different transactions. 
    The common behaviour tends to be difficult to spot because: 
        - (a) the common behaviour will be implemented in different ways by different developers;
        - (b) generally no single person understands how all of the transactions are implemented, and the structure of the patterns encourages this


#### Domain model

An object model of the domain that incorporates both behaviour and data

