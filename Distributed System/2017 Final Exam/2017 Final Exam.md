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

#### Q.2. [5 marks] For each of the following architectural designs, provide a definition and give a reason for and a reason against the use of the design. Draw a diagram to illustrate the architecture.

##### (a) Client/Server
Client/server has many clients processes connecting to a single server process. Clients invoke services in servers and results are returned. Servers in turn can become clients to other services. The clients never connect to each other. It is a simple design and easy to understand/implement. However the server can easily become a bottleneck to scalability.
 <img src="Client-server.png" alt="550" width="550">

##### (b) Peer-to-Peer
Peer-to-Peer allows all peer processes to connect to each other. Each process in the systems plays a similar role interacting cooperatively as peers (playing the roles of client and server simultaneously). The failure of any given peer is no worse than other peer. Peer-to-Peer provides scalability but it leads to IP addresses being revealed which is a security concern.
 <img src="Peer-to-Peer.png" alt="550" width="550">

##### (c) Multi-server
Multi-server is like Client/server however there are multi-servers to which a client may connect. The multi-servers typically communicate with each other or with a data storage tier. Objects may be partitioned (e.g web servers) or replicated across servers (e.g. Sun Network Information Service (NIS)). Multi-server provides for greater scalability than Client/server but it adds latency to the client requests, since multiple servers may need to be involved with a single request. It also is more complex than Client/server.
 <img src="multi-servers.png" alt="550" width="550">

##### (d) Proxy server
A Proxy server architecture has a proxy process, which typically sits between a client and a server. The client's requests to the server are instead sent to the proxy and the proxy forwards them on the server. Responses from the server go to the proxy which forwards them back to the client. The proxy is typically transparent to the client/server protocol. It is good for providing monitoring of client/server requests (e.g. to obtain statistics) but it adds more latency and may become a bottleneck.
 <img src="proxy-server.png" alt="550" width="550">

##### (e) Thin-client
A thin client architecture runs the entire application on the server and uses the client only for user I/O and control. The client does not install any applications. This makes managing the client easy but it means more network bandwidth and latency requirements which may cause some applications to be ineffective (such as video editing).
 <img src="thin_clients.png" alt="550" width="550">