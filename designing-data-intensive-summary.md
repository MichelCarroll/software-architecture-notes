
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


## Chapter 3 - Storage and Retrieva

### Glossary

**Log:**
Append-only sequence of records.

**Index:**
Data structure derived from the primary data with the goal of improving the performance of operations on it.

**Hash Index:**
Index implemented using a hash map.

**Compaction:**
Removing redundant or obsolete data segments and keeping the relevant ones. In the case of logs, removing redundant records and keeping the most recent one. 

**Tombstone:**
Entry with the sole purpose of indicating a deleted record.

**SSTables:**
Sorted String Table. Log of key-value pairs sorted by key.

**Memtable:**
In-memory tree data structure used to sort incoming log writes, and periodically merge them into the on-disk SSTable.

**LSM-Trees:**
Log-structured merge trees. The algorithm involving the use of a memtable and an  SSTable.

**Term dictionary:**
Key value structure where the key is a term, and the values are its postings list; a reference to each document containing that term.

**Bloom filters:**
Data structure used to quickly compute whether a given key is NOT inside a data structure. 

**Size-tiered compaction:**
Segments of similar size are compacted and merged into larger segments when there are a certain number of them.

**Level-tiered compaction:**
Segments are organized into levels and progressively merged into subsequent levels.

**Embeddable database:**
Database engine meant to be used inside an application, as opposed to run as a separate server process.

**B-tree:**
Database are broken up into pages, and indexes are organized as a tree of pages. Uses update-in-place philosophy, rather than an immutable append-only structure.

**Branching factor:**
Number of children per B-tree node, determined by the size of a page reference.

**Write-ahead log (WAL):**
Write operations to a B tree are appended to log to restore it to a consistant state in the event of a crash.

**Latches:**
Analogous to an OS mutex lock. Used by B trees to enable concurrent access to it.

**Copy-on-write schema:**
Writes to B-tree are written to an alternative location, and then the reference is updated once the new location is consistant.

**Write amplification:**
Effect of one write to a database causing multiple disk writes. Problematic when the performance bottleneck is the write speed.

**Heap file:**
Files where data is stored in no particular order. Used by page-oriented databases to store row data.

**Clustered index:**
Index structure where the entire row is stored inside the index. For example, InnoDB uses a clustered index for primary keys.

**Covering index:**
A subtype of clustered index where only a subset of the row's columns are stored in the index.

**Concatenated index:**
Index key made up of a tuple of different column values. For example, the phone book has one using the first name and last name.

**Space filling curve:**
Mathematical function for encoding a multi-dimensional value using one dimension.

**R-trees:**
A specialized index for storing spatial data. PostGIS uses R-trees.

**Levenstein automaton:**
A finite state automaton for storing a fuzzy index of words.

**Anti-caching:**
Used by in-memory databases to extend the size of the memory available by storing LRU entries into disk, similar to the OS virtual memory.

**Transaction processing:**
Many small low-latency reads and writes.

**Batch processing:**
A few large high volume operations run periodically.

**Online transaction processing (OLTP):**
Access pattern for databases doing transaction processing for interactive applications.

**Online analytic processing (OLAP):**
Access pattern for databases enabling batch processing and analytical queries for data analytics and intelligence.

**Data warehouse:**
Database used for running OLAP workloads without affecting the performance of OLTP databases in the business. Data is often extracted from OLTP sources using ETL.

**Star schema:**
Also known as dimensional modeling. The use of fact tables referencing dimension tables for organizing data warehouses for maximum flexibility.

**Fact table:**
Contains facts representing events in a data warehouse.

**Dimension table:**
Contains dimensions representing the what,when,who,where,why,how of a event in a data warehouse.

**Snowflake schema:**
Generalization of the star schema where dimension tables can have relationships with other dimension tables.

**Row-oriented storage:**
Scheme for storing data where each row is stored in one place.

**Column-oriented storage:**
Scheme for storing data where each column is stored in one place.

**Bitmap encoding:**
A column compression schema where each column value is stored as a bitmap. Useful for columns with a small cardinality.

**Run-length encoding:**
A column compression scheme for the number of zeroes and ones are stored as a digit. Useful for sparse data.

**Vectorized processing:**
Taking advantage of SIMD (single instruction, multi data) instructions in modern CPUs to quickly compute operations from compressed columns, and leverage L1 cache.

**Materialized aggregate:**
Used to improve performance on large aggregate queries by caching their results.

**Materialized view:**
Implementation of materialized aggregates in an SQL database by defining a query that acts as a virtual view of the data, while keeping the results to disk for improved performance.

**Data cube:**
A type of materialized view used in OLAP databases for storing a grid of aggregates grouped by different dimensions.

### Key Points

