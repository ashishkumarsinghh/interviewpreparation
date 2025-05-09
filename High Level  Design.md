# Ace Your FANG High-Level Design Interview: Expert Guide & Top Problems

Welcome to the ultimate guide for conquering High-Level Design (HLD) interviews at FANG and other top tech companies. As an interview expert, I'll walk you through a structured approach, common problems, and the critical thinking interviewers expect.

## I. The Expert Framework for HLD Interviews

Every HLD problem, regardless of its specifics, can be tackled with a consistent framework. Interviewers are evaluating your thought process, ability to handle ambiguity, and understanding of trade-offs, not just a single "correct" answer.

**1. Understand and Clarify Requirements (5-10 minutes)**
   *   **Ask clarifying questions:** Don't assume anything.
       *   What are the primary use cases?
       *   Who are the users?
       *   What is the expected scale (e.g., users, requests/second, data storage)?
       *   What are the key functional requirements?
       *   What are the critical non-functional requirements (NFRs)? (Latency, Availability, Consistency, Durability, Scalability, Security, Cost-effectiveness)
   *   **Define scope:** What features are essential (MVP)? What can be deferred?
   *   **Identify constraints:** Any specific technologies to use or avoid? Time/resource limits?

**2. High-Level System Design (10-15 minutes)**
   *   **Sketch a block diagram:** Identify major components (e.g., Load Balancers, Web Servers, App Servers, Databases, Caches, Message Queues, CDNs, Microservices).
   *   **Define APIs:** How will components interact? (e.g., REST, gRPC). Define key API endpoints for core functionalities.
   *   **Data Model Design:** How will data be stored? (SQL vs. NoSQL, schema design).

**3. Deep Dive into Components & Design Choices (15-20 minutes)**
   *   For each major component:
       *   Discuss its role and responsibilities.
       *   Explain technology choices (e.g., "Why NoSQL here? Which NoSQL DB and why?").
       *   Address NFRs: How does this component contribute to scalability, availability, etc.?
   *   **Data Storage:**
       *   Choice of database (SQL, NoSQL - Key-Value, Document, Columnar, Graph). Justify.
       *   Sharding/Partitioning strategy for scalability.
       *   Replication for availability and read scaling.
       *   Consistency model (Strong, Eventual).
   *   **Caching:**
       *   Where to introduce caches (Client-side, CDN, Server-side - DB cache, Application cache, Distributed cache like Redis/Memcached).
       *   Caching strategy (e.g., Write-through, Write-around, Write-back, Cache-aside).
       *   Eviction policies.
   *   **Scalability & Performance:**
       *   Load balancing strategies (Round Robin, Least Connections, etc.).
       *   Horizontal vs. Vertical scaling for different components.
       *   Asynchronous processing (Message Queues like Kafka, RabbitMQ).
   *   **Availability & Reliability:**
       *   Redundancy (multiple instances of services, data replication).
       *   Failover mechanisms.
       *   Monitoring and alerting.
       *   Data backup and recovery.

**4. Identify Bottlenecks and Trade-offs (5-10 minutes)**
   *   Proactively identify potential bottlenecks in your design (e.g., single points of failure, database hotspots).
   *   Discuss how you might address them.
   *   Explicitly state the trade-offs made (e.g., "Chose eventual consistency for higher availability and lower latency, sacrificing strong consistency"). CAP theorem is often relevant here.

**5. Further Considerations & Future Enhancements (Optional, if time permits)**
   *   Security aspects (Authentication, Authorization, Data encryption).
   *   Analytics and monitoring.
   *   Cost optimization.
   *   Specific features for V2 or future iterations.

**Interviewer's Focus:**
*   **Structured Thinking:** Can you break down a complex problem?
*   **Communication:** Can you articulate your ideas clearly?
*   **Technical Depth:** Do you understand the fundamentals of distributed systems?
*   **Pragmatism:** Do you make reasonable choices and understand trade-offs?
*   **Handling Ambiguity:** Can you make sensible assumptions and state them?
*   **Collaboration:** Do you engage with the interviewer and incorporate feedback?

---

## II. Core Concepts, Technologies & Decision Choices in HLD

Understanding the fundamental building blocks, common technologies, and critical decision points is crucial for success in HLD interviews. This section provides an overview of these core areas.

**A. Key Decision Choices & Trade-offs**

Every design involves trade-offs. Articulating these is key.

*   **Consistency vs. Availability (CAP Theorem):** In a distributed system, you can only choose two out of Consistency, Availability, and Partition Tolerance. Since network partitions are a given, the trade-off is usually between strong consistency and high availability.
*   **Latency vs. Throughput:** Optimizing for low latency (quick response for a single request) might impact overall throughput (total requests handled per unit time), and vice-versa.
*   **SQL vs. NoSQL:**
    *   **SQL:** Structured data, ACID properties, strong consistency, joins. Good for complex queries and transactional integrity.
    *   **NoSQL:** Flexible schema, horizontal scalability, eventual consistency (often). Better for high volume, unstructured/semi-structured data, or specific access patterns (e.g., key-value, document).
*   **Normalization vs. Denormalization:**
    *   **Normalization:** Reduces data redundancy, improves data integrity. Can lead to more joins and slower reads.
    *   **Denormalization:** Improves read performance by adding redundant data, reducing joins. Can lead to data anomalies and increased storage.
*   **Build vs. Buy:** Decide whether to build a component from scratch or use an existing managed service/off-the-shelf solution. Consider development time, cost, expertise, and customization needs.
*   **Synchronous vs. Asynchronous Communication:**
    *   **Synchronous:** Sender waits for a response. Simpler, but can lead to blocking and higher latency.
    *   **Asynchronous:** Sender doesn't wait; processing happens in the background (e.g., via message queues). Improves responsiveness and resilience.
*   **Push vs. Pull Models:**
    *   **Push:** Server proactively sends data to clients. Good for real-time updates.
    *   **Pull:** Client requests data from the server. Simpler for clients, but can lead to polling overhead.

**B. Essential Technologies & Strategies**

**1. Data Storage**

*   **Relational Databases (SQL):**
    *   **When to use:** Structured data, ACID compliance needed, complex relationships, well-defined schema.
    *   **Examples:** PostgreSQL, MySQL, Oracle, SQL Server.
    *   **Properties:** ACID (Atomicity, Consistency, Isolation, Durability).
