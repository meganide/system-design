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
1. Application Protocol (HTTP, HTTPS, SMTP). Rules for how specific types of apps talk over the internet.
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

## Application Protocols
Rules for how specific types of apps talk over the internet. Every app or service on the internet needs a common language so both sides know what the data means. An application protocol defines that language for a particular job.

This is basically the "languages" apps use to exchange information after TCP or UDP has delivered the data between devices.

### Remote Procedure Call (RPC)
A protocol that allows a program on one computer to execute code on another computer/server. Method to invoke a function as if it were a local call, when in reality the function is executed on a remote machine. 

## Caching
Benefits of caching:
- reduced latency
- lowered server load
- improved ux

### Server caching
Happens before hitting the database, so it can skip DB queries entirely.

Use server caching when you want full control and to reduce DB calls completely.

### Database caching
Happens inside the DB after a query arrives, so the DB still sees the request but may serve it faster.

Rely on database caching for automatic speedups, but not as your main scaling strategy. It's still bound by DB capacity.

### Eviction Policies (deciding what to remove when cache is full)

| Policy | How it works | When to use |
|--------|--------------|-------------|
| **LRU** (Least Recently Used) | Removes the item that hasn’t been accessed for the longest time. | General-purpose, good when recent access predicts future access. |
| **LFU** (Least Frequently Used) | Removes the item with the lowest access count. | Good for items that stay popular over time. |
| **FIFO** (First In, First Out) | Removes the oldest inserted item. | Simple, but may evict still-hot items. |
| **MRU** (Most Recently Used) | Removes the most recently accessed item. | Rare, useful if recent items are less likely to be used again (edge cases). |
| **Random** | Evicts a random item. | Useful for simplicity in large, distributed caches. |
| **TTL-based** | Items expire after a fixed time-to-live. | For data with predictable staleness bounds. |


### Write Strategies (how cache interacts with the backing store)

| Strategy | How it works | Trade-offs |
|----------|--------------|------------|
| **Write-through** | Write to cache and DB at the same time. | Data is always in sync, but slower writes. |
| **Write-back (write-behind)** | Write to cache first, flush to DB later. | Faster writes, risk of data loss if cache fails before flush. |
| **Write-around** | Write to DB, skip cache; cache only on read. | Avoids caching rarely used writes, but first read will be slow. |


### Read Strategies (how cache is filled)

| Strategy | How it works | Trade-offs |
|----------|--------------|------------|
| **Cache-aside (lazy loading)** | App checks cache → if miss, fetch from DB and put in cache. | Popular & simple; cache only stores hot data, but first read is slow. |
| **Read-through** | Cache automatically loads from DB on miss. | Transparent to app, but requires integrated cache layer. |
| **Refresh-ahead** | Cache refreshes items before they expire. | Avoids latency spikes on expiration, but may cache unused items. |


### CDN
Network of servers distributed geographically, generally used to serve static content such as JS, HTML, CSS, image, video files. They cache the content from the original server and deliver it to users from the nearest CDN server.

This leads to reduced latency, high availability and improved security (DDOS protection and traffic encryption).

## Proxy Server
Acts as an intermediary between a client requesting a resource and a server that returns a response.

Purposes:
- caching resources
- anonymizing requests
- load balancing

### Types of proxy servers
- forward proxy
- reverse proxy (NGINX)
    - load balancers
    - CDNs
    - Firewalls (WAFs)
    - SSL offloading (some reverse proxies handle the encryption and decryption of SSL and TLS traffic)

## Load Balancers
Distribute incoming traffic across multiple servers to make sure that no server bears too much load. By spreading the request effectively they increase the capacity and reliability of applications.

### Strategies

#### Round Robin
The simplest form of load balancing. Each server in the pool gets the request in sequential order.
Evenly distribute.

#### Least Connection
Send the request to the server with the least active connections.
Ideal for longer tasks and when the server load is not evenly distributed.

