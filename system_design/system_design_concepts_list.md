# **System Design Interview – Complete Cheat Sheet**

A system design interview tests whether you can select the right architectural components, understand trade-offs, and design for scalability, reliability, performance, and maintainability.

Below are important concepts grouped by category, with **when to use each**.

## **1. Architecture & Design Patterns**

| Concept                    | When to use                                           |
| -------------------------- | ----------------------------------------------------- |
| Monolithic Architecture    | Small applications, simple deployment                 |
| Microservices              | Large systems with independently scalable services    |
| Modular Monolith           | Want clear boundaries without microservice complexity |
| Event-Driven Architecture  | Asynchronous communication between services           |
| Serverless Architecture    | Event-triggered workloads, variable traffic           |
| Layered Architecture       | Separate API, business, and data layers               |
| Hexagonal Architecture     | Isolate business logic from external dependencies     |
| CQRS                       | Read and write workloads have different requirements  |
| Event Sourcing             | Need complete history of state changes                |
| Saga Pattern               | Distributed transactions across multiple services     |
| Strangler Pattern          | Gradually migrate legacy monolith to microservices    |
| API Gateway                | Central entry point for multiple backend services     |
| BFF (Backend for Frontend) | Different API needs for web and mobile clients        |

## **2. Scalability & Performance**

| Concept                 | When to use                                               |
| ----------------------- | --------------------------------------------------------- |
| Horizontal Scaling      | Increase capacity by adding servers                       |
| Vertical Scaling        | Increase CPU/RAM of existing server                       |
| Load Balancing          | Distribute requests across instances                      |
| Auto Scaling            | Handle variable traffic automatically                     |
| Caching                 | Reduce database load and response latency                 |
| CDN                     | Serve static content closer to users                      |
| Database Replication    | Improve read scalability and availability                 |
| Database Sharding       | Distribute massive datasets across databases              |
| Connection Pooling      | Reuse DB connections and reduce overhead                  |
| Asynchronous Processing | Move slow tasks outside request-response cycle            |
| Batch Processing        | Process large volumes of records together                 |
| Parallel Processing     | Execute independent tasks simultaneously                  |
| Backpressure            | Prevent overloaded services from accepting excessive work |
| Rate Limiting           | Restrict requests per user/client                         |
| Request Coalescing      | Combine identical concurrent requests                     |

## **3. Database & Data Storage**

| Concept                | When to use                                                              |
| ---------------------- | ------------------------------------------------------------------------ |
| SQL / Relational DB    | Structured data, transactions, relationships                             |
| NoSQL                  | Flexible schemas, high-scale distributed workloads                       |
| Document DB            | JSON-like documents and flexible structures                              |
| Key-Value DB           | Sessions, caching, simple lookups                                        |
| Wide-Column DB         | Massive distributed write workloads                                      |
| Graph DB               | Relationship-heavy queries and traversals                                |
| Time-Series DB         | Metrics, IoT, monitoring data                                            |
| Vector DB              | Semantic search, RAG, similarity search                                  |
| Object Storage         | Images, videos, PDFs, backups                                            |
| Full-Text Search       | Keyword search across large documents                                    |
| Composite Index        | Queries filtering/sorting multiple columns                               |
| Partitioning           | Split large tables into manageable sections                              |
| Read Replicas          | Offload read-heavy database workloads                                    |
| Materialized Views     | Precompute expensive read queries                                        |
| Database Normalization | Reduce duplication and maintain consistency                              |
| Denormalization        | Optimize read-heavy workloads                                            |
| Optimistic Locking     | Prevent conflicting concurrent updates                                   |
| Pessimistic Locking    | Protect critical shared records                                          |
| ACID Transactions      | Financial and consistency-critical operations                            |
| CAP Theorem            | Understand consistency/availability trade-offs during network partitions |

## **4. Distributed Systems**

| Concept                      | When to use                                                              |
| ---------------------------- | ------------------------------------------------------------------------ |
| Consistent Hashing           | Distribute data across changing nodes                                    |
| Distributed Lock             | Coordinate access to shared resources                                    |
| Leader Election              | Select one node for coordination                                         |
| Quorum                       | Require agreement from multiple replicas                                 |
| Eventual Consistency         | Accept temporary stale data for availability                             |
| Strong Consistency           | Need immediately consistent reads                                        |
| Idempotency                  | Prevent duplicate processing of requests                                 |
| Retry with Backoff           | Recover from temporary failures                                          |
| Exponential Backoff + Jitter | Avoid retry storms                                                       |
| Circuit Breaker              | Stop calls to an unhealthy dependency                                    |
| Bulkhead Pattern             | Isolate failures between components                                      |
| Service Discovery            | Locate dynamic service instances                                         |
| Heartbeat                    | Detect unhealthy nodes                                                   |
| Gossip Protocol              | Distribute cluster membership/state                                      |
| Clock Synchronization        | Coordinate distributed timestamps                                        |
| Distributed Transaction      | Maintain consistency across multiple databases/services                  |
| Outbox Pattern               | Reliably publish DB changes as events                                    |
| Exactly-Once Processing      | Prevent duplicate business effects through carefully designed processing |

