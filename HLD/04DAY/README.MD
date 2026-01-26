
    Sharding Using Hash Functions----
    Hash-based sharding is a common technique for distributing data across multiple database shards by applying a hash function to a shard     key value.

    How Hash Sharding Worksp-----
      Key Selection: Choose a column or set of columns as your shard key (e.g., user_id, order_id)
      Hash Application: Apply a consistent hash function to the shard key value
      Shard Assignment: Use the hash output to determine which shard gets the data (typically via modulo operation)

    Challenges and Limitations--
    Resharding Complexity:
       Changing number of shards requires rehashing all keys ("modulo N" problem)
       Consistent hashing can help mitigate this
    Query Limitations: 
       Queries that do not include the sharding key may require broadcasting to all shards, reducing efficiency.
    Joins and Transactions: 
       Performing joins or transactions across shards is challenging and can impact consistency and performance.
    Operational Overhead: 
      Managing and monitoring shards, especially as the system scales, adds operational complexity.


     CONSISTENT HASHING (TO SOLVE THE PROBLEM OF DATABASE MIGRATION)
        Consistent hashing is a distributed hashing technique that minimizes data movement when nodes (shards) are added or removed from a         system. It is widely used in distributed databases, caching systems (like Memcached, Redis), and load balancers.

     1. Problem with Traditional Hashing
       Traditional hashing (e.g., hash(key) % N) has two major drawbacks:
       Resharding Overhead: When the number of nodes (N) changes, most keys are remapped, leading to massive data movement.
       Hotspots: Uneven distribution can cause some nodes to be overloaded.

     2. How Consistent Hashing Works
       2.1 Basic Concept
       Instead of mapping keys directly to nodes, keys and nodes are mapped onto a hash ring (a circular space).
       each node is assigned multiple virtual nodes (or "tokens") on the ring.
       A key is assigned to the next closest node in the clockwise direction.

       2.2 Key Components
         Component	    Description
         Hash Ring	    A circular space (e.g., 0 to 2^160 - 1 for SHA-1)
        Virtual Nodes	  Multiple points per physical node to ensure uniform distribution 
        Replication	    Keys may be stored on multiple adjacent nodes for fault tolerance

       2.3 Example Workflow
        Hash nodes and keys onto the ring (e.g., using SHA-1).
        Key lookup: Find the first node whose hash is ≥ the key’s hash (clockwise).
        Adding/Removing a node only affects neighboring keys, not the entire dataset.

        3. Benefits of Consistent Hashing
        ✅ Minimal Data Movement
           Only K/N keys need remapping when a node is added/removed (where K = keys, N = nodes).
        ✅ Load Balancing
           Virtual nodes ensure keys are evenly distributed.
        ✅ Scalability
           Adding/removing nodes does not require full rehashing. 
        ✅ Fault Tolerance
           Replicas on adjacent nodes provide redundancy.

        4. Challenges & Solutions
            Challenge                	Solution
      Non-Uniform Distribution	Use many virtual nodes per physical node
            Hotspots	                Dynamic replication based on load
         Node Failures	            Replicate data to next N nodes in the ring 
         Locality Issues         	Rendezvous hashing (HRW) for better key distribution


     5. When to Use Consistent Hashing?
      ✔ Distributed Caches (Redis, Memcached)
      ✔ Database Sharding (Cassandra, DynamoDB)
      ✔ CDN Load Balancing
      ✔ Microservices Request Routing

      ❌ Not ideal for range queries (use range-based sharding instead).


     Application of load balancer--
      scalability
      availability
      seccurity(dos ddos)
       responsiveness



    Static Load Balancing Algorithms
       Static load balancing distributes traffic across servers using predefined rules without real-time performance monitoring. These            algorithms are simple, predictable, and best suited for stable workloads.

    1. Round Robin (RR)
       How it works: Distributes requests sequentially across servers in rotation.
       Pros: Simple, fair distribution.
       Cons: Doesn’t account for server load or capacity.
       Use Case: Homogeneous server environments (e.g., web servers with equal specs).

    2. Weighted Round Robin (WRR)
       How it works: Like Round Robin, but assigns weights to servers (e.g., Server A gets 3 requests,  Server B gets 1).
       Pros: Accounts for server capacity differences.
       Cons: Weights must be manually configured.
       Use Case: Mixed-capacity servers (e.g., cloud VMs with varying CPU power).

    3. IP Hash
       How it works: Uses client IP address to determine which server handles the request (same IP → same server).
       Pros: Session persistence (useful for stateful apps).
       Cons: Uneven distribution if IPs are clustered.
       Use Case: Applications requiring sticky sessions (e.g., e-commerce carts).

    When to Use Static Load Balancing?
      ✔ Predictable traffic patterns
      ✔ Servers with similar specs
      ✔ Simple, low-overhead systems
      ❌ Not ideal for highly dynamic cloud environments (use dynamic LB instead).



     Dynamic Load Balancing Algorithms
       Dynamic load balancing adjusts traffic distribution in real-time based on current server conditions (CPU, memory, latency, etc.).          Unlike static methods, these algorithms continuously monitor and optimize performance.

     1. Least Response Time
      How it works: Routes traffic to the server with the fastest response time.
      Pros: Optimizes user experience by minimizing latency.
      Cons: Requires constant health checks.
      Use Case: Real-time applications (gaming, video calls).

     2. Weighted Least Connections
     How it works: Combines Least Connections with server weights (CPU/RAM capacity).
     Pros: Balances load while considering server power.
     Cons: Needs real-time metrics.
     Use Case: Cloud auto-scaling groups (AWS, GCP).

     3. Resource-Based (Adaptive)
     How it works: Uses live metrics (CPU, memory, disk I/O) to assign traffic.
     Pros: Maximizes resource utilization.
     Cons: Overhead from monitoring.
     Use Case: High-performance computing (HPC), big data.

     When to Use Dynamic Load Balancing?
     ✔ Unpredictable traffic (e.g., viral content)
     ✔ Cloud auto-scaling environments
     ✔ Applications requiring low latency
     ✔ Systems with varying server health

     Advanced Load Balancer Types: 
      Application, Network, Global, and DNS-Based
      Here's a breakdown of specialized load balancer types beyond traditional static/dynamic algorithms:

     1. Application Load Balancer (Layer 7)
      Works at: 
         HTTP/HTTPS layer (Application Layer)
      Key Features:
         Routes traffic based on content (URL paths, headers, cookies).
         Supports SSL termination (decrypts traffic at LB).
         Enables host-based and path-based routing.
      Use Cases:
         Microservices (e.g., /api/users → Service A, /api/orders → Service B).
         Web apps with multiple backend services.
      Examples:
         AWS ALB, NGINX Plus, F5 BIG-IP.

      2. Network Load Balancer (Layer 4)
       Works at: 
         TCP/UDP layer (Transport Layer)
       Key Features:
         Ultra-low latency (no HTTP parsing).
         Handles millions of requests/sec (used for high-throughput systems).
         Preserves source IP (unlike some L7 LBs).
       Use Cases:
         Gaming servers, VoIP, IoT.
         High-performance databases (e.g., Redis, MySQL clusters).
        Examples:
        AWS NLB, Google Cloud TCP/UDP LB, HAProxy in TCP mode.

     3. Global Server Load Balancer (GSLB)
      Works at: 
        DNS level (multi-region)
      Key Features:
        Routes users to the nearest/healthiest data center.
        Uses geo-IP, latency checks, and health probes.
        Provides failover if a region goes down. 
      Use Cases:
         Multi-cloud/geo-distributed apps (e.g., Netflix, Zoom).
         Disaster recovery setups.
      Examples:
         cloudflare Load Balancer, AWS Route 53 (with latency routing), Akamai GTM.

     4. DNS Load Balancer
      Works at:
         DNS layer (simple round-robin)
      Key Features:
         Distributes traffic via DNS A/AAAA record rotation.
         No deep packet inspection (unlike GSLB).
         Minimal overhead but no real-time health checks.
      Use Cases:
         Simple failover setups.
         Basic round-robin distribution for static sites.
      Examples: 
        Basic DNS providers (GoDaddy, Namecheap).
        Not ideal for dynamic apps (lacks smart routing).

    5. Content-Aware Load Balancer (Advanced L7)
     Works at:
       Application Layer (with deep inspection)
     Key Features:
      Reads API payloads (e.g., JSON/XML) for routing.
      Can throttle/block malicious requests.
     Use Cases:
       API gateways (e.g., GraphQL, REST APIs).
       Fraud detection systems.
     Examples:
      Kong, Apigee, Traefik.



      6. Hardware vs. Software Comparison
       Feature   	     Hardware LB	               Software LB
      Performance	   ✅ 10M+ RPS	                ✅ 1M-5M RPS (depends on host)
        Cost	       ❌ $$$$ (CAPEX)        	    ✅ $ (OPEX)
      Scalability	   ❌ Vertical only             ✅ Horizontal (auto-scaling)
       Security	     ✅ Hardware-accelerated SSL	❌ Software-based
      Cloud Support  ❌ Limited	                  ✅ Native (AWS/GCP/Azure)
      Maintenance	   ❌ Firmware updates	        ✅ Zero-downtime updates