*   **NoSQL Databases:**
    *   **Key-Value Stores:**
        *   **Examples:** Redis, Amazon DynamoDB (can also be used as a document DB), Memcached.
        *   **Use Cases:** Caching, session management, user profiles, real-time leaderboards. Simple data model, extremely fast reads/writes.
    *   **Document Stores:**
        *   **Examples:** MongoDB, Couchbase, Amazon DocumentDB.
        *   **Use Cases:** Content management, product catalogs, user profiles where data is self-contained documents (e.g., JSON, BSON, XML). Flexible schema.
    *   **Columnar (Wide-Column) Stores:**
        *   **Examples:** Apache Cassandra, HBase, Google Bigtable.
        *   **Use Cases:** Big data analytics, time-series data, write-heavy workloads, applications requiring high availability and scalability across many servers. Optimized for queries over columns.
    *   **Graph Databases:**
        *   **Examples:** Neo4j, Amazon Neptune.
        *   **Use Cases:** Social networks, recommendation engines, fraud detection, knowledge graphs. Efficient for managing and querying highly connected data.
*   **Data Warehousing:** Centralized repository for large amounts of structured data from various sources, optimized for querying and analysis (OLAP). Examples: Snowflake, Amazon Redshift, Google BigQuery.
*   **Data Lakes:** Store vast amounts of raw data in its native format. Used for big data processing, machine learning. Examples: AWS S3-based data lakes, Azure Data Lake Storage.

**2. Caching Strategies & Technologies**

*   **Purpose:** Reduce latency, decrease load on backend systems, improve throughput.
*   **Cache Locations:**
    *   **Client-side Cache:** Browser cache, mobile app cache.
    *   **CDN (Content Delivery Network):** Caches static assets (images, videos, JS, CSS) at edge locations globally.
    *   **Server-side Cache:**
        *   **In-memory Cache:** Within an application server (e.g., Guava Cache, Ehcache). Fast but limited by server memory and not shared.
        *   **Distributed Cache:** External cache shared by multiple servers (e.g., Redis, Memcached).
*   **Caching Strategies:**
    *   **Cache-Aside (Lazy Loading):** Application code first checks the cache. If miss, loads from DB, then populates cache. Most common.
    *   **Read-Through:** Application treats cache as main data store. Cache itself handles DB reads on miss.
    *   **Write-Through:** Data written to cache and DB simultaneously (or cache first, then DB synchronously). Ensures consistency, but higher write latency.
    *   **Write-Back (Write-Behind):** Data written to cache only. Cache asynchronously writes to DB later. Fast writes, but risk of data loss if cache fails before DB write.
    *   **Write-Around:** Data written directly to DB, bypassing cache. Cache populated on read miss. Good for write-heavy, rarely read data.
*   **Cache Eviction Policies:** LRU (Least Recently Used), LFU (Least Frequently Used), FIFO (First-In, First-Out), MRU (Most Recently Used).
*   **Common Caching Technologies:**
    *   **Redis:** In-memory data structure store, used as a database, cache, and message broker. Supports various data structures (strings, hashes, lists, sets, sorted sets). Offers persistence.
    *   **Memcached:** High-performance, distributed memory object caching system. Simpler than Redis, purely a volatile cache.

**3. Sharding & Data Partitioning**

*   **Why Shard?** To distribute data across multiple servers/databases when a single server cannot handle the load (storage, read/write traffic). Improves scalability and performance.
*   **Partitioning Schemes:**
    *   **Horizontal Partitioning (Sharding):** Divides a table into multiple smaller tables (shards) with the same schema, but different rows. Rows are distributed based on a shard key.
    *   **Vertical Partitioning:** Divides a table into multiple tables with fewer columns. Some columns go to one table, others to another, linked by a common key.
    *   **Directory-Based Partitioning:** A lookup service (directory) keeps track of where specific data shards reside.
*   **Sharding Strategies (for Horizontal Partitioning):**
    *   **Range-Based Sharding:** Data divided based on ranges of the shard key (e.g., User IDs 1-1000 on Shard A, 1001-2000 on Shard B). Can lead to hotspots if data isn't evenly distributed.
    *   **Hash-Based Sharding:** Shard key is hashed, and the hash value determines the shard. Distributes data more evenly but can make range queries difficult.
    *   **Consistent Hashing:** A more advanced hashing technique that minimizes data movement when adding or removing shards. Commonly used in distributed systems like Cassandra and DynamoDB.
*   **Challenges:**
    *   **Hotspots:** Uneven data distribution or access patterns.
    *   **Resharding:** Adding/removing shards can be complex and require data migration.
    *   **Joins:** Cross-shard joins are expensive and difficult. Often requires denormalization or application-level joins.

**4. Message Queues & Event Streaming**

*   **Purpose:** Decouple services, enable asynchronous communication, buffer requests, level load, facilitate event-driven architectures.
*   **Key Concepts:**
    *   **Producer:** Application that sends messages.
    *   **Consumer:** Application that receives and processes messages.
    *   **Broker/Queue/Topic:** Intermediary that stores messages until consumers are ready.
*   **Examples:**
    *   **RabbitMQ:** Traditional message broker, supports complex routing, AMQP protocol.
    *   **Apache Kafka:** Distributed streaming platform, high-throughput, fault-tolerant, used for real-time data pipelines and event streaming.
    *   **AWS SQS (Simple Queue Service):** Fully managed message queuing service.
    *   **Google Cloud Pub/Sub:** Real-time messaging service.
*   **Use Cases:** Background job processing, notifications, order processing, microservice communication, log aggregation, real-time analytics.

**5. Load Balancing**

*   **Purpose:** Distribute incoming network traffic across multiple servers to ensure no single server is overwhelmed, improving availability, reliability, and scalability.
*   **Types:**
    *   **Layer 4 (Network/Transport Layer):** Operates at the TCP/UDP level. Distributes traffic based on IP addresses and ports. Faster, less complex.
    *   **Layer 7 (Application Layer):** Operates at the HTTP/HTTPS level. Can make routing decisions based on content (URL, headers, cookies). More intelligent, SSL termination.
*   **Algorithms:**
    *   **Round Robin:** Distributes requests sequentially.
    *   **Least Connections:** Sends request to server with fewest active connections.
    *   **Least Response Time:** Sends request to server with fastest response time.
    *   **IP Hash:** Uses client's IP address to determine server, ensuring a client is always directed to the same server (useful for sticky sessions).
*   **Sticky Sessions (Session Affinity):** Directs all requests from a specific client to the same server. Useful for stateful applications, but can lead to uneven load.

**6. Content Delivery Networks (CDNs)**

