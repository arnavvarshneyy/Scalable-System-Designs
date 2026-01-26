    1. CAP THEOREM 

     CAP Theorem (also called Brewer's Theorem) is a fundamental principle in distributed systems that states a distributed database can        only guarantee two out of three properties simultaneously:
     Consistency (C)
     Availability (A)
     Partition Tolerance (P)

    Since network partitions (P) are inevitable in distributed systems, the real trade-off is between Consistency (C) and Availability (A).

    The Three Properties----
      A. Consistency (C)
      Every read receives the most recent write or an error.
      All nodes see the same data at the same time (strong consistency).
     Example:
        If you write X=5 to Node A, a subsequent read from Node B must return 5.

     B. Availability (A)
      Every request receives a non-error response, but not necessarily the latest data.
      The system remains operational even if some nodes fail.
      Example:
        If Node A fails, Node B still serves requests (possibly stale data).

     C. Partition Tolerance (P)
      The system continues to operate despite network failures (dropped messages, delays, or node failures).
       Network partitions are unavoidable, so P is mandatory in distributed systems.


     The Three Possible Combinations--
     Since P (Partition Tolerance) is unavoidable, the real-world trade-off is between C (Consistency) and A (Availability).

         System Type	     Guarantees	                      Sacrifices	                      Examples
            CA     Strong consistency & always available   Fails under network partitions	  Single-node databases (MySQL on one server)

            CP     Strong consistency, tolerates partitions	May become unavailable during partitions	MongoDB, postgreSQL, ZooKeeper

           AP     Always available, tolerates partitions   May return stale data during partitions	Cassandra, DynamoDB , Riak


    2. Availability Numbers
        Availability is a critical metric in distributed systems, measured as the percentage of time a system is operational over a given          period. It’s often expressed as "nines" (e.g., 99.9%, "three nines").
                Availability Tiers (The "Nines")
       Availability (%)	Downtime per Year	  Tier Name	Use Cases
       90% (1 nine)     	~36.5 days	Basic	    Non-critical internal tools
       99% (2 nines)	    ~3.65 days	Moderate	Small business apps
       99.9% (3 nines)	    ~8.76 hours	High	    E-commerce, SaaS
       99.99% (4 nines)	~52.6 minutes	        Mission-critical	Cloud providers (AWS, Azure)
       99.999% (5 nines)	~5.26 minutes	        Fault-tolerant	Telecom, financial systems
       99.9999% (6 nines)	~31.5 seconds	        Ultra-reliable	Air traffic control, nuclear systems

      calculate:
        Availability=(1−DowntimeTotal Time)×100%



    3. Back of the Envelope Calculations (QUERY/SECOND AND STORAGE UNIT)
       Back-of-the-envelope calculations are rough, simplified estimates used to:
       ✔ Validate feasibility of a design
       ✔ Compare alternatives quickly
       ✔ Identify bottlenecks before deep diving


    4. MONOLITH AND MICROSERVICE

	                             Monolithic Architecture       	                  Microservices Architecture
    Definition	         Single, unified codebase handling all functions.  	     Decoupled services,each with specific purpose.
    Development Speed	✅ Faster initially (no cross-service coordination).	❌ Slower due to distributed complexity.
    Scalability      	❌ Scales as a whole (vertical scaling).	            ✅ Scales independently (horizontal scaling).
    Fault Isolation	    ❌ Single point of failure (crash affects all).	        ✅ One service failing doesn’t break others. 
    Deployment          ❌ Full redeploy for small changes.	                    ✅ Independent deployments per service.
    Tech Stack	        ❌ Locked into one language/framework.	                ✅ Polyglot (each service can use different tech).
    Database	        ✅ Single, consistent DB (ACID transactions).        	❌ Distributed DBs (eventual consistency).
    Latency          	✅ Low (in-process calls).	                            ❌ Higher (network calls between services).
    Debugging       	✅ Easier (centralized logs).                     	    ❌ Harder (distributed tracing needed).
    Team Structure	    ✅ Single team manages everything.	                    ✅ Cross-functional teams own services.
    Cost	            ✅ Lower (fewer servers, no service mesh).	            ❌ Higher (more infra, orchestration tools).


   5. Different phases of microservices
    .Decomposition (monolith service ko microservice mai kaise convert kroge)
    .Database (shared db use karoge ki unshared db)
    .Communication (communicate kaise krawoge different microservers ko )
    .Deplyoment (deploy kaise karoge)
    .Observability(monitor kaise kroge deploy ke baad)


  1.Decomposition
     -- by business logic 
     -- by subdomains


    How will you do it (MONOLITH TO MICROSERVICE)
    strangler pattern?
    Strangler Pattern: Incrementally Migrating from Monolith to Microservices

     The Strangler Pattern is a strategy to gradually replace a legacy monolith with microservices by "strangling" the old system piece by      piece. Inspired by strangler fig trees that slowly replace their host, this approach minimizes risk and avoids big-bang rewrites.
     How It Works 
        1. Identify a Module to Extract
           Target low-risk, high-value functionality (e.g., "user authentication," "payment processing").
           Choose modules with:
              Clear boundaries (minimal dependencies).
              Frequent changes (benefits from independent deploys).

        2. Build the New Service
           Reimplement the module as a standalone microservice.
           Use the same API contract as the monolith (for backward compatibility).

        3. Redirect Traffic Gradually
           Step 1: Proxy requests through an API Gateway or load balancer. 
           Route some traffic to the new service (e.g., 10% of users).
           Keep the monolith as a fallback.

           Step 2: Validate performance, then increase traffic to 50% → 100%.
  
        4. Retire the Monolith Component
           Once the new service handles 100% of traffic:
           Remove the old code from the monolith.
           Update the monolith to call the new service (if needed).



    2. Database problems (preferrable non sharable databases)
       solve problem of join 
       CQRS (Command Query Responsibility Segregation) - Deep Dive pattern 
         CQRS is an architectural pattern that separates reads (queries) from writes (commands) to optimize performance, scalability, and security. Unlike traditional CRUD, where the same model handles both reads and writes, CQRS uses different models for each operation.       

       solve problem of transaction management :
       SAGA PATTERN---
        The Saga Pattern is a critical design for handling long-running, distributed transactions across multiple microservices                    without relying on traditional ACID transactions (which don’t scale in distributed systems). It
        ensures data consistency using a sequence of local transactions and compensating actions.
        1. Why Sagas?
          Problem: Distributed Transactions 
          In microservices: 
            Each service has its own database.
            No 2PC (Two-Phase Commit): It’s slow and doesn’t scale.
            Network failures can leave systems inconsistent.

          Solution: Sagas
            Break a transaction into smaller steps (local transactions). 
            If any step fails, execute compensating actions (rollbacks).

        2. Saga Types
           A. Choreography-Based Saga
            Decentralized: Services communicate via events (e.g., Kafka).
            Each service triggers the next step or compensates.

            Pros:
             ✅ No single point of failure.
             ✅ Loosely coupled.
  
            Cons:
             ❌ Hard to debug (event chains).
             ❌ Complex recovery logic.

           B. Orchestration-Based Saga
            Centralized: A Saga Orchestrator (e.g., AWS Step Functions) manages the flow.
            Pros:
             ✅ Easier to track progress.
             ✅ Simpler error handling.
           
            Cons:
             ❌ Orchestrator is a bottleneck if not scaled.
 

         Example:
            Uber uses Sagas for ride matching (driver assignment → payment → notifications).
            Airbnb uses Sagas for booking (reserve → payment → confirm). 
  


