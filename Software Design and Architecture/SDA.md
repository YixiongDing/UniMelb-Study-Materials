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
