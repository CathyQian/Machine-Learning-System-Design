#### 1. What is Distributed Systems?
- A distributed system is essentially a group of independent computers that are linked together by a single network. 
- These groups of computers work together to appear as a single computer to the end user.
- These computers communicate and organize their actions by passing emssages, have a shared state and operate concurrently. 
- They can fail independently without affecting the entire system’s uptime, making distributed systems architecture a sort of failsafe.
- Example: A Blockchain system like the Bitcoin cryptocurrency is one example of a distributed system. In a traditional banking system, the ledger of all of the existing credits and debits exist on a centralized computer owned by the bank. A cryptocurrency uses blockchain to maintain a distributed ledger which exists on many different computers at once. Whenever a credit or debit is written to the ledger, it is written on every copy of the cryptocurrency’s ledger in the blockchain.

#### 2. What are the pros and cons of a distributed system?
Pros:
- improved performance as programs are executed concurrently
- reliability (such as fault tolerance)
    - Fault tolerance describes the distributed system’s ability to continue to function in the event of a partial failure. Faults are most often caused by hardware or software problems, or by malicious actors. Analyzing and increasing the fault tolerance of a distributed system involves solving for these factors.
    - Components can fail independently without disrupting other components' execution.
- incremental growth over time
- simple sharing of data and resources
- improved communication throughout the system

Cons (challenges to developers):
- heterogeneity
    - What is it? 
        - Various entities must be able to interoperate with each other in the system, without being affected by differences in operating systems, hardware architectures, communication protocols, programming languages, security models, data formats, and software interfaces.
    - The solution is a load balancer
        - A load balancer works to spread traffic across a number of different servers in order to make applications more responsive and available. For example, imagine 1 million users make requests to your application simultaneously. By putting a load balancer between your application and the users who are making requests, you can gain the benefit of having redundant versions of your application running on separate machines. If you had 5 machines running your application, the load balancer might send 200,000 of the 1,000,000 requests to each of the 5 machines, rather than having all of the requests processed by one machine.
        - A load balancer can also keep track of the status of the various machines running in your distributed system. For example, if a server fails and is not responding to requests (or perhaps it is responding with more errors than the other servers), the load balancer can stop sending a request to that particular server. Depending on the configuration, a load balancer might be responsible for spinning up new servers to handle more traffic, or for killing & replaces servers which seem to have failed. 
        - Load balancing is important because it is an effective tool for keeping an application high availability. Even if there is a power outage and some computers in your distributed system are not available, a load balancer combined with duplicate instances of your application can ensure your application is still available for users.
- openness 
    - The degree to which a complex computer system can be extended and re-implemented depends on openness
- security 
    - solution: encryption, authentication (passwords, public key authentication, etc.), and authorization (access control lists.)
    - Confidentiality can be implemented by utilizing protection against disclosure to unauthorized individuals, such as access control lists that provide authorized access to sensitive information. The integrity of the system can be improved by implementing protection against alternation or corruption. Availability, or protection against interference targeting access to the resources, can be implemented as well to block denial of service (DoS) attacks.Proof of sending and receiving information can be established through the use of digital signatures.
- scalability 
    - A distributed system should work efficiently at a range of different scales, from a small Intranet to the whole Internet. The performance of the system should be enhanced by addition of a resource. The system should also work with increasing number of users efficiently. There are some challenges in designing scalable distributed systems, which includes the cost of physical resources, problems getting the cost to linearly increase with system size, and performance loss. These challenges can take some work to tackle and often require a team.
    
- failure handling (which is usually the first thing tackled as failure prevention is the main benefit of distributed systems), concurrency
- transparency
    - Transparency ensures that the distributes system should be perceived as the single entity by the users or the application programmers rather than the collection of autonomous systems, which is cooperating. It includes the following:
        - *Access Transparency*. There should be no apparent difference between local and remote access methods.
        - *Location Transparency*. The location of an object in the system may not be visible to the user or programmer. 
        - *Concurrency Transparency*. Users and Applications should be able to access shared data or objects without interference between each other. 
        - *Replication Transparency*. If the system provides replication (for availability or performance reasons) it should not concern the user.
        - *Fault Transparency*. If software or hardware failures occur, these should be hidden from the user. 
        - *Migration Transparency*. If objects (processes or data) migrate (to provide better performance, or reliability, or to hide differences between hosts), this should be hidden from the user.
        - *Performance Transparency*. The configuration of the system should not be apparent to the user in terms of performance. 
        - *Scaling Transparency*. A system should be able to grow without affecting application algorithms. 

#### 3. How do components communicate in a distributed system?
Components in a distributed system communicate via message passing. Message passing means that each component in the distributed system has a contract which defines what sorts of messages it is able to receive and interpret, and what sorts of messages it will give as responses. Documenting these contracts for each component is important because people working on one component gain the ability to interact with the wider system by following the contract. A team responsible for the inner operations of two different components of the distributed system can work “safely”, so long as they do not change the contract of their microservice.

