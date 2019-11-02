# Modelling Complex Software Systems Notes

Summary is wriiten by Yixiong Ding  
The University of Melbourne  
June, 2019   
_ _ _

## 1. Concurrency
- Lecture Con.01 -- Introduction to Concurrency
    - March 6, 2019 12:05pm-1:00pm
    - March 8, 2019 3:20pm-4:15pm

### Concurrent Program
1. Sequential program:
    - has a single thread of control, that is, a single instruction pointer suffices to manage its execution
2. Concurrent program:
    - allows multiple threads of control.
    - each thread, or process, in a concurrent program often either shares data or communicates with one or more other threads in that program

### Parallelism vs concurrency
1. A program written in a concurrent programming language may be executed with or without actual parallelism; on a single processor typically by time-sharing.
2. Conversely, a program written in a sequential programming language may be executed with parallelism; for example, via vectorization

### Why Concurrency
1. A natural model
2. Necessity
3. Performance

### Concurrent programming is hard
1. **Communication**: Processes generally need to communicate with each other, either by accessing shared data, or by message passing
2. **Synchronization**: Processes may need to synchronize certain events, such as “P must not reach point p until after Q has reached point q.”

### Non-determinism
1. A major source of frustration is the fact that the execution of a concurrent program is non-deterministic
2. Unlike sequential programs, a concurrent program may be **speed-dependent** — its behaviour may depend on the relative speeds of its components’ execution
3. Small, random fluctuations in processor or input-output speed are sources of observed non-determinism

### Concurrent programming language paradigms
1. Shared-memory: assignment 1a
    - Utilising the concept of monitors
    - These have a long history from Concurrent Pascal to Java and C#
2. Message-passing: assignment 1b
    - Based on Hoare’s idea of Communication Sequential Processes (CSP)
    - Examples include Occam, Erlang and Go

### Atomicity
In most programming languages an assignment such as n := n+1 is not atomic; a compiler will break it up into more basic instructions.

### Interference
Interleaving causes problems because there are many different
possible interleavings that can occur, and each execution may yield a different interleaving. However: 
*A concurrent program must be correct for all possible interleavings.*

### Safety and liveness properties
Properties of concurrent systems:
1. **Safety**: “nothing bad will ever happen”
    - **Interference** (or rather its absence) is an archetypal safety property
2. **Liveness**: “something good eventually happens”
    - **Deadlock** (or rather its absence) is an archetypal liveness property

### Summary
1. Concurrency is **potential** parallelism
2. Concurrency is an **abstraction** that makes it easier to reason about the dynamic behaviour of a system 
3. Formal approaches to understanding concurrent systems are required, because their behaviour is often **non-deterministic**
4. In a concurrent program, atomic operations can be interleaved arbitrarily
5. For a concurrent program to be correct, it must be correct for **all** possible interleavings

## 2. Java threads; mutual exclusion
- Lecture Con.02 -- Java threads and Mutual Exclusion
    - March 8, 2019 3:20pm-4:15pm
    - March 13, 2019 12:05pm-1:00pm

### Threads in Java
Java calls a process a “thread” 
- There are two ways to create threads in Java. 
1. Extend the java.lang.Thread class:
<img src="java-thread1.png" alt="550" width="550">
Create an instance of this class:
<img src="java-thread2.png" alt="550" width="550">
The first statement creates the thread.
The call to start() causes the new thread to call its run() method and execute it independently of the caller

As Java does not support multiple inheritance, you may not always be able to extend class Thread.

2. The alternative (and usually recommended!) way to create a thread is to implement the Runnable interface:
<img src="java-thread3.png" alt="550" width="550">
Then, create an instance of this using Thread:
<img src="java-thread4.png" alt="550" width="550">

### Thread states
A thread that is alive is always in one of three states:
1. running: it is currently executing;
2. runnable: it is currently not executing but is ready to execute; or
3. non-runnable: it is not running and is not ready to run—may be waiting on some input or shared data to become unlocked.
<img src="thread-states.png" alt="550" width="550">

### Java thread primitives
1. Calling **start()** causes the Java virtual machine to execute the **run()** method in a dedicated thread, concurrent with the calling code
2. A thread stops executing when **run()** finishes
3. A thread can be suspended for a specified amount of time using **sleep(long milliseconds)**
4. We can test whether a thread is running using the **isAlive( )** method
5. The method **yield()** causes the current thread to pause, going from “running” status to “runnable”
6. Calling **t.join()** suspends the caller until thread t has completed (In this sense the two join together.)

