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

Replication improves the read performance since read requests are spread across multiple replicas recuding the load on each one.
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
    - It also reduces unnecessary disk I/O making daa retrieval quicker

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
