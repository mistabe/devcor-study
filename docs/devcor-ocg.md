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

