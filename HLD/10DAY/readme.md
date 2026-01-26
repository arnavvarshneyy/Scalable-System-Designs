

Authentication and Authorization
Authentication (AuthN)
Authentication verifies who you are - it's the process of confirming a user's identity.

Common methods:
Username/password - Most basic form
Multi-factor authentication (MFA) - Combines multiple verification methods
Biometrics - Fingerprint, facial recognition
Token-based - JWT, OAuth tokens
Certificate-based - Digital certificates

Authorization (AuthZ)
Authorization determines what you can do - it specifies access rights and privileges after authentication.

Common models:
Role-Based Access Control (RBAC) - Permissions tied to roles
Attribute-Based Access Control (ABAC) - Uses attributes/policies
Discretionary Access Control (DAC) - Resource owners set permissions
Mandatory Access Control (MAC) - System-enforced policies

Key Differences----
Authentication    	Authorization
Verifies identity	Grants permissions
Comes first	        Comes after authn
"Who are you?"  	"What can you do?"





One-Factor Authentication (Single-Factor)
Definition: Uses only one method to verify identity (typically "something you know" like a password).

[User] → [Enter Password] → [System] → [Access Granted]
Diagram:
┌─────────────┐     ┌─────────────┐    ┌─────────────┐
│             │     │             │    │             │
│    User     │───▶ │  Password  │───▶│   System    │
│             │     │ (1 Factor)  │    │             │
└─────────────┘     └─────────────┘    └─────────────┘
Multi-Factor Authentication (MFA)
Definition: Requires two or more independent credentials from these categories:

Something you know (password, PIN)
Something you have (phone, security token)
Something you are (fingerprint, facial recognition)

[User] → [Enter Password] → [Enter SMS Code] → [System] → [Access Granted]
Diagram:
┌─────────────┐     ┌───────────────────┐    ┌─────────────┐
│             │     │                   │    │             │
│    User     │───▶ │    Password      │ ───▶│  System    │
│             │     │ (1st Factor)      │    │             │
└─────────────┘     │                   │    └─────────────┘
                    │  SMS Code/Token   │            ▲
                    │ (2nd Factor)      │            │
                    └───────────────────┘            │
                            ▲                        │
                            │                        │
                    ┌───────────────┐                │
                    │ Mobile Device │                │
                    │ or Biometric  │  ──────────────┘
                    └───────────────┘

Key Differences
Feature--       	One-Factor Auth 	Multi-Factor Auth
Security Level	        Low             	High
Common Methods	   Password only	    Password + SMS/Token/Biometric
Vulnerability	   Phishing attacks	    More resistant to attacks
User Convenience	  High	              Moderate
Compliance            Basic	            Required for many regulations


Session Management Overview
Session management tracks a user's interactions with a system across multiple requests. The approach differs significantly between stateful and stateless architectures.


Stateful Session Management
Characteristics:
Server maintains session state
Session data stored in memory or database
Typically uses session IDs (often in cookies)

Flow:
1. User Login → Server creates session → Stores session data → Returns session ID
2. Subsequent requests include session ID → Server validates ID → Retrieves session data

Diagram:-
┌─────────┐     ┌─────────┐      ┌──────────────┐
│         │     │         │      │              │
│ Client  │───▶│ Server   │───▶ │ Session Store│
│         │     │         │      │ (DB/Memory)  │
└─────────┘     └─────────┘      └──────────────┘
     ▲              │                 │
     │              └─────────────────┘
     └─────────────────┘
Pros:
Easy to implement
Can store complex user state
Native support in web servers

Cons:
Scalability challenges
Single point of failure
Server affinity required (hard to load balance)

Stateless Session Management--
Characteristics:
No server-side session storage
Client contains all necessary auth/session data
Typically uses signed tokens (JWT)

Flow:-
1. User Login → Server validates → Returns signed token
2. Client stores token → Includes in subsequent requests
3. Server verifies token signature → Processes request
Diagram:
┌─────────┐    ┌─────────┐
│         │    │         │
│ Client  │───▶│ Server  │
│ (Stores │    │ (Verifies│
│  Token) │    │  Token) │
└─────────┘    └─────────┘
Pros:
Excellent scalability
No server affinity needed
Works well with microservices

