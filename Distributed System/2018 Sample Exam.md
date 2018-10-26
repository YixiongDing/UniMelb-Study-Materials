# COMP90015 Distributed System
## Sample Exam
### Semester 2, 2018
_ _ _
Wriiten by Yixiong Ding  
The University of Melbourne  
October, 2018  
_ _ _

#### Question 1:
##### A) Identify various types of resources that can usefully be shared in computer networks. Give examples of their sharing as it occurs in distributed systems. [5]

In computing, a shared resource, or network share, is a computer resource made available from one host to other hosts on a computer network. It is a device or piece of information on a computer that can be remotely accessed from another computer, typically via a local area network or an enterprise intranet, transparently as if it were a resource in the local machine.

Some examples of shareable resources are computer programs, data, storage devices, and printers. E.g. shared file access (also known as disk sharing and folder sharing), shared printer access, shared scanner access, etc. The shared resource is called a shared disk, shared folder or shared document

##### B) Discuss briefly key challenges that one needs to address in the design and development of distributed applications. [5] 

Challenges:

1. Heterogeneity: Heterogeneous components must be able to interoperate

2. Distribution transparency: Distribution should be hidden from the user as much as possible

3. Fault tolerance: Failure of a component (partial failure) should not result in failure of the whole system

4. Scalability: System should work efficiently with an increasing number of users

5. Concurrency: Shared access to resources must be possible

6. Openness: Interfaces should be publicly available to ease inclusion of new components

7. Security: The system should only be used in the way intended

#### Question 2:
#####  A) Discuss peer-to-peer architectural model for construction of distributed systems. [5]

Each process in the systems plays a similar role interacting cooperatively as peers (playing the roles of client and server simultaneously).

Peer-to-Peer allows all peer processes to connect to each other. The failure of any given peer is no worse than other peer. Peer-to-Peer provides scalability but it leads to IP addresses being revealed which is a security concern.

##### B) Identify main types of security threats that might occur in the Internet with an example. [5]

Security threats fall into three broad classes:

1. Leakage -- the acquisition of information by unauthorized recipients;
    - Eavesdropping – obtaining private or secret information or copies of messages without authority.

2. Tampering -- the unauthorized alteration of information;
    - Message tampering - altering the content of messages in transit

3. Vandalism -- interference with the proper operation of a system without gain to the perpetrator.
    - Denial of service - flooding a channel or other resource, denying access to others
    
#### Question 3:
##### A) Discuss difference between TCP/IP and UDP protocols for Socket-based communication. [5]
TCP: 
1. **Connection**: TCP is a connection-oriented protocol, as a message makes its way across the internet from one computer to another. This is connection based.

2. **Ordering of data packets**: TCP rearranges data packets in the order specified.

3. **Speed of transfer**: The speed for TCP is slower than UDP.

4. **Reliability**: There is absolute guarantee that the data transferred remains intact and arrives in the same order in which it was sent.

5. **Usage**: TCP is suited for applications that require high reliability, and transmission time is relatively less critical.

6. **Handshake**: SYN, SYN-ACK, ACK

UDP: 
1. **Connection**: UDP is a connectionless protocol, UDP is also a protocol used in message transport or transfer. This is not connection based which means that one program can send a load of packets to another and that would be the end of the relationship.

2. **Ordering of data packets**: UDP has no inherent order as all packets are independent of each other. If ordering is required, it has to be managed by the application layer.

3. **Speed of transfer**: UDP is faster because error recovery is not attempted. It is a "best effort" protocol.

4. **Reliability**: There is no guarantee that the messages or packets sent would reach at all.

5. **Usage**: UDP is suitable for applications that need fast, efficient transmission, such as games. UDP's stateless nature is also useful for servers that answer small queries from huge numbers of clients.

6. **Handshake**: No handshake (connectionless protocol)

##### B) Write a multithreaded Java program that responds to remote clients’ requests for meaning of words stored in a Dictionary. If a client program sends a message “King” to the server, the server program responds back with the meaning of word “King” by retrieving it from the dictionary (as a string). Use Java Sockets for communication between clients and the server. [5]