### Additional suspension states
To account for Java’s concurrency primitives fully, we need to
consider additional states that a thread can be in:

1. Having called sleep();
2. having called join();
3. waiting for a lock to be released

A thread can be interrupted through Thread.interrupt().
If interrupted in one of the three states above, it will return to “runnable” state, and **sleep()**, **join()**, or **wait()** will throw an InterruptedException

### Deadlock
Deadlock is a situation where a set of processes are unable to make any further progress, because of mutually incompatible demands they make of shared resources

Concurrent languages provide means for mutual exclusion and for processes to wait for required resources. They also use a principle of no preemption, that is, a process has to give up resources voluntarily

<img src="dead-lock.png" alt="550" width="550">

## 3. Semaphores and State Diagrams
- Lecture Con.03 -- Semaphores and State Diagrams
    - March 13, 2019 12:05pm-1:00pm

Semaphores provide a concurrent programming construct on a higher level than machine instructions. Using semaphores, the critical section problem can be solved trivially

### Semaphores
1. A semaphore is a simple but versatile concurrent device for managing access to a shared resource. 
2. It consists of a **value** *v ∈ N* of currently available access permits, and a **wait set** *W* of processes currently waiting for access. 
3. It must be initialized *S := (k, { })*, where **k** is the maximum number of threads can simultaneously access some resource. 
4. *S* comes with two atomic operations, wait and signal

### Semaphore operations
<img src="semaphore-operations.png" alt="550" width="550">

### Semaphores in Java
1. The package java.util.concurrent has a Semaphore class
2. The wait and signal operations are called **acquire()** and **release()**
3. The Semaphore constructor has, apart from the value argument, an optional Boolean argument which, when true, makes the semaphore strong; that is, it gives access to waiting threads on a “first in, first out” basis

### Summary
1. Sempahores are an elegant and efficient construct for solving problems in concurrent programming
2. Semaphorse are widely implemented
3. Liveness properties of semaphores may depend on implementation
4. Monitors make concurrency easier still

## 4. Monitors; Java summary
- Lecture Con.04 -- Monitors in Java
    - March 15, 2019 3:20pm-4:15pm

Semaphores are easier to use than shared protocol variables, but they are still a low-level and unstructured primitive and don’t scale well

For that reason, concurrent programming languages offer higher-level synchronization primitives. In the case of Java:
1. **Synchronized methods/objects**: a method or an object can be declared synchronized, which means only one process can execute or modify it at any one time
2. **Monitors**: a set of synchronized methods and data (an object or module) that queue processes trying to access the data

### Synchronized methods

The synchronized keyword declares a method or object as being
executable or modifiable by only one process at a time. If a method is declared as synchronized, in effect, it marks this method as a critical section

### Monitors
Monitors are language features that help with providing **mutual exclusion to shared data**

In Java, a monitor is an **object** that encapsulates some (private) data, with access to the data only via synchronized methods. However, a monitor is more than just a collection of synchronized methods. It manages the blocking and unblocking of processes that vie for access

All objects in Java have monitors. The Object class in Java, from which all other classes inherit, contains the following three methods relevant to monitors:

1. void wait(): Causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object
2. void notify(): Wakes up a single thread that is waiting on this object’s lock (the choice of thread that is awoken is arbitrary)
3. void notifyAll(): Wakes up all threads that are waiting on this object’s lock

### Implementing Java monitors
For a class to meet the requirements of a monitor:
1. all attributes should be private
2. all methods that access these attributes should be synchronized

This ensures that all methods will be treated as atomic events

## 5. Processes in FSP
- Lecture Con.05 -- Processes in FSP
    - March 20, 2019 12:05pm-1:00pm


## 6. Synchronisation in FSP
- Lecture Con.07 -- Synchronisation in FSP

### Deadlock
A process is in a **deadlock** if it is blocked waiting for a condition that will never become true

### Livelock
A process is in a **livelock** (a busy wait deadlock) if it is spinning while waiting for a condition that will never become true. Either can happen if concurrent processes or threads are mutually waiting for each other

### The Coffman conditions
Coffman, Elphick, and Shoshani identify four necessary and sufficient conditions (the **Coffman conditions**) that all must occur for deadlock to happen:

1. **Serially reusable resources**: the processes involved must share some reusable resources between themselves under mutual exclusion
2. **Incremental acquisition**: processes hold on to resources that have been allocated to them while waiting for additional resources
3. **No preemption**: once a process has acquired a resource, it can only release it voluntarily — it cannot be forced to release it
4. **Wait-for cycle**: a cycle exists in which each process holds a resource which its successor in the cycle is waiting for