Cons:
Token revocation challenges
Larger request size
All session data must fit in token

Key Differences
Aspect                  	Stateful	            Stateless
Session Storage         	Server-side	            Client-side (token)
Scalability	                 Limited	             Excellent
Typical Use          	Traditional web apps	    APIs, microservices
Implementation	       Session IDs + storage    	JWT/OAuth tokens
Load Balancing      	Requires sticky sessions	No special requirements


1. Access Token
Short-lived (typically 15-60 minutes)
Contains user claims/privileges
Usually a JWT (JSON Web Token)
Sent with every API request

2. Refresh Token
Long-lived (days/weeks/months)
Stored securely (HTTP-only cookie or secure storage)
Used only to obtain new access tokens
Can be revoked server-side

AUTHENTICATION  FLOW-
1. User Login → Server validates credentials
2. Server issues:
   - Access Token (short-lived)
   - Refresh Token (long-lived, stored securely)
3. Client uses Access Token for API requests
4. When Access Token expires:
   - Client sends Refresh Token to /refresh endpoint
   - Server validates Refresh Token
   - Issues new Access Token
5. Repeat until:
   - Refresh Token expires
   - User logs out (server revokes Refresh Token)


┌───────┐       ┌────────┐       ┌───────┐
│       │       │        │       │       │
│ Client│       │ Server │       │  DB   │
│       │       │        │       │       │
└───┬───┘       └───┬────┘       └───┬───┘
    │               │                 │
    │─1.Login Credentials──────────▶ │
    │               │                 │
    │               │─2.Validate────▶│
    │               │                 │
    │◀─3. Access Token + Refresh─────│
    │               │                 │
    │─4. API Request (Access Token)─▶│
    │               │                 │
    │◀─5. Protected Data─────────────│
    │               │                 │
    │─6. Expired Token Error────────▶│
    │               │                 │
    │─7. Refresh Token──────────────▶│
    │               │                 │
    │               │─8. Validate────▶│
    │               │                 │
    │◀─9. New Access Token───────────│
    │               │                 │   



// AUTHORIZATION


1. Role-Based Access Control (RBAC)
Definition:
Access permissions are assigned to roles rather than individual users. Users are then assigned to appropriate roles.

Key Components:

Roles (e.g., Admin, Editor, Viewer)
Permissions (e.g., read, write, delete)
Users assigned to roles

How It Works:
User → Assigned Role → Inherits Permissions → Accesses Resources



2. Policy-Based Access Control (PBAC)
Definition:
Uses centralized policies that evaluate multiple attributes (user, resource, environment) to make access decisions.

Key Components:

Attributes (user department, time, location)
Policies (if-then rules combining attributes)
Policy Decision Point (evaluates requests)

How It Works:
Request → Policy Engine evaluates → Decision (Allow/Deny)


3. Access Control List (ACL)
Definition:
Explicit list specifying which users/systems can access particular resources and what operations they can perform.

Key Components:
Subjects (users, groups, systems)
Objects (files, databases, services)
Permissions (read, write, execute)

How It Works:
Resource ACL:
- UserA: Read, Write
- UserB: Read
- GroupX: Execute


Comparison Table
Feature	       RBAC	    PBAC	 ACL
Granularity	 Medium	    High	 Low-Medium
Flexibility 	Low	    High	 Medium
Scalability	   Good	    Moderate Poor
Complexity	Low-Medium	High	 Low
Best For	Organizations	    Dynamic systems 	Simple systems
Maintenance	 Role management	Policy updates	   Per-resource
Performance  	Fast	Slower	Fast

RBAC when:
You have clear organizational roles
Need simple, maintainable structure
Compliance requires role definitions

PBAC when:
Need dynamic, context-aware decisions
Have complex access requirements
Attributes frequently change

ACL when:
Managing small, simple systems
Need direct user-resource mapping
Working with traditional filesystems