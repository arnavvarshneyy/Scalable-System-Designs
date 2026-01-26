

   // QUESTION 1. DATABASE AND TYPES 
   //ANSWER
   A database is an organized collection of structured data stored electronically in a computer system. Databases allow for efficient data    management, storage, retrieval, and manipulation.
   DBMS is software that interacts with users to process data.
   Main Types of Databases
   1. Relational Databases (RDBMS)
     Organize data into tables with rows and columns
     Use SQL (Structured Query Language) for queries
     Enforce ACID properties (Atomicity, Consistency, Isolation, Durability)
     Examples: MySQL, PostgreSQL, Oracle, Microsoft SQL Server, SQLite

   2. NoSQL Databases
     Designed for unstructured or semi-structured data
     More flexible schema than relational databases
     Types:
        Document Databases: Store data in JSON-like documents (MongoDB, CouchDB)
        Key-Value Stores: Simple pairs of keys and values (Redis, DynamoDB)
        Column-Family Stores: Organize data in columns rather than rows (Cassandra, HBase)
        Graph Databases: Store data in nodes and edges (Neo4j, Amazon Neptune)


    //QUESTION2. WHICH DATABSE TO USE HOW TO DEDCIDE??
    //ANSWER
      1. Data Structure
      2. Scalability Needs
      3. Performance Requirements
      4. Consistency vs Availability
      5. Transaction Requirements

    //QUESTION3. LOAD BALANCER AND MASTER SLAVE ARCHITECTURE
    //ANSWER:
     A load balancer is a critical component that distributes network or application traffic across multiple servers to improve                 responsiveness, increase availability, and prevent server overload.

     A database architecture pattern where:
       Master node: Handles all write operations
       Slave nodes: Handle read operations (replicas of master)

     How It Works:
       All writes go to the master database
       Changes are replicated to slave databases
       Read requests are distributed among slaves

    Benefits:
       Improved performance: Read queries distributed
       High availability: If master fails, a slave can be promoted
       Scalability: Can add more slaves for read-heavy workloads

    Challenges:
      Replication lag: Slaves may have slightly stale data
      Single point of failure: The master node
      Write bottleneck: All writes must go through master


  
    //QUESTION 4. What is Database Caching and CDN ?
    Database caching is a technique that stores frequently accessed data in temporary, high-speed storage (cache) to reduce the need to        repeatedly retrieve the same data from the primary database.    

    A Content Delivery Network (CDN) is a geographically distributed network of servers that work together to provide fast delivery of         Internet content (like web pages, images, videos, and other static assets) by serving content from locations closest to end users.

    How CDNs Work:::
       User Request: When a user requests content from a website
       DNS Resolution: The request is routed to the nearest CDN edge server
       Content Delivery:
         If cached: Served directly from the edge server
         If not cached: Retrieved from origin server, cached, then delivered
       Optimized Routing: Uses intelligent routing algorithms for fastest delivery


       [Users]
         ↓
    [Geo-DNS (Route53/Cloudflare)]
         ↓
    [Regional Load Balancer] 
         ↓ 
    [Auto-Scaled Web Tier] → [Messaging Queue (Kafka/SQS)] → [Auto-Scaled Workers]
         ↓                                                   ↓
    [Distributed Cache (Redis)]                          [Sharded Databases]
         ↓
    [CDN (For Static Assets)]


    //Auto Scaling of servers
    1. Auto Scaling of Servers

    Purpose: Automatically adjust computing resources based on demand
    Implementation:
      Horizontal scaling (adding/removing instances)
      Vertical scaling (resizing instances)
      Cloud providers offer auto-scaling groups (AWS ASG, Azure VM Scale Sets)
      Metrics: CPU utilization, request rate, queue length
      Cool-down periods to prevent thrashing



     2. Data centre and Geo Enabled DNS
     Purpose: Reduce latency and improve availability by routing users to nearest servers
     Components:
      Multiple data centers in different regions
      DNS-based routing (GeoDNS)
      Anycast routing for IP addresses
      CDN integration

     Example Flow:
       User in London queries your domain
       GeoDNS resolves to EU-west servers
       Anycast routes to nearest edge location
       Request served from Dublin data center


    3. Messaging Queue Model
    Purpose: Decouple components and handle asynchronous processing
    Common Patterns:
       Producer/consumer model
       Pub/Sub (Publisher/Subscriber)
       Point-to-point queues
       Dead letter queues for failed messages

    Popular Technologies:
       RabbitMQ, Apache Kafka, AWS SQS, Azure Service Bus

    Example Architecture-
      [Web Servers] → [Message Queue] → [Worker Processes] → [Database]

    //sharding
    Purpose: Horizontal partitioning of data across multiple databases