*   **Purpose:** Distribute static content (images, videos, CSS, JavaScript) to servers geographically closer to users (Points of Presence - PoPs or edge servers). Reduces latency, offloads origin server.
*   **How it works:** When a user requests content, the CDN directs the request to the nearest edge server. If the content is cached there, it's served directly. If not, the edge server fetches it from the origin server, caches it, and then serves it.
*   **Use Cases:** Websites with global audiences, media streaming, software distribution.

**7. API Paradigms**

*   **REST (Representational State Transfer):**
    *   Architectural style using standard HTTP methods (GET, POST, PUT, DELETE). Stateless. Resource-based URLs. Commonly uses JSON.
    *   **Pros:** Simple, widely adopted, leverages HTTP caching.
    *   **Cons:** Can lead to over-fetching or under-fetching of data, multiple round trips for complex needs.
*   **gRPC (Google Remote Procedure Call):**
    *   High-performance, language-agnostic RPC framework. Uses Protocol Buffers for message serialization and HTTP/2 for transport.
    *   **Pros:** Efficient (binary serialization, multiplexing), supports streaming, code generation.
    *   **Cons:** Less browser-friendly directly (requires gRPC-Web proxy), steeper learning curve.
*   **GraphQL:**
    *   Query language for APIs and a server-side runtime. Allows clients to request exactly the data they need.
    *   **Pros:** Solves over/under-fetching, single endpoint for multiple resources, strongly typed.
    *   **Cons:** Can be complex to implement on the server, caching is more challenging than REST.
*   **WebSockets:**
    *   Enables full-duplex communication channels over a single TCP connection.
    *   **Pros:** Real-time, bidirectional communication (e.g., chat apps, live updates).
    *   **Cons:** Can be stateful, managing connections at scale can be complex.

**C. Fundamental Laws, Theorems & Concepts**

