 
    CACHE
      Caching is a fundamental technique used to improve system performance by storing frequently accessed data in fast-access storage            layers. In high-level system design, caching plays a critical role in reducing latency, decreasing load on backend systems, and             improving overall user experience.

    Common Cache Locations in System Architecture---
     Client-side Caching: Browser cache, mobile app cache
     Content Delivery Network (CDN): Edge caching for static assets
     Application-level Caching: In-memory caches (Redis, Memcached)
     Database Caching: Query result caching, buffer pools
     OS-level Caching: File system caching, page caching  


    Cache Algorithms (Replacement Policies)
     When cache space is limited, these algorithms determine which items to evict:

    1. Least Recently Used (LRU) or Most recently used(reverse of LRU)
       Evicts the least recently accessed items first
       Implemented using a hash map + doubly linked list
       Time complexity: O(1) for both get and put operations
       Example: Memcached default, Redis approximate LRU

    2. Least Frequently Used (LFU)
       Evicts the least frequently accessed items
       Tracks access frequency for each item
       More complex to implement than LRU
       Example: Used in some database caching systems

    3. First-In-First-Out (FIFO)
       Evicts items in the order they were added
       Simple queue implementation
       Doesn't consider access patterns

    4. Random Replacement (RR)
       Randomly selects items to evict
       Simple to implement but unpredictable performance

    5. Time-To-Live (TTL)
       Items expire after a fixed time period
       Often combined with other algorithms
       Example: Web page caching


     Cache Considerations in HLD
      Consistency Models:
      Strong consistency vs eventual consistency
      Cache invalidation strategies

     Cache Size:
      Too small: Low hit rate
      Too large: Memory pressure, longer eviction times

     Cache Key Design:
      Proper namespace organization
      Avoiding hot keys

     Distributed Caching:
       Replication strategies
       Partitioning/sharding approaches
       Handling cache misses


    Read Policies--

     1. Cache-Aside (Lazy Loading)
     Behavior: Application checks cache first, loads from DB if missing
     Pros: Simple, only caches requested data
     Cons: Cache miss penalty, potential for stale data
     Use Case: General purpose, read-heavy workloads

     2. Read-Through
     Behavior: Cache automatically loads from DB on misses
     Pros: Cleaner application code, cache handles misses
     Cons: Requires cache to understand DB schema
     Use Case: When using intelligent caching systems

    Write Policies---

     1. Write-Through
     Behavior: Writes to cache and DB simultaneously
     Pros: Strong consistency, cache always up-to-date
     Cons: Higher write latency (waits for DB)
     Use Case: Systems requiring strong consistency

     2. Write-Behind (Write-Back)
     Behavior: Writes to cache first, asynchronously to DB
     Pros: Low write latency, can batch DB writes
     Cons: Risk of data loss if cache fails, eventual consistency
     Use Case: Write-heavy systems tolerant of some inconsistency

     3. Write-Around
     Behavior: Writes directly to DB, bypassing cache
     Pros: Avoids flooding cache with write-only data
     Cons: Subsequent reads will miss cache
     Use Case: Write-heavy data that's rarely read




