


   //QUESTION 1. WHY HLD??
    High-Level Design (HLD) is important because it provides a broad overview of a system’s architecture and components before detailed        implementation begins. HLD helps to:

    Define the system’s structure and main modules.
    Identify major technologies, interfaces, and data flow.
    Align stakeholders on the system’s approach and boundaries.
    Reduce risks by exposing design flaws early.
    Serve as a blueprint for Low-Level Design (LLD) and development.


   //QUESTION 2. CLIENT-SERVER MODEL OR REQUEST-RESPONSE MODEL
    The client-server model is a distributed computing architecture that divides tasks between service providers (servers) and service         requesters (clients). This model is fundamental to modern networked applications, including the web, email, and databases.

    Key Concepts:
     Client:
        Requests services or resources from the server.
        Examples: Web browsers (Chrome, Firefox), email clients (Outlook), mobile apps.
     Server:
        Provides services, data, or computation upon request.
        Examples: Web servers (Apache, Nginx), database servers (MySQL), game servers.
     Communication:
        Clients and servers interact over a network (e.g., the Internet) using protocols like:
          .  HTTP/HTTPS (Web)
          .  SMTP/POP3/IMAP (Email)
          .  TCP/IP, UDP (General networking)

   How It Works:
    . The client sends a request (e.g., loading a webpage, fetching an email).
    . The server processes the request and sends back a response (e.g., webpage data, email content).
    . The client receives and displays/processes the response.

   Advantages:
    ✔ Centralized Management – Servers control access, security, and updates.
    ✔ Scalability – Servers can handle multiple clients simultaneously.
    ✔ Efficiency – Clients offload heavy processing to servers.

   Disadvantages:
    ✖ Single Point of Failure – If the server crashes, clients lose access. 
    ✖ Network Dependency – Requires stable connectivity.
    ✖ Server Overload – Too many clients can slow down performance.


   //QUESTION 3. HORIZONTAL SCALING AND VERTICAL SCALING
    1. Horizontal Scaling (Scale-Out)
    Definition: Adding more servers/nodes to distribute the workload.

    How It Works:
     Instead of making a single server more powerful, multiple smaller servers work together.
     Uses load balancers to distribute requests across servers.
     Often implemented in cloud computing (e.g., AWS Auto Scaling, Kubernetes).

   Pros:
    ✔ High Availability – If one server fails, others take over.
    ✔ Better Fault Tolerance – No single point of failure.
    ✔ Handles Traffic Spikes – Easily add more servers when demand increases.
    ✔ Cost-Effective for Cloud – Pay only for what you use.
   Cons:
    ✖ Increased Complexity – Requires distributed systems (e.g., databases, caching).
    ✖ Network Overhead – Servers must communicate efficiently.
    ✖ Software Must Be Stateless – Sessions must be managed externally (e.g., Redis).
  
   Use Cases:
    Web Applications (e.g., Netflix, Facebook)
    Microservices Architectures
    Big Data Processing (Hadoop, Spark)

   2. Vertical Scaling (Scale-Up)
    Definition: Increasing the power of an existing server (CPU, RAM, storage).

    How It Works:
     Upgrade hardware (e.g., from 8GB RAM → 64GB RAM).
     No need for load balancing since it’s a single machine.

   Pros:
    ✔ Simpler Architecture – No distributed system challenges.
    ✔ No Software Changes Needed – Works with existing monolithic apps.
    ✔ Better for Single-Threaded Apps – Some apps can’t be parallelized.
   Cons:
    ✖ Limited by Hardware – Eventually, you hit physical limits.
    ✖ Single Point of Failure – If the server crashes, the system goes down.
    ✖ Expensive Upgrades – High-end servers cost more than multiple smaller ones.

   Use Cases:
     Database Servers (MySQL, PostgreSQL on a powerful machine)
     Legacy Applications (Not designed for distributed computing)
     High-Performance Computing (HPC)



   //QUESTION 4. DISTRIBUTED SYSTEMS
    A distributed system is a collection of independent computers that work together as a single coherent system. These machines               communicate over a network to achieve a common goal, such as processing large-scale data, providing high availability, or improving        performance.

    Key Characteristics:
    ✔ Multiple Nodes – Composed of multiple independent computers (nodes).
    ✔ Concurrency – Tasks run in parallel across different machines.
    ✔ No Shared Clock – Nodes operate without a global time source.
    ✔ Fault Tolerance – The system continues working even if some nodes fail.
    ✔ Transparency – Users perceive the system as a single entity (e.g., Google Search).

   Why Use Distributed Systems?
    Scalability – Handle more load by adding more machines (horizontal scaling).
    High Availability – If one node fails, others take over (fault tolerance).
    Performance – Distribute computation closer to users (e.g., CDNs).
    Geographic Distribution – Serve users worldwide with low latency.


   //QUESTION 4. NETWORK COMMUNICATION AND NETWORK PROTOCOLS
     Network communication refers to the exchange of data between devices (computers, servers, IoT devices, etc.) over a network. This          communication is governed by network protocols, which are standardized rules that define how data is transmitted, received, and            processed.

     Protocols ensure structured and reliable communication. They are often organized into layers (as in the OSI and TCP/IP models).

     A. OSI Model (7 Layers)

     Layer	Name	          Function	                        Example Protocols
     7	Application 	User interface, network services	HTTP, FTP, SMTP, DNS
     6	Presentation	Data translation, encryption     	SSL/TLS, JPEG, MPEG
     5	Session	         Manages connections	            NetBIOS, RPC
     4	Transport    	End-to-end communi, reliability	    TCP, UDP
     3	Network     	Logical addressing, routing	        IP, ICMP, OSPF
     2	Data Link	    Physical addressing,error detection	Ethernet, PPP, Wi-Fi (802.11)
     1	Physical	    Raw bit transmission over media	    USB, Fiber, Coaxial

     B. TCP/IP Model (4 Layers)
     Layer	           Protocols                     	Function
     Application 	HTTP, FTP, SMTP, DNS	     User-level data exchange
     Transport	    TCP (reliable), UDP (fast)	 End-to-end communication
     Internet    	IP, ICMP, ARP	             Routing & addressing
     Network Access	Ethernet, Wi-Fi, PPP	     Physical transmission

     3. Key Network Protocols
    A. Application Layer Protocols
      HTTP/HTTPS – Web browsing (HTTP = unencrypted, HTTPS = encrypted with SSL/TLS).
      FTP (File Transfer Protocol) – File transfers.
      SMTP (Simple Mail Transfer Protocol) – Email sending.
      DNS (Domain Name System) – Converts domain names to IP addresses.
      DHCP (Dynamic Host Configuration Protocol) – Assigns IP addresses dynamically.

    B. Transport Layer Protocols
    TCP (Transmission Control Protocol)
        Connection-oriented, reliable (retransmits lost packets).
        Used in web browsing (HTTP), email (SMTP), file transfers (FTP).

    UDP (User Datagram Protocol)
        Connectionless, fast but unreliable (no retransmission).
        Used in video streaming (VoIP, YouTube), DNS queries, online gaming.

