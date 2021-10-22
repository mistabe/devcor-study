# Cisco DEVCOR Official Certfication Guide

<https://www.ciscopress.com/store/cisco-devnet-professional-devcor-350-901-study-guide-9780137500048>

## Notes for reference

### 1.1 Describe sitributed applications related to the concepts of front end, back end and load balancing

Presentation and Application logic seperation.
Presentation being what's served to the client, which may not always be human.
Complex Distributed applications can appear such that the backend of one service is the front end of another service. E.g. when an application backend sends data to be stored, it likely uses the API of another service rather than "directly connecting to it" (one assumes one is omitting the presentation layer otherwise this thread seems non-sensical).

HTML, CSS and JavaScript are the most popular Front End (Client Side) languages.

Classic web apps use server-side rendering. Problematic because the information has to travel to/from the server all the time.
Adding more servers and using load balancing is a way of ~~spending more money~~ scaling that out. Most current Load Balancers employ plenty more technologies like;

- Caching
- Compression
- SSL Offloading

Specific benefits of Load Balancing are;

- High Availabilty and reliability
- Reduced Downtime
- Redundancy
- Flexibility
- Efficiency

GSLB (Global Server Load Balancing) addressing lattency issues (assuming you're working with an applicationt that's used round the globe). GSLB designs include servers at multiple geographic locations ensuring that clients connect to a server that's close to them.

