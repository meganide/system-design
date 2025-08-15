# System Design Overview

## 1. Client-Server Architecture
A client (web browser, mobile app) sends a request to the server to store, retrieve or modify data. 

## 2. IP Adress
In order for the client to know where to send the request to the server we need an IP adress.

## 3. Domain Name System (DNS)
When we visit a website we don't type the IP adress, we just enter the website name.
Instead of remembering IP adresess we use domain names that's more human friendly.
The domain name needs to be mapped to the corresponding IP adress.
That's where DNS (Doman Name System comes) in.

It maps domain names to IP adresses.
When you type in a website name, your computer asks a DNS server for the corresponding IP adress.
Once the DNS server responds with the IP adress your browser uses it to establish a connection with the server and make a request.

## 4. Proxy / Reverse Proxy

### Proxy
When you visit a website your request doesn't always directly go to the server first.
Sometimes it passes through a proxy or a reverse proxy first. 

It acts as a middleman between your device and the internet. 

When you request a web page: 
- client sends a request to the proxy
- the proxy forwards your request to the target server
- retrieves the response and sends it back to you

The client knows it's using a proxy, and the proxy knows the client's identity.
A proxy server hides your IP adress, keeping your location and identity private.

**Typical uses:**
- Privacy & anonymity: hides the client's IP from target server
- Content filtering: blocking or allowing certain websites.
- Caching: speeds up repeated requests by storing responses.
- Access control: allowing only certain users ot devices to connect to the internet.

### Reverse Proxy
A reverse proxy sits between clients and one or more servers.

When you request a web page:
- client sends a request to the the reverse proxy
- the proxy forwards them to one of the backend servers
- returns the response to the client

The client doesn't know which backend server handeld the request, it only sees the reverse proxy.

**Typical uses:**
- Load balancing: distributes traffic across multiple servers.
- SSL termination: handles encryption/decryption so backend servers don't have to.
- Security: hides backend server details, preventing direct access.
- Caching & compression: Speeds up responses and reduces load.
- Web application firewall (WAF): filters malicious traffic

## 5. Latency
Whenever a client communicates with a server, there is always some delay.
One of the biggest cause of this delay is physical distance.

If the client is in a different country than the server, the client first has to travel all that physical distance to the server and in order to get the response we need to do the same long travel back. This round trip delay is called latency.

High latency can make applications feel slow and unresponsive.

One way to reduce latency is by deploying our service across multiple data centers worldwide.
This way, users can connect to the nearest server instead of waiting for data to travel across the globe.

## 6. HTTP / HTTPS
Once a connection is made, how do clients and servers actually communicate?

They communicate using a set of rules called HTTP or HTTPS.
HTTP has a major security flaw, sending data in plain text.
Modern websites use HTTPS which encrypts all data using SSL or TLS protocol.
This ensures that if anyone intercepts the request they can't read or alter the data.

## 7. APIS
An API is a middleman that allows clients to communicate with servers without worrying about low level details.

## 8. REST
REST defines how clients and servers communicate over HTTP in a structured way.

REST endpoints often return more data than needed, leading to inefficient network usage.

## 9. GraphQL
To adress the issues with over-fetching with REST, GraphQL was introduced 2015 by Facebook.
GraphQL lets clients ask for exactly what they need. You can combine requests into one with GraphQL.
GraphQL is harder to cache than REST.

## 10. Database
Database ensure that data is stored, retrieved and managed efficiently while keeping it secure, consistent and durable.

## 11. SQL vs NoSQL
SQL databases store data in tables with a strict predefined schema and follow ACID properties.
They are ideal for applications that require strong consistency and structured relationships, such as banking systems.

NoSQL databases are designed for high scalability and performance, has a flexible schema, and use different data models like:
- key-value stores
- document stores
- graph databases
- wide-column stores 

## 12. Vertical Scaling
As our user base grows so does the number of requests hittings our server.
One of the quickest solutions is by upgrading the existing server by adding more CPU, RAM, or storage. 
This is called vertical scaling or scaling up, which makes a single machine more powerful.

There are major limitations though:
- You can't keep upgrading a server forever, every machine has a maximum capacity
- More powerful servers become exponentially more expensive
- Single point of failure, if this entire server crashes then the entire system goes down

Vertical scaling is a quick fix and not a long term solution for handling high traffic and ensuring system reliability.

## 13. Horizontal Scaling
A better approach that is going to make our system more scalable and fault tolerant is to add more servers to share the load.
This approach is called horizontal scaling or scaling out.

More servers --> More capacity --> System can handle increasing traffic more effectively.

If one server goes down, others can take over which improves reliability.