- Keeping indexes comes with a trade-off between read speed and write speed
- Logs are a great way to increase write speed because disk sequential access is always faster than random access
- Compaction and merging can be used to keep a list of keys in sorted order
- Using a binary format eliminates the need for escaping
- Checksums can be used to protect against partially written data in the event of a crash
- Concurrency and crash recovery is simplyer for immutable and append-only data
- Fragmentation can be helped by periodically merging old segments
- LSM-tree algorithm can be slow for searching keys that don't exist, but this can be eleviated using Bloom filters
- A relatively small branching factor can accomodate an incredibly large index while still keeping a small depth (4KB pages + branching factor of 500 = 256 TB index)
- A WAL can be used to make database more resilient to crashes
- Generally, LSM-trees have faster writes while B trees have faster reads 
- LSM-trees can have less consistant performance than B trees due to periodic compaction operations
- LSM-tress have less write amplification problem, especially on magnetic hard drives
- LSM-trees can be compacted more easily and have less fragmentation
- If misconfigured, compaction might not keep up with new writes. This needs to be monitored closely.
- The perfomance of in-memory databases comes primarily because it doesn't need to encode/decode data into/from a disk-friendly format
- Non-volatile memory is becoming a key driver for database technologies
- Despite being more normalized and flexible, snowflake schemas are harder to work with for data analysts than star schemas
- Dates in data warehouses tend to be stored in dimension tables to account for differences in work days, holidays, etc
- The principle behind column-based storage is that anaylytic queries normally don't use all the values in a row
- Column-based storage can be more easily take advantage of column compression and vectorized processing
- Data warehouses sometimes keep data sorted in multiple orders to improve query performance
- LSM-trees can be used by data warehouses to improve write performance during batch operations
- While an in-depth understanding of database internals is not always necessary, more knowledge can help understanding and tuning database parameters


## Chapter 4 - Encoding and Evolution

### Glossary

**Backwards compatibility:**
Newer code can read data written by old code

**Forwards compatibility:**
Older code can read data written by new code

**Serialization:**
Also known as marshalling. Encoding in-memory data to be communicated as a byte sequence.

**Textual encoding:**
More-or-less human readable encodings. For example: JSON, XML, CSV

**Binary encoding:**
Efficient and compact non-human readable encodings. For example: BSON, WBXML

**MessagePack:**
A binary encoding with the same principle as JSON but slightly more space efficient.

**Apache Thrift:**
A binary encoding requiring a schema defined in the Thrift IDL. Has both a BinaryProtocol and CompactProtocol taking up less space. Uses field tags.

**IDL:**
Interface definition language, for defining data schemas.

**Protocol Buffer (protobuf):**
A binary encoding requiring a schema. Uses field tags.

**Field tags:**
Numbers that appear in both the encoded data and the schema definition, for specifying the name and type of each field.

**Avro:**
A binary encoding that uses a schema defined in Avro IDL or in an equivalent JSON representation. Matches the writer's schema against the reader's schema, as long as they're compatible, according to Avro's rules. The schema sent separately from the data, often alongside it.

**Object container file:**
Avro encoded data with a writer's schema defined at the beginning of the file. A self-describing format.

**REST:**
HTTP-based communication philosophy between services. Uses OpenAPI (also known as Swagger) to define the format. 

**SOAP:**
XML-based protocol for communicating between services, independent from the HTTP protocol.

**WSDL:**
Used to enable code generation of stubs between services communication using SOAP.

**RPC:**
Remote procedure calls. Network communication in a location transparent manner: attempting to hide to fact that procedures are happening remotely.

**gRPC:**
RPC implementation using Protocol Buffers. Fact that communication is over the network is more transparent. Supports streams.

**Finagle:**
RPC implementation using Thrift. Similar philosophy as gRPC.

**Service discovery:**
Enables services to programmatically find out the IP/port of another service.

**Message broker:**
Buffers messages between services, logically and temporaly decoupling them. Also enables fan-out. For example: RabbitMQ, Apache Kafka

**Actor model:**
Programming model for concurrency within a single process. Abstraction layer over threads.

**Distributed actor model:**
Using actor model to communicate between different actors in a location transparent manner using implicit message brokers and serialization. For example: Akka, Erland OTP.

### Key Points

- Forward compatibility is often harder than backwards compatibility, since new code can explicitly handle old data
- With client-side applications, you're often at the mercy of users not updating software, so forward compatibility is important
- Language specific encodings (Java's Serializable, Python's pickle) is often a poor choice, since it couples you to the language, and makes backwards/forward compatibility difficult  
- Lack of explicit typing in textual encodings can be problematic without a schema
- Representing large numbers or high precision in JSON can cause encoding issues. Using strings instead of numbers can eleviate that limitation
- Binary encodings are more compact, but there's a trade-off with readability 
- Thrift and Protobuf often uses hand-written schemas, so the programmer can be more explicit about compatibility
- Avro's field tagless model makes dynamically generated schemas and code generation simpler
- Code generation is the most useful for statically typed languages
- Many database drivers use a proprietary encoding for requests and responses
- A schema is a great source of documentation since it doesn't diverge from reality as easily
- Keeping a database of schemas can be useful for ensuring forward and backwards compatibility before deployment
- Backwards compatiblity is necessary when storing data in a databass. Forward compatibility is important for distributed services
- Avro's evolution rules can be used to encode databases. LinkedIn's Espresso does this.
- Avro is a good fit for archival data, so it's self-describing
- Parquet is a more column-oriented storage format similar to the row-oriented Avro.
- SOAP relies heavily on tooling, IDEs and code generation. It's a very poor choice for external facing APIs. 
- RPC is fundamentally flawed, since it hides complexities linked to distributed software. For example, timeouts, latency, idempotency, retry logic, serialization.
- Using futures to encapsulate network calls is an improvement over traditional RPC
- In case of transient dataflow through a system, backwards compatibility of requessts and forward compatibility of responses is sufficient
- Versioning can be used to insure long-term compatibility of clients
- When an encoding is forward and backwards compatible, you have the flexibility to change a message broker's consumers and producers independently
- The actor model is a natural way to model a distributed system, but backwards and forward compatiblity of messages still needs to be considered