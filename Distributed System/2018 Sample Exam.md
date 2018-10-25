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