## 7. Introduction to Complex System
- Lecture Cx.01 -- Introduction to Complex Systems
    - May 1, 2019 12:05pm-1:00pm

### What are complex system
1. What is a system?
    - Oxford Dictionary: A set of things working together as parts of a mechanism or interconnecting network; a complex whole

2. What is a complex system?
    - Complex systems. . .
        - are made up of a number of components,
        - that interact with each other,
        - typically in a non-linear fashion.
        - may arise from and evolve through self-organization,
        - are neither completely regular nor completely random,
        - permit the development of emergent behaviour at macroscopic scales.

### Properties of complex systems
1. Emergence
    - The system has properties that the individual parts do not
    - These properties cannot be easily inferred or predicted
    - Different properties can emerge from the same parts, depending upon context or arrangement
2. Self-organisation
    - Order increases without external intervention
    - Typically as a result of interactions between parts
3. Decentralisation
    - There is not a single controller or ‘leader’
    - distribution: each part carries a subset of global information
    - bounded knowledge: no part has a full view of the whole
    - parallelism: parts can act simultaneously
4. Feedback
    - Positive feedback amplifies fluctuations in system state
    - Negative feedback dampens fluctuations in system state

### What is a model?
- Oxford Dictionary: A simplified description, especially a mathematical one of a system or process, to assist calculations and predictions.

### Why build models?
- Models allow us to examine system behaviour in ways that are not feasible in the real world:
    1. too expensive
    2. too time-consuming
    3. not ethical
    4. simply not possible
- Models help us to understand a system by building it

### Types of models
1. Mathematical models：
    - macro-equations
        - For some systems, we can describe the global state and behaviour in terms of **macro-equations**
        - For some systems, these macro-equations exist and have an analytical solution (found using calculus), Unfortunately, many systems have no such analytic solution
    - no macro-equations：
    
3. Computational models:
    - In particular **agent-based models** (ABMs), which arose (in the 1960s) to model systems that were too complex for analytical descriptions. The rapid growth of computational power has made ABMs a practical tool
        - system parts: **agents** with local state and rules
        - system structure: **patterns** of local interaction between agents
        - system behaviour: dynamic **rules** for updating agent state on the basis of interactions

### Steps in modelling a complex system
1. define the key questions
2. identify the structure (parts and interactions) of the system
3. define the possible states for each part
4. define how the state of each part changes over time through interactions with other parts
5. verify, validate and evaluate the model in terms of simplicity, correctness and robustness
6. define and run experiments with the model to address key questions

## 8. Dynamical Systems and Chaos
- Lecture Cx.02 -- Dynamical Systems and Chaos
    - May 3, 2019 3:20pm-4:15pm

### What is a dynamical system?
- a set of possible states (the state space)
- time t (which we may treat as discrete or continuous)
- a rule that determines the state at the present time (xt ) in terms of states at earlier times (xt−1, xt−2, . . .)
- For now, we will require this rule to be deterministic, so that the history of the system (it’s past state) uniquely determines the present state.
- initial condition x0 : the state that the system is in when t = 0

### Functions and iteration
- **Function**: takes a number as an input, does something to it, and produces a number as an output; eg,
    - f (x) = 3x
- **Iteration** is simply using the output of the previous application of a function as the input for the next application:
    - x<sub>0</sub>, f (x<sub>0</sub>), f (f (x<sub>0</sub>)), f (f (f (x<sub>0</sub>))), . . .
    - x<sub>0</sub>, x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub>, . . . , x<sub>t</sub>

### Red foxes in Australia

#### Assumptions:
1. the initial population contained 2 female red foxes
2. a female red fox reproduces in its first year of life
3. a female red fox only reproduces once in it’s life
4. half of newborn kits are female

#### Formula
- The number of female red foxes will double each year
    - x<sub>t+1</sub> = 2x<sub>t</sub>
    - where xt is the number of female red foxes alive in year t
- The general formula for x<sub>t</sub>:
    - x<sub>t</sub> = x<sub>0</sub>2<sup>t</sup>
- The **exponential model** is often stated as x<sub>t</sub> = x<sub>0</sub>r<sup>t</sup> , where r is a **parameter** that governs how steeply the curve rises

#### Refining our model
5. when there are few red foxes, there will be plenty of food, and their numbers will grow rapidly
6. when there are many red foxes, there won’t be enough food to go around, and their numbers will grow more slowly