## **5. Messaging & Communication**

| Concept                  | When to use                                       |
| ------------------------ | ------------------------------------------------- |
| REST API                 | Standard synchronous request-response             |
| GraphQL                  | Clients need flexible data selection              |
| gRPC                     | High-performance service-to-service communication |
| WebSockets               | Real-time bidirectional communication             |
| Server-Sent Events (SSE) | Server-to-client live updates                     |
| Message Queue            | Decouple producers and consumers                  |
| Pub/Sub                  | Broadcast events to multiple subscribers          |
| Kafka                    | High-throughput event streaming and replay        |
| RabbitMQ                 | Task queues and flexible message routing          |
| Dead Letter Queue (DLQ)  | Store messages that repeatedly fail               |
| Message Acknowledgment   | Confirm successful message processing             |
| Consumer Groups          | Parallelize message consumption                   |
| Message Deduplication    | Avoid duplicate event processing                  |
| Schema Registry          | Manage event/message schema compatibility         |

## **6. Reliability & Fault Tolerance**

| Concept                    | When to use                                          |
| -------------------------- | ---------------------------------------------------- |
| Redundancy                 | Avoid single points of failure                       |
| Failover                   | Switch to backup instance/system                     |
| Health Checks              | Identify unhealthy instances                         |
| Graceful Degradation       | Keep essential features working during failures      |
| Timeout                    | Prevent requests from hanging indefinitely           |
| Retry                      | Recover from transient failures                      |
| Circuit Breaker            | Protect against cascading failures                   |
| Disaster Recovery          | Recover after major infrastructure failure           |
| Multi-Region Deployment    | Geographic availability and disaster recovery        |
| Active-Active              | Multiple regions serve traffic simultaneously        |
| Active-Passive             | Backup region takes over after failure               |
| Replication Lag Monitoring | Detect stale database replicas                       |
| Checkpointing              | Resume long-running processing after failure         |
| Bulkhead Isolation         | Prevent one failure from affecting the entire system |

## **7. Security**

| Concept            | When to use                                     |
| ------------------ | ----------------------------------------------- |
| Authentication     | Verify user identity                            |
| Authorization      | Control what users can access                   |
| OAuth 2.0          | Delegated authorization                         |
| OpenID Connect     | User authentication and identity                |
| JWT                | Stateless token-based authentication            |
| RBAC               | Access based on user roles                      |
| ABAC               | Access based on attributes and policies         |
| API Keys           | Identify applications/services                  |
| TLS / HTTPS        | Encrypt communication                           |
| Encryption at Rest | Protect stored data                             |
| Secrets Manager    | Securely manage credentials and API keys        |
| WAF                | Protect web applications against common attacks |
| Input Validation   | Prevent malformed or malicious input            |
| Audit Logging      | Track sensitive operations                      |
| Zero Trust         | Verify every access request                     |

## **8. Observability & Monitoring**

| Concept             | When to use                                |
| ------------------- | ------------------------------------------ |
| Logging             | Debug application behavior                 |
| Metrics             | Track CPU, memory, throughput, errors      |
| Distributed Tracing | Follow requests across microservices       |
| Correlation ID      | Track one request across multiple services |
| RED Metrics         | Monitor Rate, Errors, Duration             |
| USE Metrics         | Monitor Utilization, Saturation, Errors    |
| SLA                 | Define service-level agreement             |
| SLO                 | Define internal reliability targets        |
| SLI                 | Measure service reliability                |
| Alerting            | Notify when thresholds are breached        |
| Health Dashboard    | Monitor system health                      |
| Error Budget        | Balance reliability and release velocity   |

## **9. Networking & Infrastructure**

| Concept                | When to use                                      |
| ---------------------- | ------------------------------------------------ |
| DNS                    | Resolve domain names to IP addresses             |
| Reverse Proxy          | Route incoming requests to backend services      |
| Forward Proxy          | Control outbound client traffic                  |
| L4 Load Balancer       | Route based on TCP/UDP information               |
| L7 Load Balancer       | Route based on HTTP paths, headers, etc.         |
| TLS Termination        | Handle HTTPS encryption at the load balancer     |
| VPC / VNet             | Isolate cloud network resources                  |
| Subnets                | Separate public and private resources            |
| NAT Gateway            | Allow private resources outbound internet access |
| Firewall               | Restrict network traffic                         |
| Containerization       | Package applications consistently                |
| Kubernetes             | Orchestrate containerized workloads              |
| Infrastructure as Code | Provision infrastructure reproducibly            |

