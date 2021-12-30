
# Designing Data-Intensive Applications - Book Summary

## Chapter 1 - Reliable, Scalable, Maintainable Applications

### Glossary

**Data-intensive**
Problem has data with large volume, complexity or speed of change

**Compute-intensive**
Problem has amount of computation required as a limiting factor

**Data system**
System composed of one or many computer systems designed to handle data (eg. databases, message queues, caches, indexes)

**Non-functional requirements**
Application requirements which don't directly relate to its software specifications (eg. maintainability, security, scalability)

**Reliability** 
Increases likelihood that software behaves as expected in a timely manner, even in the presence of faults. Resistant to misuse and abuse.

**Fault tolerant**
Decreased likelihood of system failure in the event of faults (eg. hardware, software, and human faults). Does *not* mean a decrease of faults themselves.

**Telemetry**
Seting up detailed monitoring, performance metrics and error reporting.

**Scalability**
Ability for system to stay reliable despite increased load.

**Load parameter**
Quantitative description of load (eg. requests per second, query size)

**Throughput**
Number of requests that can be fulfilled per time unit. Useful in batch processing.

**Response time**
Time between sending a request and receiving a response. Counts both processing time and network latency. Useful metric in online systems.

**Latency**
Time that a request is *waiting* to be processed. Does not count processing time.

**Fan-out**
In order to fulfill one request, many other requests need to be fulfilled (eg. updating home timelines in Twitter). Can lead to exponential increase in number of requests as a result of load.

**Tail latency**
The 99th or 99.9th percentile response time.

**Head-of-line blocking**
Slowing down of response time due to earlier slow requests in the queue, since the system has a limited level of parralelism.

**Tail latency amplification**
High latency in one service causing latency in many other requests due to multiple backend calls.

**Service-level agreement (SLA)**
Level of availability guaranteed by a software system. Usually in terms of uptime, defined by a median response time or tail latency.

**Scaling up**
Increasing individual server memory and CPU capacities. Also known as vertical scaling.

**Scaling out**
Increasing number of servers in system. Also known as horizontal scaling.

**Elasticity**
System which can automatically adjust capacity in event of changing load.

**Maintainability**
Decreases post-development costs of software. Includes evolvability, simplicity, operability.

**Operability**
Ease of keeping the system running smoothly by making routine tasks faster and easier to execute. Includes monitorability.

**Evolvability**
Ease of modifying and adding functionality as requirements change without introducing regressions. Also known as extensibility, plasticity, modifiability.

**Configurability**
Ability to change behaviour or system without requiring a change and deployment of its source code.

**Simplicity**
Ease of understanding how a system works.

**Accidental complexity**
System complexity not due to the problem domain. Eleviated by abstraction.

**Inherent complexity**
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