#### The logistic map
The logistic map (based on a the continuous equation introduced in 1838 by Pierre François Verhulst) displays a diverse range of behaviours, depending on the value of the parameter r
- The logistic map of model
    - x<sub>t+1</sub> = rx<sub>t</sub>(1-x<sub>t</sub>)
    - rx<sub>t</sub> corresponds to positive feedback
    - x<sub>t+1</sub> corresponds to negative feedback

### Chaotic System

#### Definitions
1. The dynamical update rule is deterministic
2. The system behaviour is aperiodic
3. The system behaviour is bounded
4. The system displays sensitivity to initial conditions

### Summary
1. To model a dynamical system, we define:
    - our system variables and the states they can take
    - the function that computes the next state
2. This will usually involve simplifying assumptions about the real world
    - important to record these
3. When exploring the behaviour of our model, we need to evaluate it against what we know about the real system
    - This may result in us refining our model
4. Dynamical systems have several different types of characteristic behaviours
    - fixed points
    - limit cycles
    - aperiodic orbits
5. The dynamic behaviour that a system exhibits can depend critically on the values of system parameters

## 9. ODE Models I: Predator-Prey

### Model formulation
- Prey (rabbits): 
    - <sup>dR</sup> &frasl; <sub>dt</sub> = αR − βRF
- Predators (red foxes): 
    - <sup>dF</sup> &frasl; <sub>dt</sub> = δRF − γF
- Parameters:
    - α: growth rate of the rabbit population
    - β: rate at which foxes predate upon (eat) rabbits
    - γ: growth rate of the fox population
    - δ: decay rate of the fox population due to death and migration

## 10. ODE Models II: Epidemics

### The basic SIR model
The Susceptible, Infectious, Recovered (SIR) model is one of the simplest disease models, proposed almost 100 years ago and still the basis of ID modelling today.

The SIR model sorts people into three categories based on their disease state:
- Susceptible – can be infected
- Infectious – can infect others
- Recovered – cannot be infected nor infect others

If we think about a single person in our population, there are two possible events that can occur to them:
1. If they are susceptible, and they encounter an infectious person, they may be infected
2. If they are infectious, they may recover

## Cellular Automata I: 1D and 2D CAs
Cellular Automata (CA) are a type of discrete dynamical system
that exhibit complex behaviour based on simple, local rules.
- **automaton**: a theoretical machine that updates its internal state based on external inputs and its own previous state
- **cellular automaton**: an array of automata (cells), where the inputs to one cell are the current states of nearby cells

### Cellular Automata
In a CA:
- time is discrete (like the logistic map)
- space is discrete: typically a 1D or 2D grid (3D also possible)
- system state is discrete: each component of a system is in one of a finite set of states (eg, ON or OFF); therefore the entire system has a finite, countable number of states
- update rules are defined in terms of local interactions between components

## Agent-Based Models
Like cellular automata (CA), agent-based models (ABMs) are an approach to modelling complex systems that focuses on the components of the system and the interactions between them.

An ABM typically has three elements:
1. agents
2. environment (including other agents)
3. interactions

ABMs differ from CA in that allow more flexibility in how they
represent agent behaviour and interaction structure.

### The agent (boid)

Agent behaviours include:

- forward motion
- turning left or right
- (perhaps) acceleration and deceleration 

#### Characteristics of agents
1. self-contained
    - An agent is a modular component of a system, is has a boundary and can be clearly distinguished from and recognised by other agents.
    - Example: Flocking model: the boids are clearly distinguishable and ‘recognised’ (though not uniquely) by other boids.
2. autonomous
    - An agent can function independently in its environment, and in its interactions with other agents (within the scope of the defined model).
    - Example: Flocking model: the behaviour of each boid is entirely defined by the information it obtains about its local environment
3. dynamic state
    - An agent has attributes, or variables, that can change over time. An agents current state is what determines its future actions and behaviour.
    - Example: Flocking model: a boid’s state consists of its current heading and speed. This state determines the motion of the boid, and is modified on the basis of information it recieves about its local environment
4. social
    - An agent has dynamic interactions with other agents that influence their behaviour.
    - Example: Flocking model: boids ‘interact’ by perceiving and reacting to the location and behaviour of other boid
5. adaptive
    - An agent may have the ability to learn and adapt its behaviour on the basis of its past experiences.
    - Example: imagine if prey boids in predator-prey model observed that predators more often attacked boids on their right than their left (becuase of the location of their sensors/eyes), they might be able to ‘learn’ to move to the left of a predator and hence escape
