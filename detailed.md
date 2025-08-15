# System Design Detailed
How do you glue an entire system together?

System design is all about trade-offs, it's not about finding the perfect solution it's about finding the best solution for our specific use case.

## Computer Architecture
Computers function through a layered system, each optimized for a specific task.

At the core computers only understand binary, 0s and 1s, these are represented as bits. 

- **Bit:** The smallest piece of data, either 0 or 1.
- **Byte:** 8 bits, used to represent a single character or number (Like A or 1).
- **KB:** 1024 bytes
- **MB:** 1024 KBs
- **GB:** 1024 MBs
- **TB:** 1024 GBs

### Disk Storage
To store this data we have computer disk storage, which holds the primary data, it can be either HDD or SDD type.
It is non-volatile, which means that it maintains data without power. If you turn it off, the data will still be there.

- Size: Hundreds of GBs to multiple TBs
- HDD read speed: 80 MB/s to 160 MB/s
- SDD read speed: 500 MB/s to 3500 MB/s

### RAM (Random Access Memory)
RAM serves as the primary active data holder and holds data structures, variables, and application data that are currently in use or being processed.   

When a program runs its variables, intermediate computations, and more are stored in RAM because it allows for quick read and write access. This is a volatile memory which means that it requires power to retain is contents, if you restart your computer the data might not be persisted.

- Size: A few GBs to hundreds of GBs in high end servers.
- Read/Write speed often >5000 MB/s

### Cache
But sometimes, even the RAM speed isn't enough which brings us to the cache.
The cache is smaller than the RAM, typically measured in MBs.

The access times for cache memory are even faster than RAM, just a few nano seconds.

The CPU first checks the L1 cache for the data, if it is not found it then checks the L2 and the L3. 
Finally, it checks the RAM.

The purpose of the cache is to reduce the average time to access data, that's why we store frequently accessed data here to optimize CPU performance. 

### CPU
CPU the brain of the computer, it fetches, decodes and executes instructions.
When you run your code it is the CPU that processes the operations in that program.

Before it can run our code which is often written in high level languages like Python, JS, Java etc. it first needs to be compiled to machine code (the loewst level representation of a computer program, 0s and 1s).

A compiler performs these translations and then the CPU can execute it.  

The CPU can read and write from our RAM, disk and cache data.

### GPU (Graphics Processing Unit)

The GPU is specialized for parallel processing, originally for rendering graphics but now also used for AI, simulations, and data-heavy computations.

Unlike CPUs with a few powerful cores, GPUs have hundreds or thousands of smaller cores for handling many tasks at once. They also have dedicated high-speed VRAM (4 GB to 48+ GB) for storing data during processing.

In modern systems, the CPU handles general logic, while the GPU takes on highly parallel workloads for speed and efficiency.

### Motherboard
The component that connects everything, provides the pathways that allow the data to flow between these components. 

## High-level Architecture of a Production-ready App

### CI/CD
Continous Integration and Continous Deployment, ensures that our code goes from the repo to a series of tests and pipeline checks onto the production server without any intervention.

Configured with platforms like Github actions or Jenkins.

## What makes a good design?

- **Scalability:** Our system's growth with our user base
- **Maintainability:** Ensuring future developers can understand and improve our system. 
- **Efficiency:** Making the best use of our resources.
- **Reliability:** Planning for failure. Building a system that not only performs well when everything is running smoothly, but also maintains its composure when things go wrong.

System design is all about:

1. Moving data
    - Ensuring that data can flow seamlessly from one part of our system to another.
    - User requests
    - Data transfers between databases
    - Need to optimize for speed and security 

2. Storing data
    - access patterns
    - indexing strategies
    - backup solution
    - data needs to be readily available when needed

3. Transforming data
    - taking raw data and turning it into meaningful information

### System Design Concepts

- **Availability** → “Is the system up and responsive right now?”  
- **Reliability** → “When it’s up, can I trust it to work correctly every time?”  
- **Resilience** → “If it breaks, can it recover and keep going?”  
- **Scalability** → “Can the system handle more load by adding resources?”  
- **Maintainability** → “Can we easily update, fix, and improve the system over time?”  
- **Performance** → “Does the system meet the required speed and efficiency?”  
- **Security** → “Is the system protected against unauthorized access and attacks?”  
- **Observability** → “Can we monitor, measure, and understand the system’s internal state?”  