A common way for components in a distributed application to communicate is via HTTP. HTTP is the protocol your web browser uses to send data to and from web-pages on the internet. Within a distributed application, different servers can use HTTP similarly to communicate information amongst themselves and bring a full application piece of software to life. RESTful APIs are one common specification for how message passing can be accomplished between components in a web application.
There are other protocols that can achieve message passing between components, for example, Thrift and GRPC accomplish message fasting with higher throughput and without the use of HTTP. This makes them potentially good options for applications where the amount of traffic and communication between components is expected to be very high.

#### 4. How can distributed computing help large software teams be productive or organized?
Using distributed computing can be useful for large software teams, because the larger team can be organized into smaller units while reducing the risk that any given team introduces breaking changes to the wider system. Because distributed systems rely on message passing to communicate, and focus on high availability and performance of individual components, smaller teams can define a set of abstractions for only their part of the system. For example, the Surveys team and the Users team might define their own REST APIs for communicating with their respective components. Once these abstractions are defined, team members need to be less concerned about their changes impacting other pieces of the software. As long as the abstractions they have defined for their components do not change (e.g. you always retrieve a list of users from the GET /api/v2/users REST endpoint), the internal business logic can be changed with a low amount of risk. 

#### 5. What are some examples of distributed systems used today?
Some examples include the Internet, intranets, and mobile or ubiquitous computing.


#### 6. Describe one of the primary differences between parallel computing and distributed computing?
A major difference between parallel computing and distributed computing is that in a parallel computing system each component of the system has access to **shared memory**. In distributed computing, there is no shared memory and each component must communicate information about its internal state via message passing.

#### 7. What are the main differences between mobile and ubiquitous computing?
Mobile computing has a unique advantage when using different devices, such as mobile devices, laptops, and printers. Ubiquitous computing is used in a single environment, such as at home or in hospitals. 

#### 8. Why is Middleware used in a distributed system?
Middleware is essentially **a layer of software** with the sole purposes of **masking heterogeneity (enable the various components of a distributed system to communicate and manage data) in distributed system and providing a convenient and usable programming model to application programmers and developers**. Middleware is represented by processes and objects in a set of computers that interact with each other in order to implement communication and resource sharing support for distributed applications in the system.
![](2020-08-06-08-40-30.png)
Image copied from here: http://csis.pace.edu/~marchese/CS865/Lectures/Chap1/Chapter1a.htm

#### 9. How can data replication help improve the fault tolerance of a distributed system?
Data replication is the storage of replica data sets at a number of locations that can be interchangeably accessed by the application. If one data source encounters a hardware or software problem, the replica data set can be accessed quickly, leading to little risk of downtime for users. This increases the availability of the application thereby making it more fault tolerant.

#### 10. What are microservices in an application? How is an application built using microservices different than a monolithic application?
Microservices are small applications, usually running on their own infrastructure or within a virtual machine, which is responsible for one segment of a larger application’s business requirements. A microservice may have various software components like its own processing, memory, data storage, and its own APIs.

Imagine the opposite: a monolithic application which exists on one centralized piece of infrastructure. Our imaginary monolithic application is responsible for all or most of the application’s business requirements. For example, looking at the database models of our imaginary application a developer would likely see references to multiple entities in the system. e.g. Users, Organizations, Surveys, Subscriptions, and Teams. Our monolithic application exposes a large REST API, where clients can interact with any of these entities.

There are many ways to build a microservice architecture, but breaking such an application into microservices might commonly look like a separate application for each of the entities each of which may run on its own set of separate infrastructure. An entirely separate application can be built, for example, Users Application, Surveys Application, Subscriptions Application, and Teams Application, each of which exposes its own smaller REST API for interacting with the separate entities.

#### 11. Imagine you have a microservices architecture where one microservice is responsible for Users and a separate microservice is responsible for Wallets. These microservices use REST to communicate. While working on a requirement, an engineer on your team recommends the use of transactions so that: 1) when a User is created, a corresponding Wallet must be created 2) if the creation of the Wallet fails, the creation of the User should be rolled back to prevent incomplete data. What is the main issue with this solution?
In order for the User creation to roll back in case the Wallet creation later fails, the User microservice must keep some information about the current state of the Wallet service in its memory. Since RESTful APls are meant to remain stateless by definition, the use of transactions breaks the specification.

#### 12. What is protocol?
By definition, a protocol is used to refer to a common set of rules and formats that are used for communication between processes in order to perform a specific task. 
The definition of a protocol has two vital parts to it: A specification of the sequence of communicative messages that must be exchanged, and a specification in the format of the data in the messages.

#### 13. What are the different types of system models?
Architecture model, fundamental model, interaction model, failure model, and security model.


#### 14. What is the transparency dogma in distributed systems middleware and what is wrong with it?
Tips: Middleware tries to hide the fact of distribution. Does this work completely ? What happens in case of errors?

#### 15. What is a Single-point-of-failure and how can distribution help here?
A single point of failure (SPOF) is a part of a system that, if it fails, will stop the entire system from working. Distribution makes failure independent from other programs, therefore if anyone machines/programs fail, other machines/programs can still continue and finish the job.
