
    Rate Limiting: Concepts, Algorithms, and Implementation
      Rate limiting controls how often a user, service, or API can make requests within a specified time window. It prevents abuse, ensures       fair usage, and protects systems from overload (e.g., DDoS attacks).

   1. Why Use Rate Limiting?
      ✅ Prevents API abuse (e.g., brute-force attacks, scraping)  
      ✅ Protects backend services from being overwhelmed
      ✅ Ensures fair usage among clients
      ✅ Helps manage quotas (e.g., free vs. premium API tiers)

   2. Common Rate Limiting Algorithms
    A. Fixed Window (Simple Counter)
      How it works:
            Limits requests per fixed time window (e.g., "100 requests per minute").
            Resets counter after the window expires.
      Pros: Simple to implement.
      Cons: Burst traffic at window edges can exploit it.
      Example:
           User A makes 100 requests at 00:59, then another 100 at 01:00 → 200 requests in 2 seconds!

    B. Sliding Window Log
    How it works:
          Tracks timestamps of all requests in a log.
          Counts only requests within the current sliding window.
    Pros: More accurate than fixed window.
    Cons: Memory-heavy (stores all timestamps).
    Example:
          If the limit is 100 requests/minute, only requests from the last 60 seconds are counted.

    C. Sliding Window Counter (Hybrid)
     How it works:
           Combines Fixed Window + Sliding Log efficiency.
           Estimates current window traffic by overlapping previous/current windows.
     Pros: Balances accuracy and performance.
     Cons: Slightly complex implementation.
     Example:
          If 50 requests were made in the last 30 sec, and 60 requests in the current 30 sec, the effective rate is 110/60 sec.

    D. Token Bucket
    How it works:
          A "bucket" holds N tokens (e.g., 100 tokens = 100 requests).
          Each request consumes 1 token.  
          Tokens refill at a fixed rate (e.g., 10 tokens/sec).
    Pros: Allows bursts (if tokens are available).
    Cons: Requires managing token state.
     Example:
          A user can send 50 requests in 1 sec if tokens are available, then must wait for refill.

    E. Leaky Bucket
     How it works:  
           Requests enter a "bucket" at a variable rate but exit at a fixed rate.
           If the bucket overflows, requests are dropped/throttled.
     Pros: Smooths out traffic spikes.
     Cons: No burst allowance (unlike Token Bucket).
     Example:
           API processes 10 requests/sec max, even if users send 50 requests/sec.


     Summary
        Algorithm	      Best For	      Burst Handling	Complexity    
      Fixed Window 	    Simple APIs	          ❌ No	       Low
      Sliding Log	    Accurate tracking	      ❌ No	       High
      Sliding Counter	Balanced approach       ✅ Yes	     Medium
      Token Bucket	Bursty traffic	          ✅ Yes	     Medium
      Leaky Bucket	Smoothing traffic	        ❌ No	       Medium