## **10. Important System Design Numbers & Trade-offs**

Know these concepts when discussing capacity and architecture:

| Concept                      | When to use                                    |
| ---------------------------- | ---------------------------------------------- |
| QPS / RPS                    | Estimate incoming traffic                      |
| Peak Traffic                 | Design for maximum load                        |
| Throughput                   | Estimate processing capacity                   |
| Latency (p50/p95/p99)        | Define response-time expectations              |
| Bandwidth                    | Estimate network requirements                  |
| Storage Estimation           | Calculate database/object storage requirements |
| Data Retention               | Decide how long data should be stored          |
| Availability (99.9%, 99.99%) | Define uptime requirements                     |
| RPO                          | Maximum acceptable data loss                   |
| RTO                          | Maximum acceptable recovery time               |
| Hot vs Cold Data             | Decide storage and retrieval strategy          |
| Cost vs Performance          | Choose infrastructure based on budget          |
| Consistency vs Availability  | Decide behavior during distributed failures    |
| Latency vs Throughput        | Optimize for response time or volume           |

## **11. Apply these concepts to common system design problems**

This is particularly useful for interviews: recognize the scenario and identify the architecture.

| System to design     | Important concepts                                            |
| -------------------- | ------------------------------------------------------------- |
| URL Shortener        | Hashing, SQL/NoSQL, caching, read replicas                    |
| WhatsApp / Chat      | WebSockets, message queues, pub/sub, delivery acknowledgment  |
| Instagram            | CDN, object storage, feed generation, caching                 |
| YouTube              | Object storage, CDN, transcoding, queues                      |
| Uber                 | Geospatial indexing, WebSockets, event-driven architecture    |
| Ticket Booking       | Distributed locking, transactions, idempotency                |
| Payment System       | ACID, idempotency, Saga, audit logs                           |
| Notification System  | Queue, pub/sub, retries, DLQ                                  |
| E-commerce           | Microservices, caching, inventory consistency, Saga           |
| Search Engine        | Inverted index, distributed indexing, ranking                 |
| File Storage (Drive) | Object storage, metadata DB, chunking, replication            |
| Rate Limiter         | Token Bucket, Leaky Bucket, Sliding Window                    |
| Video Streaming      | CDN, adaptive bitrate, transcoding                            |
| News Feed            | Fan-out on write/read, caching, ranking                       |
| Logging Platform     | Kafka, partitioning, indexing, retention                      |
| AI RAG System        | Vector DB, hybrid search, reranking, caching, async ingestion |

## **12. Must-know algorithms and patterns**

| Name                 | When to use                                           |
| -------------------- | ----------------------------------------------------- |
| Token Bucket         | Rate limiting with controlled bursts                  |
| Leaky Bucket         | Smooth traffic at a fixed rate                        |
| Sliding Window       | Accurate time-based rate limiting                     |
| LRU Cache            | Evict least recently used items                       |
| LFU Cache            | Evict least frequently used items                     |
| Consistent Hashing   | Distributed cache/data partitioning                   |
| Rendezvous Hashing   | Assign keys to distributed nodes                      |
| Bloom Filter         | Quickly check possible existence of an item           |
| Merkle Tree          | Efficient data synchronization and integrity checking |
| Trie                 | Prefix search and autocomplete                        |
| Inverted Index       | Full-text search                                      |
| Dijkstra             | Shortest path in weighted graphs                      |
| K-way Merge          | Merge sorted data streams                             |
| Rendezvous Selection | Select a node based on deterministic scoring          |

## **Interview application sequence**

When given any system design question, follow this order:

1. **Requirements Clarification**

   Functional and non-functional requirements.
2. **Capacity Estimation**

   DAU, QPS, peak traffic, storage, bandwidth.
3. **API Design**

   Endpoints, request/response, authentication.
4. **High-Level Architecture**

   Client → Load Balancer → API → Services → Database.
5. **Database Design**

   Schema, indexes, relationships, partitioning.
6. **Deep Dive**

   Caching, queues, algorithms, consistency, concurrency.
7. **Scalability**

   Horizontal scaling, replication, sharding, CDN.
8. **Reliability**

   Failover, retries, circuit breakers, disaster recovery.
9. **Security & Observability**

   Auth, RBAC, logging, metrics, tracing.
10. **Trade-offs**

    Explain why you selected one approach over alternatives.