### CAP Theorem
Set of principles to guide us in making informed trade-offs between 3 key components of a distributed system:

- **Consistency:** Ensures that all nodes in a distributed system have the same data at the same time. If you make a change to one node it should be reflected in all nodes. Think of it like updating a google doc, if one person makes an edit, everyone else sees that edit immediately.
- **Availability:** The system is always operational and responsive to requests regardless of what might be happening behind the scenes. Doesn't matter if some nodes are slow, crashed, or overloaded, we want to keep the service up and answering queries. Like an online store that is always open no matter when you visit and ready to take your order. *"I will give you an answer right now even if it's not the latest data"*
- **Partition Tolerance:** The system's ability to continue functioning even when a network partition occur. Meaning that if there is a distruption in communication between nodes the system still works. Like a group chat, if one person loses connection the rest of the group can still keep chatting. 
*"If parts of the system can’t talk to each other, will each part keep running?"*

According to CAP theorem a distributed system can only achieve 2 out of these 3 properties at the same time.

### Availability
The measure of the system's operational performance and reliability.

"Is our system up and running when our users need it?"

Measured in terms of percentage like:

- 99.9% availability -> 8.76 hours of downtime per year
- 99.999% availability -> 5 minutes of downtime per year

### SLO - Service Level Objectives
Setting up goals for our systems performance and availability.

Example:
- Our web service should respond to requests within 300ms 99.9% of the times

### SLA - Service Level Agreements
Formal contracts with our users or customers.

Minimum level of service we're committing to provide.

Example:
- If our SLA guarantees 99.99% availability and we drop below that we might need to provide some sort of compensation to our customers.

## Resilience
The ability of a system to keep operating correctly when faced with failures, disruptions, or unexpected contions.
Adapting to problems and bouncing back without catastrophic impact.

### Key Aspects
- **Fault tolerance**  
  The system can keep running even if some components fail.  
  *Example:* If one server goes down, another automatically takes over.  

- **Graceful degradation**  
  If part of the system fails, it reduces functionality instead of completely breaking.  
  *Example:* A video streaming site lowering video quality when the network is slow.  

- **Self-healing**  
  The system detects problems and recovers automatically.  
  *Example:* A crashed process restarts without manual intervention.  

- **Redundancy**  
  Having backup components or duplicate data so there’s no single point of failure.  

- **Rapid recovery**  
  If something does fail, the system can restore normal operations quickly.  

## Speed
To measure the speed of our system we have throughput and latency.

### Throughput
How much data our system can handle over a certain period of time.

- Server throughput is measured in requests per second (RPS). How many client requests our system can handle in a second.
- DB throughput is measured in queries per second (QPS) and quantifies the number of queries a database can process in a second.
- Data throughput is measured in bytes per second (B/s) and reflects the amounts of data transfered over the network or processed by a system in a given period of time.

### Latency
How long it takes to handle a single request. The time for a request to get a response. 

Optimizing for either throughput or latency can result in sacrifices in the other.

For example batching operations can increase throughput but increase latency.

## Risks of poor system design

- Performance bottlenecks
- Security vulnerabilities

Unlike code which can be refactored, redesigning a system can be a monumental task.
That's why it's crucial to invest resources and time into getting the design right from the start.

## Networking Basics
How computers communicate with each other.

The internet works in layers.
1. Application Protocol (HTTP, HTTPS, SMTP)
2. Transport layer (TCP, UDP). Move data from one computer to another.
3. Internet Protocol (IP). Where to send data?

TCP and UDP are protocols, which basically means rules for how devices talk to each other over the internet.

- **TCP:** Make sure every word in my message ges to you, in order, without mistakes.
- **UDP:** Just get it there fast, even if we lose a few bits - the conversation must keep flowing.

### Networking Infrastructure
Devices on a network have either public or private IP adresses.
Public IP adresses are unique across the internet whereas private IP adresses are unique across the local network.

An IP adress can either be static (non-changing) or dynamic (changing periodically over time).

Devices connected in a local area network (LAN) can communicate with each other directly.
To protect networks we use firewalls which monitors and controls incoming and outgoing network traffic based on security policies.

Within a device, specific processes or services are identified by ports, and when combined with an IP adress create a unique identifier for a network service. 

Some ports are reserved for specific protocols like:
- HTTP: 80
- HTTPS: 443
- SSH: 20
- MYSQL: 3306