## 14. Load Balancer
Horizontal scaling introduces a new challenge, how does clients know which server to connect to?
This is where a load balancer comes in.

A load balancer sits in between clients and backend servers, acting as a traffic manager that distributes traffic across multiple servers.
If one server crashes, the load balancer automatically redirects traffic to another healthy server. 

But how does a load balancer decide which server should handle the next request?
It uses load balancing alghoritms such as:

- Round-robin
- Least connections
- IP hashing 

## 15. Indexing
One of the quickest and most effective ways to speed up database read queries is indexing.  
Think of it like the index page of a book, instead of reading through the entire book you jump directly to the relevant section.

A database index works the same way, it's a super efficient lookup table that helps the database to quickly locate the required data without scanning the entire table.

An index stores column values along with pointers to actual data rows in the table. Indexes are typically created on columns that are frequently queried such as primary keys, foreign keys, and columns frequently used in where conditions.

While indexes speed up reads they slow down writes, since the index needs to be updated whenever the data changes.
That's why we should only index the most frequently accessed columns.

## 16. Replication
But, what if indexing isn't enough and our single database server can't handle the growing number of read queries? That's where our next database scaling technique comes in, replication.

Just like we added more servers to handle increasing traffic, we can scale our database by creating copies of it across multiple servers.

### How it works
- We have one primary database called *primary replica*, that handles all write operations
- We have multiple read replicas that handle read queries
- Whenever data is written to primary database it gets copied to read replicas so that they stay in sync

Replication improves the read performance since read requests are spread across multiple replicas reducing the load on each one.
This also improves reliability since if the primary replica fails, a read replica can take over as the new primary.

Replication is great for scaling read heavy applications.   

## 17. Sharding / Horizontal Partitioning
But what if we need to scale write operations or store huge amounts of data?

Instead of keeping everything in one place, we split the database into smaller, more manageable pieces and distribute them across multiple servers.
This technique is called sharding.

### How it works
- We divide the database into smaller parts called shards.
- Each shard contains a subset of the total data (i.e shard 1 might have rows 0-1M, shards 2 might have rows 1-2M etc...)
- Data is distributed based on a sharding key, for example user ID

By distributing data this way we reduce database load, since each shard only handles a portion of the queries we speed up read and write performance.

Sharding is also refered to as *horizontal partitoning*, since it splits data by rows.

## 18. Vertical Partitioning
But what if the issue isn't the number of rows, but rather the number of columns?
In such cases we use vertical partitioning, where we split the database by columns. 

Imagine we have a user table that stores profile details, login history and billing information.
As this table grows, queries become slower because the table must scan many columns even when a request only needs a few specific fields.

To optimize this we use vertical partitioning.
- Split a table into smaller, more focused tables, based on usage patterns 
- Table 1. User_Profile, Table 2. User_Login, Table 3. User_Billing
- This improves query performance since each request only scans relevant columns instead of the entire table
- It also reduces unnecessary disk I/O making data retrieval quicker

## 19. Caching
However, no matter how much we optimize the database, retrieving data from disk is always slower than from disk.
What if we could store frequently accessed data in-memory? This is called caching.

Caching is used to optimize the performance of a system by storing frequently accessed data in memory, instead of repeatedly fetching it from the database.

One of the most commong caching strategies is the **cache-aside pattern**.
1. When a user requests the data the application first checks the cache
2. If the data is in the cache, it is returned instantly, avoiding the database call
3. If the data is not in the cache, the application retrieves it from the database and then stores it in the cache for future requests and returns it to the user
4. Next time the data is requested it is served directly from the cache, making the request much faster

To prevent outdated data from being served, we use time to live value or TTL. 

## 20. Denormalization
Most relational databases use normalization to store data efficiently by breaking it into separate tables.
While this reduces redundancy it also introduces joins (combines two tables), which can slow down queries as the dataset grows.

Denormalization reduces the number of joins by combining related data into a single table, even if it means that some data gets duplicated. 

For example, instead of keeping Users and Orders on separate tables we create a User_Orders table. 
Now when retrieving a user's order history we don't need a join operation, since the data is already stored together.
This leads to faster queries and better read performance. 

Denormalization is often used in read-heavy applications where speed is more critical. 
The downside is that it leads to increased storage and more complex update requests. 

## 21. CAP Theorem
A distributed system is a collection of independent computers that work together as a single system, communicating over a network to share resources and coordinate tasks. The goal is to achieve scalability, fault tolerance, and high availability beyond what a single machine can provide.