#### Least Response Time
Chooses the server with the lowest response time and fewest active connections.
The goal is to provide the fastest response to request.

#### IP Hashing
Determines which server gets the request based on the hash of the clients IP adress.
Ensures the client connects to the same server and is useful for session persistence, in applications where it's important that the clients connect to the same server.

#### Weighted algorithms
These methods can also be weighted. Servers are assigned weights, typically based on their capacity or performance metrics.
The servers which are more capable handle more requests. This is effective if the servers in the pool have different capabilities.

#### Geographic
Directs requests to the server geographically closest to the user. Useful for global services where latency reduction is a priority.

#### Consistent Hashing
Consistent hashing spreads requests or data across servers so that when servers are added or removed, only a small portion of the mappings change, reducing disruption.

### Health Checks
An essential feature of load balancers is continous health checking of servers. To ensure traffic is only directed to servers that are all online and responsive.
If a server fails, the load balancer will stop sending traffic to it until it is back online. 

## Types of load balancers

### Software load balancers
- HAPROXY
- NGINX

### Cloud based load balancers
- AWS Elastic Load Balancing
- Microsoft Azure Load Balancer
- Google Cloud Load Balancer 

### What happens when a load balancer goes down?
When the load balancer goes down it can impact the whole availability and performance of the application.

It's a single point of failure, if it goes down all the services become unavailable for the clients.

To avoid or minimize a load balancer goes down we have several strategies: 

- Redundancy: Add another load balancer. If one of them fails, the other one takes over (failover).
-  Health checks & monitoring: Detect & adress issues early before causing signifcant disruption.
- Auto-scaling & self healing systems: Automatically detect the failure of load balancers and replace with a new instance without manual intervention.

## Databases

### Relational databases
- great for transactions, complex queries, and integrity
- ACID compliant -> they maintain the ACID properties
    - A: atomicity, transactions are all or nothing
    - C: consistency, after a transaction your database should be in a consistent state
    - I: isolation, transactions should be independent
    - D: durability, once transaction is comitted the data is there to stay

### NoSQL Databases
- The consistency property is dropped from the ACID
- MongoDB, Cassandra, Redis, DynamoDB, Cosmos

Types of NoSQL databases:
- Key value pairs (Redis)
- Document based (MongoDB)
- Graph based (neo4j)

NoSQL databases are schemaless which means they dont have FK between tables which link the data together.
They are glued for unstructured data.

Ideal for:
- scalability, quick iteration, and simple queries

### In-memory databases
It's fast because everything is in memory.

Very fast data-retrieval. Primarily used for caching and session-storage.

Redis and mem-cache are some examples.

### Vertical Scaling
Also called scale up. Improve the performance of your database, by enhancing the capabilities of the server where your DB is running. Very limited, because there is a maximum limited to the resources you can add to a single machine.

1. Increase CPU power
2. Add more RAM
3. Add more disk storage
4. upgrade network

### Horizontal scaling
Also called scale out. Add more machines to the existing pool for resources.

#### Database sharding
Distributing different portions/shards of the dataset across multiple servers.

Sharding strategies:
- Range-based sharding: Distribute data based on the range of a given key
- Directory-based sharding: Lookup service to direct traffic to the database.
- Geographical sharding: Split databses based on geographical locations.

#### Replication
Keep copies of data on multiple servers for high availability.

Master-slave replication:
1. You have one master and several slaves
2. Master is responsible for read/write
3. Slaves are read only
4. When new data comes in it writes to the master and then that is copied over to the slaves

Master-master replication:
1. Multiple databases that can both read and write.

#### Database performance
Scaling your database is one thing, but you also need to access the data faster. 

- Caching. Cache frequent queries with in Redis to boost performance.
- Indexing. Create index for frequently accessed columns to speed up retrieval times.
- Query optimization. Minimize joins, one way is to denormalize.