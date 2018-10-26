# COMP90015 Distributed System
## Final Exam
### Semester 1, 2017
_ _ _
Wriiten by Yixiong Ding  
The University of Melbourne  
October, 2018  
_ _ _

#### Q.1. (a) [5 marks] Consider a non-distributed application that allows a user to conduct a long-running simulation, with the ability to monitor and control the simulation using a graphical user (GUI) interface. The GUI interface displays a large number of simulation outputs in real-time. Now consider the same application that has been split into two processes: a server process that runs the simulation, and a GUI process that allows user monitoring and control. This is now a distributed application. Give three reasons for and two reasons against the use of a distributed application with respect to this example. Make sure your reasons are all distinctly different.

#### Answer
**This can be considered as the pros of distributed system**

For:
- The simulation can be accessed remotely.
- Multiple users can access the same simulation at the same time.
- The GUI can be recompiled without having to stop the simulation.

**This can be considered as the cons of distributed system**

Against:
- Bandwidth between GUI and simulation can be a limiting factor.
- Application becomes more complicated to build.

#### (b) [5 marks] Explain what is meant by eventual consistency in a distributed system. You may give an example application to aid your answer. Give your reasons for and against adopting eventual consistency in a distributed system.

Eventual consistency in a distributed systems means that, assuming there are no changes to data/state, the state of the system is eventually seen as the same no matter from where data is accessed. For example, a change to some data in a distributed database may take some time to propogate to all nodes. Before it has done so, some reads at some nodes may be inconsistent. But evetually the system will become consistent. It is good for providing large scale systems because it tolerates latency and faults. However some applications may require absolute consistency such as banking applications. Eventual consistency as well needs further definition as to what kinds of inconsistent states that nodes can "see".