- **Scalability:** The system's ability to handle increasing workloads by adding resources.
- **Fault Tolerance:** The ability to keep functioning correctly even if parts of the system fail.
- **High Availability:** The system is designed to be operational and accessible for the maximum possible time, minimizing downtime.

One of the fundamental principles of distributed systems is the CAP theorem, which states that a distributed system can guarantee only two of these three at the same time:

- **Consistency:** Every user sees the same data at the same time.
- **Availability:** The system always responds to requests, even if some parts are down.
- **Partition tolerance:** The system keeps working even if network problems split it into parts that can't talk to each other.

## 22. Blob Storage
We use a blob storage to store like Amazon S3 to store individual files like images, videos or documents.
They are stored inside logical container or buckets in the cloud.
Each file gets a unique URL, making it easy to retrieve and serve over the web.

### Advantages of blob storage
- Scalability
- Pay as you go model
- Automatic replication
- Easy access

## 22. Content Delivery Network (CDN)
A CDN can deliver content faster to users based on their location, reducing buffering and load times.

A CDN is a global network of distributed servers spread out in many countries.
They work together to deliver web content and can deliver it faster due to them being closer to the user.

- HTML pages
- JS files
- Images
- Videos

Since content is served from the closest CDN server users experience faster load times with minimal buffering. 

## 23. Websockets
Most web application use HTTP which follows a request-response model. This works fine for static web pages but is too slow and inefficient for real-time applications like:

- Live chat apps
- Stock market dashboards
- Online multiplayer games

With HTTP the only way to get real time updates is through frequent polling, sending repeated requests every few seconds.
But polling is inefficient because it increases sever load and wastes bandwidth, and most responses are emtpy when there is no new data.

Websockets solves this problem by allowing continous two-way communication between the client and the server, over a single persistent connection. The client & the server can send messages to each other at any given time.

## 24. Webhooks
What if a server needs to notify another server when an event occurs?

For example, when a user makes a payment, the payment gateway needs to notify our application instantly.
Instead of constantly polling an API to check if an event has occured, webhooks allow a server to send an HTTP request to another server when an event occurs.

The receiver (for exampel your app), registers a webhook url with the provider.
When an event occurs the provuder will send a HTTP POST request to the registered webhook URL with the event details.

This saves server resources and reduces unnecessary API calls.

## 25. Microservices
Traditionally, applications were built using a monolithic architecture where all features are inside one large codebase.
This works for small apps but for larger scale systems, monoliths become hard to manage, scale and deploy.

The solution is to break down your app into smaller independent services called microservices that work together.

Each microservice handles a single responsibility, has its own database and logic, so it can scale indepdently.
They communicate with other microservices using APIs or message queues. 
This way services can be scaled and deplyed individually without affecting the entire system. 

##  26. Message Queues
However, when multiple microservices need to communicate directly, API calls aren't always efficient.
This is where message queues come in. Synchronous communcation, for example waiting for immediate responses, doesn't scale well. 

A message queue (like Kafka or RabbitMQ) enables services to communicate asynchronously, allowing requests to be processed without blocking other operations. 

### How it works
1. A producer places a message in the queue.
2. The queue temporarily holds the message.
3. The consumer retrieves the message and processes it.

Using message queues we can decouple services and improve the scalability.
We can prevent overload on internal services within our system, but how do we prevent overload for the public APIs?

## 27. Rate limiting
We can prevent overload for our public APIs with rate limiting.

Imagine a bot makes thousands of requests per second to your website, without restrictions this could crash your servers by consuming all available resources. 

Rate limiting restricts the number of requests a client can send within a specific time frame.
Every user or IP adress is assigned a request quota (like 100 requests per minute).
If they exceed this limit the server blocks additional requests temporarily.  

There are various rate limiting algorithms:
- fixed window
- sliding window
- token bucket

(Cloudflare, Redis)

## 28. API Gateway
We don't need to implement our own rate limiting system, this can be handled by an API gateway.

An API gateway is a centralized service that handles:

- authentication
- rate limiting
- logging
- monitoring
- request routing
- and more

Imagine a microservices based application with multiple services, instead of exposing each service directly, an API gateway acts as a single entry point for all client requests that sits between client and servers/services. 
It routes the request to the appropiate microservice and the response is sent back through the gateway to the client.

API gateway simplifies API management and improves scalability and security.

## 29. Idempotency

In distributed systems, network failures and service retries are common.
If a user accidentally refreshes a payment page, the system might receive two payment requests instead of one.

Idempotency ensures that repeated requests produce the same result as if the request were only made once.

### How it works

1. Each request is assigned a unique ID
2. Before processing the system checks if the request has already been handled
3. If yes, it ignores the duplicate request
4. If no, it processes the request normally