Moving into modernity, modern web architectures use client side processes and move away from doing everything on the server side. Do read [The System](https://www.waterstones.com/book/the-system/james-ball/9781526607232) by James Ball for an insight into the ebb and flow of centralised and de-centralised operations

### 1.2 Evaluate an application design considering scalability and modularity

Modularity focuses on loose-coupling. The main idea is ensuring that each task is handled well and by one part of the system. Individule modules are called _implementations_.
Internal details of modules should be hidden and only accessible via _interfaces_. All interactions between modules should only happen between those interfaces.

It's not difficult to understand the advantages of modularity over monolithic applications.

- Re-use
- Decomposition
- Troubleshooting is easier
- Testing is easier (unit testing)
- Re-factoring is easier

This all pertains to application source code. Modularity does stretch into the realms of the application architecture itself in the form of microservices architectures.
They have the same advantages as above, but with the additional explicit notion of using only a standard API interface between them.

Scalability becomes more nuanced and measured in different ways, now;

- Administrative Scalability
- Functional Scalability
- Geographic Scalability
- Load Scalability
- Generation Scalability
- Heterogenous Scalability

To add to that, "resource scalability" as it's called can either go up or out - scaling up/down and scaling out/in. For the old-timers amongst you, that's adding resources to a single VM, or adding lots of small VMs - welcome [K8S](https://kubernetes.io/).

And why end there when you can call something "Hybrid" and do both up/down and out/in, so horizontal and vertical scaling.

Monolithism is fond of vertical scaling due to it's architectural nature. Distributed applications are fond of horizontal scaling, but can certainly take advantage of vertical, too. "Peak" horizontal scaling is achieved when distributed applications are "stateless" and do not hold the notion of the connection from the client and back to the server database and are for all intents a "hollow bone".

### 1.3 Evaluate an application design considering high availability and resiliency (including on-premises, hybrid and cloud)

High availability and resiliency are Methodolgies.

Avaiability is the measurement of the system being in an operable state over time. 
High Availabilty is measured using the expression of 9s. Two nines, three nines, etc. Each extra nine increases availablity. There is an infelction point where cost and complexity become relevant. At the time of writing "five nines" is enough to please most organisations on the planet.

Three principles to achieve high availability;

- Elimination of single points of failure
- Reliable failover(switchover)
- Detection of failures as they occur

Calls for stateless data flow design. Use of Clusters is pretty much mandatory.

These methods make applications more resilient;

- Parameter checking
- Timeouts
- Asynchronous communication
- Fan out and use the quickest response
- Fail fast
- Circuit breaker
- Retries
- Fallback mechanisms

Resilience tries to survive the failure with error handling and correction, while high availability's goal is to detect and work around the problem by replacing a failed component.

### 1.4 Evaluate an application design considering latency and rate limiting

End to end latency has several components;

- Application latency
- OS and TCP stack latency
- NIC latency
- Cable distance latency
- Port-to-Port latency within each network device along the path

It takes 93ms to push a 1500-byte packet onto an ISDN 128KB interface. 0.5usec on a 25Gbps ethernet interface. Broadly it's at least 100ms in cable latency for a packet to reach the other side of the world.

Latency reduction using CDNs, Geo-replicated services. Pagination to get data in chunks rather than calling all at once. Choices of UDP/TCP - UDP is stateless and is liberated from the TCP three-way handshake. 
Use API gateways, WAFs and other constructs to:

- Manage REST API calls with 429 "Too many requests" responses
- DDoS protection
- Brute force protection - login failure limting
- Web scraping protection

Token buckets are used for temporal resource consumption. 

### 1.5 Evaluate an application design and implementation considering maintainability

The majority of the cost of software is in maintaining the software, not the initial development.
Software _maintainability_ pertains to that problem.

- Naming conventions
- Modularity
- Version Control
- OOP

[SOLID](https://en.wikipedia.org/wiki/SOLID) is a principle by our friend "[Uncle Bob](https://en.wikipedia.org/wiki/Robert_C._Martin)" to make software deisgns more understandable, flexible and maintainable.

Broadly, then. Do the following for an easy to _maintain_ application:

- Write consistent, readable code
- Review and iterate code, often
- Re-factor to improve understandability
- Maintain good documentation
- Use Continuous Integration

For a higher _quality_ application:

- Use naming conventions
- Style Guides [PEP8](https://www.python.org/dev/peps/pep-0008/)
- Use docstrings (Python)
- Be lazy and use shared libraries and tools
- [Unit Test](https://docs.python.org/3/library/unittest.html)

### 1.6 Evaluate an application design and implementation considering observability

The three pillars of observability (observability pillers) are:

- Logs
- Metrics
- Tracing

Logs are the most basic, agricultural form of observability tool, aparrently. :(

Metrics you can associate with SNMP. Interface counters, CPU usage, etc. 

Distributed Tracing is exactly that, distributed through systems through the use of a GUID for each request made so that perormance can be tracked end to end.

Obervability is a system property. These are the facts that must be built into a system to make it trul observable; 

- A complex system is never fully healthy.
- Distributed systems are very unpredicatable.
- It's impossible to predict all failure states.
- Failures need to be addressed at all phases - design, implementation, operation etc.
- Ease of debugging is critical

### 1.7 Diagnose problems with an application given logs related to an event

Often Syslog is used as a starting point for how to develop the structure of logs.

Capture code with Try/Except (else/finally) I call them "TEEF" said illiterately "Teeth" blocks.

### 1.8 Evaluate choice of database types with respect to application requirements (such as relational, document, graph, columnar, and time series)

Considered possibly _the_ most important decision when designing software.
The features, positives and benefits of each platform need to be considered. 

Data is ususally structured or unstructured, sometimes semi-structured.

_Transactions_ are a sequence of operations performed by a database as a single logical unit of work.

[ACID](https://en.wikipedia.org/wiki/ACID) describes the following;

- Atomicity
- Consistency
- Isolation
- Durability

[BASE](https://en.wikipedia.org/wiki/Eventual_consistency) is an alternative to ACID which takes a more relaxed and therefore likely cost-effective but higher latency database offering.

A distributed database (BGP, an Excel spreadsheet shared by Co-workers over email) is on muliple nodes connected by a network. The [CAP theorum](https://en.wikipedia.org/wiki/CAP_theorem) states you can have no more than two of the below three following guarantees - note CAP are the first letter of the each guarantee;

- Consistency
- Availability
- Partition Tolerance

Partitioning tolerance is a critical requiremnet for a distributed system as no network is safe from failures.

Databases designed with ACID choose consistency over availability and BASE choose availability over consistency. Banks versus Social Media as an example.

NoSQL database or Non-Structured Query Language databases could be document-oriented, graph, column-oriented, key-value, and time-series databases.

Document-oriented - MongoDB

Graph databases are all the rage. Consist of _Nodes_ and _Edges_ (or graphs or relationships). Edges are the key concept in graph databases, representing an abstraction that is not directly implemented in other databases.
Graph databases are always implemented with another type of database (document, key-value, or a traditioanl relational) that is used to store the graph data. They include [Neo4j](https://neo4j.com/), [OrientDB](https://orientdb.org/) etc.

Column-oriented - [Apache Cassandra](https://cassandra.apache.org/_/index.html), [MariaDB](https://mariadb.org/)

Key-Value databases like [Redis](https://en.wikipedia.org/wiki/Redis) support search only by the key value. 

Time-series databases like [InfluxDB](https://en.wikipedia.org/wiki/InfluxDB) enjoy metrics, events, or measurements that are time-stamped. Typically used for sensor and telemetry data.

### 1.9 Explain architecutural patterns (monolithic, services-oriented, microsservices and event-driven)

Software architectures are costly to change once implemented.

- Functional requirements like the general flow of operation and the architecture pattern.
- Non-functional requirements like the "Ops" things of reliability, operability, performance efficiency, security, compatibility, maintainability, and so on.
- Compliance requirements like legal, social, financial, competitive and technological concerns.

Reference architecture or architectural patterns are there to provide standard ways to address repeatable concerns.

- Monolithic
- Service-oriented
- Micrsoservices
- Event-driven

Monolitic applications are generally miserable and I won't labour on them here. I get what's rubbish

Service Oriented Architecture. There's a lot to be said here and I think I'll have to re-visit. 

Microservices is a variant of the Services Oriented Architecture that's refined to the point where each service is doing just one thing, whereas SOA is bigger and more complicated per instance. 
Common use case is Cloud Native and container based - almost by virtue of the description mandates a container orchestration solution.

Benefits are modularity, ease of scalability, distributed development and unique/most appropriate software language/platforms etc for each service if neccesary.

Downsides are the complexity of the orchestration and latency due to the increased services that each reuest must touch.

Event Driven Architecture is based on the production, consumption and reaction to events.

Events are defined as a significant change in state which is asynchronous in nature.
Azure Event Hub, Azure Event Grid and Azure Service Bus are all features of the Azure platform that can provide services that [deal with messages](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services) in the way described.

Two topologies exit, the Event Mediator and the Event Broker. The mediator topology is commonly used when there is a need to orchestrate multiple steps within an event through a central mediator, whereas the broker topology is used to chain events and responses together directly.
Higly complicated to implement and maintain, but is highy scalable due to it's distributed and loosely coupled nature.

### 1.10 Utilize advanced version control operations with Git

Git is a distributed version control system.

- Working directory
- Staging area
- Local repository

`git add` , `git commit` , `git log` , `git cat-file`

Are all useful commands working with your local respository.

`git branch` , `git checkout` , `git show` , `git merge` , `git rebase`

Are useful commands when working with branches

`git commit` , `git stash` , `git reset` , `git status`

### 1.11 Explain the concepts of release packaging and dependency management

### 1.12 Construct a sequence diagram that includes API calls

[diagrams.net](https://app.diagrams.net/) is our friend here.
Examples are published [here](https://github.com/jgraph/drawio-diagrams) on GitHub

[Unified Modelling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language) visualises the design of systems and templates are available directly in diagrams.net

The vertical axis represents time progressing down the page

You can use a tool which uses a textual notation to describe UML models called [PlantUML](https://plantuml.com/sequence-diagram)