6. goal-directed
    - An agent may have goals that it is attempting to achieve via its behaviours.
    - Example: imagine that, in addition to avoiding predators, prey boids also have the goal of collecting materials to build a nest. . . This would be another factor influencing their behaviour.
7. heterogeneous
    - A system may be comprised of agents of different types: these differences may be by design (eg, predators and prey), or a result of an agents’ past history (eg, the ‘energy level’ of predators, based upon whether it has eaten recently)

### Environment

Environments may be static (unchanging over time), or they
may change as a result of agent behaviour, or they may be
independently dynamic

Real environments are typically dynamic (and often stochastic). Therefore we may not be able to foresee all possible ‘states’of the environment and how agents will respond to them.

### Interactions

A defining characteristic of complex systems is local
interactions between agents, as defined by the agent
neighbourhood.

Depending on the structure of the system, the composition of
an agent’s neighbourhood may be dynamic (ie, change over
time as it moves through its environment)

### Summary
1. Agent-based models consist of agents, an environment, and interactions between agents and (a) other agents and (b) the environment.
2. ABMs feature decentralised information and decision making, and can reproduce the emergent behaviour and self-organisation found in complex systems.
3. ABMs are employed in a wide variety of domains: ecology, social science, economics, political science, etc.
4. ABMs can be much more elaborate than CAs.

### Networks

#### Why study networks?
1. More and more data on patterns of interaction is becoming available.
2. Network science offers tools to help make sense of this data.
3. We can learn something about the functional properties ofthese systems by studying their structural properties.
4. Insights developed in one domain may generalise to other domains.

#### What is a network?

A network consists of:
1. A set of nodes or vertices
2. A set of edges or links
3. Data about nodes and edges

Formally, a graph G = (N, E) is an ordered pair of finite sets. Elements of N are called nodes or vertices. Elements of E ⊂ N × N are called edges or links.

#### Network properties

1. The structural properties of networks often have functional consequences. Structural properties are often used to compare and contrast network models of different systems.
2. Networks may be unipartite (all nodes of the same type) or bipartite (nodes of two different types), or k-partite (nodes of k different types).
    - **bipartite networks**: directors and the companies whose boards they site on; actors and the movies they have appeared in
    - **directed networks**: web pages and the (directed) links between them; food webs with directed edges indicating predation
3. Edges may be directed or undirected.

##### Assumptions
1. We will consider properties of networks that are unipartite, with undirected edges
2. No parallel edges (ie, there can only be one edge between two nodes)
3. No self-connections (ie, there are no edges between a vertex and itself)

##### Density
- The density D of a network is the ratio of the number of edges in the network to the number of possible edges.

- A network with N nodes and E edges could have up to <sup>N(N−1)</sup> &frasl; <sub>2</sub> possible edges, therefore: D = <sup>2E</sup> &frasl; <sub>N(N−1)</sub>

##### Degree
- The degree k<sub>i</sub> of node i is the number of edges between node i and other nodes in the network. Nodes with high degree are often considered to be more “important”.

- The average degree < k > of a network, where < k >= 2E/N.

- The degree distribution P(k) of a network is the probability distribution of degrees over all nodes in the network; P(k) = N<sub>k</sub>/N where N<sub>k</sub> is the number of nodes of degree k.

##### Distance
- The distance d<sub>ij</sub> between nodes i and j is the length of the shortest path between those two nodes.

- The characteristic path length L is the average distance (ie, shortest path) between all pairs of nodes in the network

- The diameter of a network is the longest shortest path between any two nodes; ie, it is the largest number of nodes that must be traversed to move from one node to another without backtracking

##### Clustering
- The clustering coefficient C is a measure of “cliquishness” in a network; ie, the extent to which, if Alice is friends with Bob and Carol, Bob and Carol are likely to be friends with each other.

- Another way of thinking about the clustering coefficient of a node i is in terms of the proportion of possible triangles containing i that exist.

##### Centrality
Centrality is another approach to measuring which are the most
important nodes or edges in a network. There are many different definitions including:
1. the number of shortest paths that pass through a particular node or edge
2. the extent to which a node is connected to other “important” nodes

##### Assortativity 
Assortativity measures the extent to which similar nodes tend to be connected to each other. With respect to degree, a highly assortative network is one in which the high degree nodes are all connected to each other, while a dissasortative network is one in which high degree nodes tend to be connected to low degree nodes.

##### Summary
- A broad variety of systems can be described in terms of their network structure.
- Doing so allows us to quantify and analyse system structure in terms of relatively simple properties.
- These properties can have functional consequences, that may generalise across different systems.