*   **CAP Theorem (Brewer's Theorem):**
    *   A distributed data store can only provide two of the following three guarantees:
        *   **Consistency:** All nodes see the same data at the same time. Every read receives the most recent write or an error.
        *   **Availability:** Every request receives a (non-error) response – without guarantee that it contains the most recent write.
        *   **Partition Tolerance:** The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.
    *   In reality, partition tolerance is a must for most distributed systems. So, the trade-off is between Consistency and Availability (CP vs. AP systems).
*   **BASE Properties (Often associated with NoSQL databases):**
    *   An alternative to ACID, emphasizing availability over strong consistency.
    *   **Basically Available:** The system guarantees availability.
    *   **Soft state:** The state of the system may change over time, even without input, due to eventual consistency.
    *   **Eventual consistency:** The system will eventually become consistent once no new updates are made to a given data item.
*   **Scalability:**
    *   **Vertical Scaling (Scaling Up):** Increasing resources (CPU, RAM, Disk) on a single server. Has limits.
    *   **Horizontal Scaling (Scaling Out):** Adding more servers to distribute the load. More complex but offers better elasticity and fault tolerance.
*   **Availability vs. Reliability:**
    *   **Availability:** The percentage of time the system is operational and accessible. Measured in "nines" (e.g., 99.999% uptime).
    *   **Reliability:** The probability that the system will perform its intended function without failure for a specified period under stated conditions.
*   **Latency vs. Throughput:**
    *   **Latency:** The time taken for a single operation to complete (e.g., request-response time).
    *   **Throughput:** The number of operations processed per unit of time (e.g., requests per second).
*   **Idempotency:**
    *   An operation is idempotent if making multiple identical requests has the same effect as making a single request. Crucial for retry mechanisms in distributed systems (e.g., payment processing).
*   **Stateless vs. Stateful Services:**
    *   **Stateless:** Each request from a client to a server is treated as an independent transaction. No session information is retained by the server. Easier to scale and load balance.
    *   **Stateful:** Server retains information (state) about past interactions with a client. More complex to scale and manage.
*   **Single Point of Failure (SPOF):**
    *   A component whose failure will cause the entire system to fail. Designs should aim to eliminate SPOFs through redundancy.
*   **Redundancy and Failover:**
    *   **Redundancy:** Having duplicate components (servers, databases) to take over if a primary component fails.
    *   **Failover:** The process of switching to a redundant component when a primary fails. Can be automatic or manual.
*   **Rate Limiting and Throttling:**
    *   Techniques to control the amount of traffic sent or received. Protects services from abuse, overload, and ensures fair usage.
*   **Data Replication:**
    *   Copying data across multiple nodes/servers. Improves availability, fault tolerance, and read performance.
    *   **Strategies:** Master-Slave, Master-Master, Quorum-based.

---

## III. Top High-Level Design Problems & Expert Solutions

Here are some classic HLD problems with a structured breakdown following the expert framework.

### Problem 1: Design a URL Shortener (e.g., TinyURL, bit.ly)

**1. Understand and Clarify Requirements**
   *   **Functional Requirements:**
        *   Generate a short URL given a long URL.
        *   Redirect users from a short URL to the original long URL.
        *   Users should be able to create custom short URLs (optional).
        *   Analytics: Track click count for each short URL (optional).
        *   URL Expiration (optional).
   *   **Non-Functional Requirements:**
        *   **High Availability:** The service must be highly available.
        *   **Low Latency:** Redirection should be very fast.
        *   **Scalability:** Handle a large number of URLs and high traffic for redirection. Read-heavy system.
        *   **Durability:** Shortened URLs should not be lost.
        *   **Uniqueness:** Short URLs must be unique.
   *   **Scale Estimation (Example):**
        *   New URLs per month: 100 million
        *   Reads (redirections) per second: 10,000 (peak ~50,000)
        *   Write-to-Read ratio: Very low (e.g., 1:100 or 1:1000)
        *   URL length: Short URL ~6-8 chars. Long URL avg ~100 chars.
        *   Data storage: ~3TB over 5 years for 6 Billion URLs.

**2. High-Level System Design**
   *   **Components:** Client, Load Balancer (LB), Web/Application Servers (Stateless), ID Generation Service, Database (DB), Cache.
   *   **API Endpoints:**
        *   `POST /api/v1/shorten` (Request: `{"long_url": "...", "custom_alias": "..." (optional)}`, Response: `{"short_url": "..."}`)
        *   `GET /{short_id}` (Redirects to long URL with HTTP 301/302)
        *   `GET /api/v1/analytics/{short_id}` (Response: `{"clicks": ...}`)

**3. Deep Dive into Components & Design Choices**
   *   **ID Generation Service:**
        *   **Chosen Approach:** Distributed Counter (e.g., based on Zookeeper/etcd or dedicated DB with ranges) + Base62 encoding (62^7 ~ 3.5 trillion unique IDs with 7 chars).
        *   **Reasoning:** Guarantees uniqueness, short URLs are not guessable. App servers can fetch ranges of IDs to reduce load on the ID generator.
   *   **Database:**
        *   **Type:** NoSQL Key-Value store (e.g., DynamoDB, Cassandra). Key: `short_id`, Value: `long_url`, metadata.
        *   **Reasoning:** Fast lookups, high scalability, schema flexibility. Eventual consistency is acceptable.
        *   **Sharding:** Based on `short_id` (e.g., consistent hashing).
   *   **Cache:**
        *   **Type:** Distributed cache (e.g., Redis, Memcached) using Cache-aside strategy.
        *   **Reasoning:** Reduce DB load and latency for popular URLs. Eviction: LRU.
   *   **Application Servers:** Stateless, horizontally scalable.
   *   **Redirection Type:** HTTP 302 (Temporary Redirect) to enable click tracking and dynamic updates. Trade-off: higher load vs. control/analytics.

**4. Identify Bottlenecks and Trade-offs**
   *   **Bottlenecks:** ID generation (mitigated by distributed approach/batching), DB write contention (mitigated by NoSQL choice and sharding).
   *   **Trade-offs:** Eventual consistency for higher availability. Custom URLs add complexity (need separate lookup/DB table).

**5. Further Discussion Points / Interviewer Probes**
   *   Handling custom aliases (check availability, separate index).
   *   URL expiration (background job for cleanup).
   *   Analytics implementation (asynchronous logging to message queue like Kafka, batch processing).
   *   Rate limiting, security (malicious URL prevention via services like Google Safe Browsing API).

---

### Problem 2: Design a Social Media Feed (e.g., Twitter, Facebook News Feed)

**1. Understand and Clarify Requirements**
    *   **Functional Requirements:**
        *   Users can post content (text, images, videos).
        *   Users can follow other users.
        *   Users can see a feed of posts from people they follow, sorted chronologically or by relevance.
        *   Feed should be near real-time.
    *   **Non-Functional Requirements:**
        *   **High Availability:** Feed must always be accessible.
        *   **Low Latency:** Feed generation should be fast.
        *   **Scalability:** Support millions of users, posts, and follows. Read-heavy for feed generation.
        *   **Durability:** Posts should not be lost.
        *   **Eventual Consistency:** Acceptable for feed updates (a small delay is fine).
    *   **Scale Estimation (Example):**
        *   Daily Active Users (DAU): 500 million
        *   Average posts per user per day: 0.1 - 1
        *   Average follows per user: 200
        *   Feed reads per user per day: 5-10
        *   Read/Write Ratio: High (e.g., 100:1 for feed reads vs. posts)

**2. High-Level System Design**
    *   **Components:** Client, Load Balancer, API Gateway, User Service, Post Service, Follow Service, Feed Generation Service, Notification Service, Databases (User DB, Post DB, Follow Graph DB, Feed Cache DB), Object Storage (for media), CDN.
    *   **API Endpoints:**
        *   `POST /api/v1/posts` (Create post)
        *   `GET /api/v1/feed` (Get user's feed)
        *   `POST /api/v1/users/{userId}/follow`
        *   `GET /api/v1/users/{userId}/posts`

**3. Deep Dive into Components & Design Choices**
    *   **Data Storage:**
        *   **User Data:** SQL DB or Document DB (e.g., MySQL, MongoDB) for user profiles.
        *   **Post Data:** NoSQL DB (e.g., Cassandra, DynamoDB) optimized for writes and time-series data. `post_id`, `user_id`, `content`, `timestamp`.
        *   **Follow Graph:** Graph DB (e.g., Neo4j) or a simple SQL/NoSQL table (`follower_id`, `followee_id`).
        *   **Media Storage:** Object storage like AWS S3 or Google Cloud Storage, fronted by a CDN.
    *   **Feed Generation:**
        *   **Approach 1: Pull Model (On-the-fly generation):**
            1.  User requests feed.
            2.  Fetch IDs of users they follow.
            3.  Fetch recent posts from these followees.
            4.  Merge and rank/sort.
            *   **Pros:** Simpler for users with few follows. Always fresh.
            *   **Cons:** High latency for users with many follows ("celebrity problem"). High load on DBs.
        *   **Approach 2: Push Model (Pre-computation / Fan-out):**
            1.  User creates a post.
            2.  System identifies all followers of this user.
            3.  Inject this post (or its ID) into the feed/timeline of each follower (often stored in a cache like Redis list).
            *   **Pros:** Fast feed reads.
            *   **Cons:** High write load (fan-out on write). Wasted effort for inactive users. "Celebrity problem" still an issue for fan-out.
        *   **Hybrid Approach (Commonly Used):**
            *   Push model for most users.
            *   Pull model for users following celebrities or for very active users (don't precompute for them, or only for a subset).
            *   Use a dedicated Feed Cache (e.g., Redis sorted sets or lists) to store precomputed feeds.
    *   **Ranking:** Chronological is basic. Relevance ranking involves ML models, user engagement signals.
    *   **Caching:** Heavily cache user feeds, user sessions, frequently accessed posts.
    *   **Real-time Updates:** WebSockets or Long Polling for new post notifications or live feed updates. Message queues (Kafka) for asynchronous tasks like fan-out.

**4. Identify Bottlenecks and Trade-offs**
    *   **Feed Generation Latency:** Addressed by pre-computation and caching.
    *   **Fan-out on Write (Push Model):** Can be slow for users with millions of followers. Mitigate with asynchronous processing, dedicated worker pools.
    *   **"Hot" Posts/Users:** Can overload DBs/caches.
    *   **Trade-offs:**
        *   **Push vs. Pull:** Write amplification vs. read latency.
        *   **Consistency:** Eventual consistency for feeds is generally acceptable.
        *   **Storage:** Precomputed feeds can take significant storage.

**5. Further Discussion Points / Interviewer Probes**
    *   How to handle the "celebrity problem" in detail?
    *   How to ensure feed freshness while using caches?
    *   Designing the ranking algorithm (high-level concepts).
    *   Scaling individual services (User, Post, Follow).
    *   Handling media uploads and processing.
    *   Dealing with distributed transactions if needed (e.g., ensuring follow action is atomic).
    *   Monitoring system health and performance.

---

### Problem 3: Design a Ride-Sharing Service (e.g., Uber, Lyft)

**1. Understand and Clarify Requirements**
    *   **Functional Requirements:**
        *   Riders can request a ride from location A to B.
        *   Drivers can indicate their availability and current location.
        *   System matches available drivers to riders.
        *   Real-time location tracking of driver and ride progress.
        *   Fare calculation and payment processing.
        *   Ratings and reviews for riders/drivers.
    *   **Non-Functional Requirements:**
        *   **High Availability:** Service must be operational 24/7.
        *   **Low Latency:** Fast matching, real-time location updates.
        *   **Scalability:** Handle millions of users, drivers, and concurrent rides, especially in dense urban areas.
        *   **Accuracy:** Precise location tracking and fare calculation.
        *   **Reliability:** Ensure ride requests are not lost, and matches are fair.
    *   **Scale Estimation (Example):**
        *   Millions of active riders/drivers.
        *   Thousands of concurrent ride requests/active rides during peak hours.
        *   Frequent location updates per active user.

**2. High-Level System Design**
    *   **Components:** Rider App, Driver App, Load Balancer, API Gateway, User Service, Driver Management Service, Rider Management Service (Trip Service), Location Service, Matching Service, Pricing Service, Payment Service, Notification Service, Databases (User DB, Driver DB, Trip DB, Location DB), Real-time Messaging System (e.g., WebSockets, MQTT).
    *   **API Endpoints (Simplified):**
        *   Rider: `POST /api/v1/rides/request`, `GET /api/v1/rides/{rideId}/status`
        *   Driver: `POST /api/v1/drivers/{driverId}/availability`, `POST /api/v1/rides/{rideId}/accept`, `POST /api/v1/drivers/{driverId}/location`
        *   System: Internal APIs for matching, pricing.

**3. Deep Dive into Components & Design Choices**
    *   **Location Service & Management:**
        *   Drivers periodically publish their location. Riders publish their location when requesting.
        *   **Technology:** Use a geospatial database or index (e.g., PostGIS, Elasticsearch with geo-indexing, or custom Quadtree/Geohash implementation on top of a NoSQL store).
        *   **Data Flow:** Driver App -> Location Service -> Location DB.
        *   **Challenge:** Handling a high volume of location updates.
            *   **Solutions:** Batch updates, use efficient data structures, shard Location DB by region.
    *   **Driver-Rider Matching Service:**
        *   **Core Logic:** Find nearby available drivers for a rider's request. Considers factors like proximity, driver rating, vehicle type, estimated time of arrival (ETA).
        *   **Process:**
            1.  Rider requests ride.
            2.  Matching service queries Location Service for nearby drivers (e.g., within a 5km radius).
            3.  Filter drivers based on availability, type, etc.
            4.  Rank potential matches.
            5.  Notify top N drivers (sequentially or concurrently).
            6.  First driver to accept gets the ride.
        *   **Technology:** Can be a complex microservice. May use message queues for dispatching requests to drivers.
    *   **Real-time Communication:**
        *   WebSockets or MQTT for pushing location updates, ride status changes, notifications to apps.
    *   **Databases:**
        *   **User/Driver DB:** SQL or Document DB for profiles, ratings.
        *   **Trip DB:** SQL or NoSQL to store ride details, status, fare.
        *   **Location DB:** Geospatial DB or NoSQL with geo-indexing.
    *   **Pricing Service:** Calculates fare based on distance, time, demand (surge pricing), ride type.
    *   **Payment Service:** Integrates with third-party payment gateways.
    *   **State Management:** Trips go through various states (requested, accepted, ongoing, completed, cancelled). Use a robust state machine, potentially managed within the Trip Service and DB.

**4. Identify Bottlenecks and Trade-offs**
    *   **Location Update Volume:** Can overwhelm Location Service.
    *   **Matching Algorithm Complexity & Speed:** Needs to be fast and fair.
    *   **Real-time Communication Scalability:** Managing many persistent connections.
    *   **"Cold Start" Problem:** In new areas with few drivers/riders.
    *   **Trade-offs:**
        *   **Matching Precision vs. Speed:** More complex matching takes longer.
        *   **Data Consistency:** Strong consistency for ride state and payments, eventual for location updates might be acceptable for some views.
        *   **Surge Pricing:** Balances supply/demand but can impact user experience.

**5. Further Discussion Points / Interviewer Probes**
    *   How to implement surge pricing? (Monitor demand/supply ratio in geographical zones).
    *   How to handle driver cancellations or no-shows?
    *   Detailed design of the matching algorithm (e.g., using geospatial queries, ETA calculation).
    *   Scaling the Location Service and real-time messaging components.
    *   Ensuring fairness in driver dispatch.
    *   Data sharding strategies for different databases (e.g., by city/region).
    *   How to handle payments and refunds securely.
    *   Offline support or intermittent connectivity for drivers/riders.

---

### Problem 4: Design a Video Streaming Service (e.g., YouTube, Netflix)

**1. Understand and Clarify Requirements**
    *   **Functional Requirements:**
        *   Users can upload videos.
        *   Videos need to be processed and made available in various qualities/formats.
        *   Users can search for and stream videos.
        *   Users can create accounts, channels (optional), like/comment (optional).
        *   Support for different devices (web, mobile, smart TVs).
    *   **Non-Functional Requirements:**
        *   **High Availability:** Service must be highly available.
        *   **Low Latency Streaming:** Minimal buffering, fast start times.
        *   **Scalability:** Handle millions of users, vast video library, high concurrent streaming. Read-heavy (streaming).
        *   **Durability:** Uploaded videos should not be lost.
        *   **Reliability:** Consistent streaming experience.
    *   **Scale Estimation (Example):**
        *   Millions of Daily Active Users (DAU).
        *   Petabytes of video storage.
        *   High number of concurrent streams (e.g., 1 million+).
        *   Video uploads per day: e.g., 100,000s.

**2. High-Level System Design**
    *   **Components:**
        *   **Client Apps:** Web, Mobile, Smart TV.
        *   **Load Balancers/API Gateway:** Entry point for all requests.
        *   **User Service:** Manages user accounts, profiles, authentication.
        *   **Video Upload Service:** Handles raw video uploads.
        *   **Video Processing Service (Transcoding Pipeline):** Converts raw video into multiple formats and bitrates (e.g., HLS, DASH).
        *   **Metadata Service:** Manages video titles, descriptions, thumbnails, user data related to videos.
        *   **Search Service:** Enables users to find videos.
        *   **Recommendation Service:** Suggests videos to users.
        *   **Streaming Service:** Delivers video segments to clients.
        *   **Notification Service:** For upload completion, new content, etc.
        *   **Analytics Service:** Tracks views, engagement.
        *   **Storage Systems:**
            *   **Raw Video Storage:** e.g., AWS S3, Google Cloud Storage (GCS).
            *   **Processed Video Storage (Segments):** e.g., S3/GCS, often fronted by CDN.
            *   **Metadata DB:** e.g., Cassandra, MySQL/PostgreSQL (sharded).
            *   **Search Index:** e.g., Elasticsearch.
        *   **CDN (Content Delivery Network):** Crucial for distributing video segments globally.
        *   **Message Queues:** For decoupling services (e.g., Kafka, RabbitMQ for processing pipeline).

**3. Deep Dive into Components & Design Choices**
    *   **Video Ingestion & Processing:**
        1.  User uploads raw video via Upload Service to a temporary storage.
        2.  Upload Service sends a message to a queue (e.g., Kafka) indicating a new video.
        3.  Video Processing Service (workers) pick up messages:
            *   Validate video format.
            *   **Transcoding:** Convert to adaptive bitrate streaming formats (e.g., HLS, MPEG-DASH). This creates multiple versions of the video at different resolutions and bitrates.
            *   Generate thumbnails.
            *   Extract metadata.
        4.  Processed segments and thumbnails are stored in permanent object storage (e.g., S3).
        5.  Metadata (URLs to segments, title, etc.) is stored in Metadata DB.
        6.  Notification sent on completion.
    *   **Storage:**
        *   **Object Storage (S3/GCS):** For raw and processed video files due to scalability, durability, and cost-effectiveness.
        *   **Metadata DB:** Can be NoSQL (e.g., Cassandra for video metadata, user watch history due to high write volume and scalability) or SQL (e.g., PostgreSQL for user accounts, structured data). Sharding is essential.
        *   **Search Index (Elasticsearch):** For indexing video metadata (title, description, tags) to enable fast searching.
    *   **Video Streaming & CDN:**
        *   Client requests video.
        *   Streaming Service (or client directly with signed URLs) fetches a manifest file (e.g., .m3u8 for HLS) from CDN.
        *   Manifest file lists available bitrates and URLs to video segments.
        *   Client player adaptively requests video segments (small chunks of a few seconds) from the CDN based on network conditions.
        *   CDN caches popular video segments at edge locations close to users.
    *   **Adaptive Bitrate Streaming (ABS):**
        *   Key for smooth playback. Player dynamically switches between different quality streams based on available bandwidth.
    *   **Search Service:**
        *   Uses Elasticsearch or similar to index video metadata.
        *   Provides full-text search, filtering, sorting.
    *   **Recommendation Service:**
        *   Uses collaborative filtering, content-based filtering, or hybrid approaches.
        *   Analyzes user watch history, likes, video metadata.
    *   **Scalability:**
        *   All services should be stateless and horizontally scalable.
        *   Heavy use of CDNs.
        *   Message queues for asynchronous processing.
        *   Database sharding and replication.

**4. Identify Bottlenecks and Trade-offs**
    *   **Video Processing:** Computationally intensive. Use a scalable worker fleet, potentially serverless functions.
    *   **Storage Costs:** Significant. Tiered storage, compression.
    *   **CDN Costs & Cache Hit Ratio:** Optimizing CDN usage is crucial.
    *   **Real-time Recommendations:** Complex, may require separate infrastructure.
    *   **Trade-offs:**
        *   **Storage vs. Processing:** Storing many pre-transcoded versions vs. on-the-fly transcoding (less common for VOD).
        *   **Latency vs. Cost:** Higher CDN distribution means lower latency but higher cost.
        *   **Consistency of metadata:** Eventual consistency is often acceptable for view counts, likes.

**5. Further Discussion Points / Interviewer Probes**
    *   Digital Rights Management (DRM).
    *   Live streaming architecture.
    *   Personalization and A/B testing features.
    *   How to handle copyright infringement (e.g., content ID systems).
    *   Monitoring video quality of experience (QoE).
    *   Cost optimization strategies.

---

### Problem 5: Design an E-commerce Website (e.g., Amazon, Flipkart)

**1. Understand and Clarify Requirements**
    *   **Functional Requirements:**
        *   Users can browse/search products.
        *   Product pages with details, images, reviews.
        *   Shopping cart functionality.
        *   User accounts and order history.
        *   Checkout process (shipping, payment).
        *   Inventory management.
        *   Seller-side features (product listing, order management - if applicable).
    *   **Non-Functional Requirements:**
        *   **High Availability:** Especially during sales events.
        *   **Scalability:** Handle millions of users, products, and orders.
        *   **Low Latency:** Fast page loads, quick search results.
        *   **Consistency:** Strong consistency for inventory and orders. Eventual for reviews, recommendations.
        *   **Durability:** Order data, user data must not be lost.
        *   **Security:** Secure payment processing, user data protection.
    *   **Scale Estimation (Example):**
        *   Millions of SKUs (Stock Keeping Units).
        *   Millions of DAU.
        *   Thousands of orders per second during peak.

**2. High-Level System Design**
    *   **Microservices Architecture:**
        *   **Product Catalog Service:** Manages product information (details, images, prices).
        *   **Search Service:** Powers product search and filtering.
        *   **Inventory Service:** Manages stock levels for products.
        *   **Cart Service:** Manages users' shopping carts.
        *   **Order Service:** Handles order creation, processing, and tracking.
        *   **User Service:** Manages user accounts, profiles, addresses.
        *   **Payment Service:** Integrates with payment gateways.
        *   **Recommendation Service:** Suggests products to users.
        *   **Review Service:** Manages product reviews and ratings.
        *   **Notification Service:** For order updates, promotions.
        *   **API Gateway:** Single entry point for client requests.
    *   **Databases:**
        *   **Product Catalog DB:** NoSQL (e.g., MongoDB, Elasticsearch) for flexible schema, text search.
        *   **Inventory DB:** SQL (e.g., PostgreSQL, MySQL) for ACID transactions to ensure strong consistency.
        *   **Order DB:** SQL for transactional integrity.
        *   **User DB:** SQL or NoSQL.
        *   **Cart DB:** Key-value store (e.g., Redis) for speed and temporary nature.
    *   **Caches:** Distributed cache (Redis/Memcached) for product data, sessions, search results.
    *   **Message Queues (Kafka/RabbitMQ):** For asynchronous tasks (order confirmation emails, updating search index, inventory updates post-order).
    *   **CDN:** For static assets (product images, CSS, JS).
    *   **Search Engine (Elasticsearch/Solr):** For product search.

**3. Deep Dive into Components & Design Choices**
    *   **Product Catalog & Search:**
        *   Product data stored in a NoSQL DB (flexible attributes) and indexed into Elasticsearch for rich search capabilities (faceted search, full-text, typo tolerance).
        *   CDN for product images.
    *   **Inventory Management:**
        *   Critical for consistency. Use a relational DB with ACID properties.
        *   Optimistic or pessimistic locking during checkout to reserve stock.
        *   Compensating transactions if an order fails after inventory deduction.
    *   **Shopping Cart:**
        *   Often stored in a fast key-value store like Redis (user_id -> cart_details).
        *   Can be persisted to a DB for longer-term storage if user logs out.
    *   **Order Processing:**
        *   Complex workflow: Payment authorization -> Inventory update -> Shipping logistics -> Notifications.
        *   Use message queues to decouple these steps and ensure reliability.
        *   Order data stored in a relational DB for strong consistency and audit trails.
    *   **Payment Service:**
        *   Integrates with third-party payment gateways (Stripe, PayPal).
        *   PCI-DSS compliance is crucial. Often, sensitive payment details are handled by the gateway directly.
    *   **Recommendation Service:**
        *   Collaborative filtering ("users who bought this also bought..."), content-based filtering, purchase history.
        *   Batch processing for model training, real-time for serving.
    *   **Scalability & Availability:**
        *   Microservices allow independent scaling.
        *   Databases sharded by customer ID, product ID, or order ID.
        *   Read replicas for databases.
        *   Aggressive caching for product pages and search results.
        *   Graceful degradation during high load (e.g., temporarily disable less critical features).

**4. Identify Bottlenecks and Trade-offs**
    *   **Inventory Service:** High contention, single source of truth.
    *   **Payment Gateway Integration:** External dependency, potential latency.
    *   **Order Service:** Complex state management, needs to be highly reliable.
    *   **Database Scalability:** Especially for Inventory and Order DBs.
        *   Solutions: Read replicas for SQL, careful sharding, choosing appropriate NoSQL for other services.
    *   **"Flash Sales" / High Traffic Events:** Requires significant pre-scaling and load shedding strategies.
    *   **Trade-offs:**
        *   **Consistency vs. Availability:** Strong consistency for orders/inventory is paramount, potentially sacrificing some availability under extreme load. Eventual consistency for reviews, recommendations.
        *   **Cost of infrastructure:** Supporting peak load can be expensive. Auto-scaling and serverless can help.

**5. Further Discussion Points / Interviewer Probes**
    *   How to handle "thundering herd" problem during sales? (Queuing, rate limiting).
    *   Schema design for product attributes (highly variable).
    *   Fraud detection mechanisms.
    *   Internationalization and localization.
    *   A/B testing for UI/UX and pricing strategies.
    *   Data analytics pipeline for business intelligence.

---

### Problem 6: Design a Chat Application (e.g., WhatsApp, Slack)

**1. Understand and Clarify Requirements**
    *   **Functional Requirements:**
        *   One-on-one chat.
        *   Group chat.
        *   Text messages. Media sharing (images, videos, files) - optional for MVP.
        *   Presence (online/offline/typing indicators).
        *   Message history/persistence.
        *   Read receipts (sent, delivered, read) - optional.
        *   Push notifications for new messages.
    *   **Non-Functional Requirements:**
        *   **Low Latency:** Messages delivered quickly.
        *   **High Availability:** Chat service should always be accessible.
        *   **Scalability:** Support millions of concurrent users and messages.
        *   **Reliability:** Messages should not be lost (or at least, loss should be minimized and detectable).
        *   **Consistency:** Eventual consistency for message delivery is usually acceptable, but order matters within a chat.
        *   **Security:** End-to-end encryption (E2EE) is a common advanced feature.
    *   **Scale Estimation (Example):**
        *   Millions of DAU.
        *   Billions of messages per day.
        *   High number of concurrent connections.

**2. High-Level System Design**
    *   **Components:**
        *   **Client Apps:** Mobile (iOS, Android), Web, Desktop.
        *   **Load Balancers.**
        *   **API Gateway / Frontend Servers:** Handle initial client connections, authentication.
        *   **Chat Service / Messaging Service:** Core logic for message routing, presence, group management.
        *   **User Service:** Manages user accounts, contacts, profiles.
        *   **Presence Service:** Tracks user online/offline status and typing indicators.
        *   **Message Store DB:** Stores chat history (e.g., Cassandra, HBase for scalability).
        *   **User/Group Metadata DB:** Stores user profiles, group info (e.g., MySQL, PostgreSQL).
        *   **Push Notification Service:** Integrates with APNS (Apple), FCM (Google) for mobile notifications.
        *   **Media Storage Service:** For storing images, videos, files (e.g., S3, GCS).
        *   **Real-time Communication Layer:** WebSockets are commonly used for persistent connections. MQTT is another option.
    *   **Key Technologies:**
        *   **WebSockets:** For persistent, bidirectional communication between client and server.
        *   **NoSQL Databases (e.g., Cassandra):** For storing messages due to high write volume and need for time-series data.
        *   **Message Queues (e.g., Kafka):** For asynchronous tasks, buffering messages before writing to DB.

**3. Deep Dive into Components & Design Choices**
    *   **Client-Server Communication (Real-time):**
        *   Clients establish a persistent WebSocket connection to a Chat Server (or a cluster of them via a load balancer that understands WebSockets).
        *   When User A sends a message to User B:
            1.  A's client sends message to its connected Chat Server.
            2.  Chat Server identifies B's connected Chat Server (if different, via a routing mechanism or shared state).
            3.  Message is forwarded to B's Chat Server.
            4.  B's Chat Server delivers message to B's client via WebSocket.
            5.  Message is asynchronously persisted to the Message Store DB.
    *   **Message Storage:**
        *   **DB Choice:** NoSQL like Cassandra or HBase is suitable for messages.
            *   **Schema:** `chat_id` (or `user_id1_user_id2` for 1:1, `group_id` for group), `message_id` (timestamp-based or sequence number), `sender_id`, `content`, `timestamp`.
            *   **Partition Key:** `chat_id` for co-locating messages of the same chat.
            *   **Clustering Key:** `message_id` or `timestamp` for ordering.
        *   Messages can be queued (e.g., Kafka) before writing to DB to handle bursts and improve write latency for the client.
    *   **Presence Service:**
        *   Clients send heartbeats or connect/disconnect events to Presence Service via Chat Servers.
        *   Presence status (online/offline, last seen) stored in a fast cache (e.g., Redis) or a dedicated DB.
        *   Subscribers (other users) are notified of presence changes.
    *   **Group Chat:**
        *   Similar to 1:1, but message needs to be fanned out to all members of the group.
        *   Chat Server receives message for a group, looks up group members, and delivers/routes message to each online member.
        *   Offline members get messages on next login or via push notifications.
    *   **Push Notifications:**
        *   If recipient is offline, Chat Server sends message details to Push Notification Service, which then sends a push to the user's device via APNS/FCM.
    *   **Message Synchronization & History:**
        *   When a client comes online, it fetches recent messages from the Message Store DB.
        *   Delta sync mechanisms to get only new messages since last sync.
    *   **Scalability of Chat Servers:**
        *   Stateless or semi-stateful (maintaining WebSocket connections).
        *   Service discovery to find which server a user is connected to.
        *   Consistent hashing can be used to map users to specific chat servers.
    *   **Media Messages:**
        *   Client uploads media to Media Storage Service (e.g., S3), gets a URL.
        *   Sends the URL as a message. Other clients download from URL.

**4. Identify Bottlenecks and Trade-offs**
    *   **Managing millions of persistent WebSocket connections:** Requires specialized servers (e.g., Netty, Erlang/Elixir based) and careful resource management.
    *   **Fan-out for group messages:** Can be resource-intensive.
    *   **Presence updates:** High volume of updates.
    *   **Message store write/read load.**
    *   **Trade-offs:**
        *   **Message Delivery Guarantees:** At-least-once vs. at-most-once. Idempotency needed for retries.
        *   **Consistency vs. Latency:** Eventual consistency for message delivery and presence is common. Stronger consistency for user account creation.
        *   **E2EE Complexity:** Adds significant complexity to key management and server-side features (like search).

**5. Further Discussion Points / Interviewer Probes**
    *   End-to-end encryption implementation details.
    *   How to handle message ordering and synchronization across multiple devices for the same user.
    *   "Typing..." indicators implementation.
    *   Read receipts (sent/delivered/read) mechanism.
    *   Offline message delivery.
    *   Scaling the Presence service.
    *   Dealing with "noisy neighbor" problem in a multi-tenant chat server environment.
    *   Disaster recovery for message store.

---

## IV. Other Common HLD Problems for FANG Interviews (Total 20 including detailed ones)

The following list includes other frequently asked HLD problems. Practicing these will broaden your understanding of different system challenges. The first 6 problems in this guide are detailed above.

1.  **Design a Distributed Key-Value Store (e.g., DynamoDB, Cassandra)**
    *   Key aspects: Data partitioning (sharding, consistent hashing), replication (quorum), consistency models (strong vs. eventual), conflict resolution (vector clocks), fault tolerance, scalability, gossip protocol.
2.  **Design a Search Engine (e.g., Google Search - simplified)**
    *   Key aspects: Web crawler, indexer (inverted index), query processor, ranking algorithm (PageRank simplified), distributed storage for index.
3.  **Design a Distributed Job Scheduler (e.g., cron on steroids, Airflow)**
    *   Key aspects: Job definition, submission, scheduling policies (time-based, event-based), worker management, fault tolerance (retries, idempotency), monitoring, dependency management.
4.  **Design a Typeahead Suggestion Service (Autocomplete)**
    *   Key aspects: Trie data structure, frequency analysis, personalized suggestions, low latency, updating suggestions, caching, sharding the trie.
5.  **Design a Distributed Cache System (e.g., Redis Cluster, Memcached)**
    *   Key aspects: Cache eviction policies (LRU, LFU), consistency with source of truth, sharding (client-side, proxy, server-side), fault tolerance, write strategies (write-through, write-back).
6.  **Design a Stock Trading System**
    *   Key aspects: Order matching engine (order book), real-time price updates (market data feed), high throughput, low latency, fault tolerance, transactionality (ACID).
7.  **Design a Web Crawler**
    *   Key aspects: URL frontier management (priority queue), politeness (robots.txt, crawl delay), duplicate detection (Bloom filters, hashing), distributed crawling, data extraction, storage.
8.  **Design an API Rate Limiter**
    *   Key aspects: Different algorithms (token bucket, leaky bucket, fixed window, sliding window log), distributed rate limiting (centralized store like Redis), storage for counters, accuracy.
9.  **Design a Notification System**
    *   Key aspects: Different channels (email, SMS, push), user preferences, message queuing, fan-out strategies, idempotency, tracking delivery status, retries.
10. **Design a Hotel Booking System (e.g., Booking.com)**
    *   Key aspects: Search (by location, dates, filters), availability checking (inventory management, concurrency control), booking workflow, pricing, reviews.
11. **Design a Distributed Logging System (e.g., ELK stack)**
    *   Key aspects: Log collection agents, log aggregation, scalable storage (e.g., Elasticsearch, HDFS), search/query capabilities, real-time analysis, alerting.
12. **Design a System to Find Nearby Friends/Places (Geospatial Search)**
    *   Key aspects: Geospatial indexing (Quadtrees, R-trees, Geohashes), real-time location updates from mobile devices, efficient querying for "nearby" entities.
13. **Design a Pastebin-like Service**
    *   Key aspects: Text storage, unique ID generation for pastes, optional user accounts, paste expiration, custom URLs, syntax highlighting.
14. **Design a System for Counting Unique Visitors to a Website**
    *   Key aspects: Handling large volumes of events, probabilistic data structures (HyperLogLog for approximation), batch vs. real-time processing, defining "uniqueness" (IP, cookie, user ID).

---

## V. Final Tips from an Expert

*   **Communicate Actively:** Think out loud. Explain your reasoning. Your thought process is more important than the final "perfect" design.
*   **Draw Diagrams:** Visuals are powerful. Use the whiteboard or digital tool to illustrate components and interactions.
*   **Start Simple, Then Iterate:** Don't over-engineer from the start. Get a basic working design and then address NFRs, scale, and edge cases.
*   **Be Ready to Justify:** For every choice (database, algorithm, protocol), explain *why* you chose it over alternatives and what trade-offs are involved.
*   **Don't Be Afraid to Say "I Don't Know, But Here's How I'd Think About It":** Honesty and a structured problem-solving approach are valued.
*   **Manage Your Time:** Roughly allocate time for understanding requirements, high-level design, deep dive, and discussing trade-offs.
*   **Engage the Interviewer:** Treat it as a collaborative design session. Ask for their input or if they want you to focus on a specific area.
*   **Practice, Practice, Practice:** Work through these problems yourself, ideally by whiteboarding or explaining them to someone else.

Good luck! You've got this.
