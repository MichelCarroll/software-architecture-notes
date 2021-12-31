
# Designing Data-Intensive Applications - Book Summary

## Chapter 1 - Reliable, Scalable, Maintainable Applications

### Glossary

**Data-intensive:**
Problem has data with large volume, complexity or speed of change

**Compute-intensive:**
Problem has amount of computation required as a limiting factor

**Data system:**
System composed of one or many computer systems designed to handle data (eg. databases, message queues, caches, indexes)

**Non-functional requirements:**
Application requirements which don't directly relate to its software specifications (eg. maintainability, security, scalability)

**Reliability:** 
Increases likelihood that software behaves as expected in a timely manner, even in the presence of faults. Resistant to misuse and abuse.

**Fault tolerant:**
Decreased likelihood of system failure in the event of faults (eg. hardware, software, and human faults). Does *not* mean a decrease of faults themselves.

**Telemetry:**
Setting up detailed monitoring, performance metrics and error reporting.

**Scalability:**
Ability for system to stay reliable despite increased load.

**Load parameter:**
Quantitative description of load (eg. requests per second, query size)

**Throughput:**
Number of requests that can be fulfilled per time unit. Useful in batch processing.

**Response time:**
Time between sending a request and receiving a response. Counts both processing time and network latency. Useful metric in online systems.

**Latency:**
Time that a request is *waiting* to be processed. Does not count processing time.

**Fan-out:**
In order to fulfill one request, many other requests need to be fulfilled (eg. updating home timelines in Twitter). Can lead to exponential increase in number of requests as a result of load.

**Tail latency:**
The 99th or 99.9th percentile response time.

**Head-of-line blocking:**
Slowing down of response time due to earlier slow requests in the queue, since the system has a limited level of parralelism.

**Tail latency amplification:**
High latency in one service causing latency in many other requests due to multiple backend calls.

**Service-level agreement (SLA):**
Level of availability guaranteed by a software system. Usually in terms of uptime, defined by a median response time or tail latency.

**Scaling up:**
Increasing individual server memory and CPU capacities. Also known as vertical scaling.

**Scaling out:**
Increasing number of servers in system. Also known as horizontal scaling.

**Elasticity:**
System which can automatically adjust capacity in event of changing load.

**Maintainability:**
Decreases post-development costs of software. Includes evolvability, simplicity, operability.

**Operability:**
Ease of keeping the system running smoothly by making routine tasks faster and easier to execute. Includes monitorability.

**Evolvability:**
Ease of modifying and adding functionality as requirements change without introducing regressions. Also known as extensibility, plasticity, modifiability.

**Configurability:**
Ability to change behaviour or system without requiring a change and deployment of its source code.

**Simplicity:**
Ease of understanding how a system works.

**Accidental complexity:**
System complexity not due to the problem domain. Eleviated by abstraction.

**Inherent complexity:**
System complexity due to the problem domain, and cannot be easily simplified.

### Key Points

- No data system is the same, but they can often be composed by similar building blocks. Wide-ranging systems characteristics can be built using many single-purpose tools.
- Increasing number of faults (intelligently) can make a system more fault-tolerant. Think of Netflix'x Simian Army.
- Hardware faults happen all the time. Increasing the number of servers in a system makes it more likely for individual faults to occur. AWS prioritizes elasticity over single-server reliability.
- It's hard to generalize software errors, so the best we can do is simplify systems and make them easier to troubleshoot.
- A majority of outages are caused by manual configuration errors.
- Scalability is best discussed using very specific load parameters and scenarios. There's no such thing as a one-size-fits-all scalable infrastructure.
- When a system reaches a new order of magnitude amount of load, it's wise to reconsider its scalability
- It can be more important to optimize tail latency rather than median latency, since those might represent the most important users


## Chapter 2 - Data Models and Query Languages

### Glossary

**Data representation:**
How data modeled at a certain level of abstraction is represented at a lower level of abstraction.

**Relational model:**
Model in which data is organized as unordered tuples (rows) inside different relations (tables). Uses foreign keys to represent relationships.

**NoSQL:**
Catch-all term for open-source, distributed and nonrelational databases.

**Polyglot persistance:**
The use of multiple different types of databases in one data system.

**Object-relational mismatch:**
The disconnect between data organized in the relational model versus the object model. Also known as impedence mismatch. Solved by object relational mapping frameworks (eg. ActiveRecord).

**Storage locality:**
Property of data often used together being physically close together in volatile or non-volatile storage.

**Normalization:**
Eliminating duplication and improving consistancy in databases by the use of one-to-many relationships.

**Document model:**
Model in which data is inside self-contained documents organized in a tree structure. Typically JSON. Uses document references to reference other documents.

**Hierarchical model:**
Older and mostly legacy model in which data is organized as a tree of records nested inside of other records. Progenitor of the relational and the network model.

**Network model:**
Also known as the CODASYL model. Generalization of the hierarchical model where records can have more than one parent. Reading a record is only possible through defining and imperative access path.

**Data shredding:**
Splitting document-like data structure into multiple relations.

**Clustered index:**
Database index which defines the order in which the tuples are stored on the disk. There can be only one clustered index per relation.

**Schema-on-read:**
Property of document model where the data schema is not explicitly enforced by the database. Rather, it's represented by the application logic.

**MapReduce:**
Querying document databases by imperatively defining mapping and folding operations.

**Aggregation pipeline:**
Declarative query language used to query document databases.

**Graph model:**
All data is represented as vertices and edges. Useful for data with lots of many-to-many relationships.

**Property graph model:**
Graph database model where each vertex and edge can have many key-value properties.

**Cypher query language:**
Used to query property graph model databases using a declarative syntax.

**Recursive common table expressions:**
SQL feature using `WITH RECURSIVE` to enable querying relational databases in a graph traversal manner.

**Triple-store:**
Graph database model where each tuple is represented as a `(subject, predicate, object)`. Often written in a Turtle format.

**RDF data model:**
Used to write triple-store model data in an XML syntax. Analogous to the Turtle format.

**SPARQL query language:**
Used to query triple-store graph model databases using an SQL-like syntax.

**Datalog:**
Data model subset of Prolog where data is represented as facts, in the form of `predicate(subject, object)`. Queries are specified via rules which are also predicates derived from other data.

### Key Points

- Data models affect how we think about the problem, as well as how the software is written
- Clean data abstractions enable different teams to effectively work together by hiding complexity 
- Data models embody assumptions about how the data is going to be used 
- NoSQL is an unfortunate name for non-relational model databases, and should be reinterpreted as "not only SQL"
- Document database support for joins is weak, often requiring multiple queries for one-to-many relationships.
- Data tends to be come more interconnected as features are added to applications, which may cause a document modeled database to become cumbersome if it's used in early prototyping.
- The relational model uses the query optimizer to play the role of a developer manually defining an access path through the data
- Main arguments for using a document database are schema flexibility, better performance due to localality, and helping with impedence mismatch. 
- Main argument for using a relational database is better support for joins, one-to-many and many-to-many relationships.
- For highly interconnected data (many-to-many relationships), a graph database is often the most natural
- Newer document-based and relational databases blend features from both types, so choosing a database can come down to looking at the specific features they accomodate.
- Declarative query languages can take advantage of parralelism and other optimizations under the hood
- Graph databases are good for evolvability, with the addition of new features being relatively painless
- While graph traversal is possible using recursive common table expressions in SQL, it's often very verbose and hard to understand
- NoSQL has diverged into two directions: Document-based where each entity is self-contained, and graph-based where each entity is highly interconnected