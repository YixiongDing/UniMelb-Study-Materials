# Software Design and Architecture Sample Exam

Content is wriiten by Yixiong Ding  
The University of Melbourne  
November, 2019   

- - -
## Part A – Short-answer questions
- - -

### Question 1 [4 Marks] List and describe each of the ACID properties of transactions.

1. **Atomicity**: The transaction must complete in its entirety, or roll back to the start state; the “all or nothing” property. 

    Atomicity（原子性）：一个事务（transaction）中的所有操作，或者全部完成，或者全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。即，事务不可分割、不可约简。

2. **Consistency**: All associated resources must be in a consistent, non-corrupt state at the start and end of the transaction.

    Consistency（一致性）：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设约束、触发器、级联回滚等。

3. **Isolation**: Any effect that a transaction has on resources must only be visible to other transactions after completion; that is, changes must not be visible during execution.
    
    Isolation（隔离性）：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括未提交读（Read uncommitted）、提交读（read committed）、可重复读（repeatable read）和串行化（Serializable）。

4. **Durability**: The results of the transaction must be permanent; that is, it must survive system shutdowns and crashes.

    Durability（持久性）：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。


### Question 2 [4 Marks] The transaction script pattern and the domain model pattern are both patterns used to organise domain layers. Describe two design and/or implementation differences between these two patterns. That is, do not describe the pros and cons of the patterns, but ways in which they differ in the structure of the design or implementation.

#### Transaction script

##### Design

The transaction script pattern is a simple pattern that organises the domain layer such that each transaction is organised into a single procedure (or script), with common behaviour factored into shared sub-procedures.

##### Implementation

There is a single method/procedure for each transcript. While this method/procedure can use other methods/procedures to execute the transaction, each transaction is served by exactly one script, and each script serves exactly one transaction

#### Domain model

##### Design

The domain model pattern involves constructing a model of the business domain of the system, usually an object-oriented model, and then coupling the behaviour to the objects in the domain using methods.

##### Implementation

Two approaches:

1. Small applications: simply load the object graph from a file and place it into memory, generally running on a single machine

2. Alternatively: store the information required in a database, and then only create the objects as required

### Question 3 [3 Marks] The first law of distributed object design is: Do not distribute objects. Using an example, state why you either agree or disagree with this law

I agree. 

The reasons:

1. For high cohesion. objects hold only a small amount of related data types

2. Their interfaces should allow fine-grained access to this data. However, such an approach results in many small calls

3. Querying an object over a network (remote method calling) is extremely costly in compared to local method calling

4. When distributing objects, remote calls made between processes are an order of magnitude slower than local method calls. Remote calls made to another machine are an order of magnitude slower than remote calls on the same machine

### Question 4 [3 Marks] What is a long transaction? Briefly explain two methods that can be used to mitigate concurrency issues in long transactions.

Long transaction: a transaction that spans two or more requests to the system

Methods:

1. Optimistic offline lock An optimistic lock is a late transaction pattern that validates the changes about to be committed in one session do not conflict with changes in another

2. Pessimistic offline lock: A pessimistic lock is a long transaction pattern that aims to prevent this by locking the required data over the entire business transaction. Thus, it prevents the “end of transaction” failures by avoiding conflicts all together

### Question 5 [3 Marks] Compare and contrast the model-view-controller and the model-view-presenter design patterns.

Both MVC and MVP patterns abstract the logic related to presentation to three modules: 

1. Model: Model represents the information about the domain
2. View: View deals with the display of information to the client through a user interface
3. Presenter/Controller: Handles the interactions between the view and the model

The main difference between the two patterns lie in the interactions of the view and the model. In the MVC pattern the view directly queries the model while in the MVP pattern the view is managed by the presenter.

In the context of the layered architecture, the view and the presenter/controller are on the layer above the model layer. Both these patterns provide a clear separation between the model layer and the upper layer (view-presenter/controller) layer, however, less separation is provided between the view and the controller/presenter.


### Question 6 [3 Marks] Name and briefly describe three design patterns for enterprise integration.

1. File transfer

    Uses files to transfer data/information between multiple applications

2. Shared Database

    Uses a shared database to transfer data/information between multiple applications

3. Messaging
    
    Uses messages to transfer data/information between multiple applications