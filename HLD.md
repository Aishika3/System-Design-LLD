# HLD Topics to Learn (System Design Roadmap Notes)

Below are **clean, brief-but-descriptive notes** for the **HLD step-by-step topics** (like a study notebook).  
Structured for quick revision.

---

## 1) Basic Fundamentals

**Goal:** Understand the building blocks of how a system runs and scales.

### Servers vs Serverless
- **Server-based (EC2 / VMs):** You manage server capacity, scaling, deployments, monitoring.
- **Serverless (Lambda):** Cloud handles scaling/infrastructure; you focus on functions.
- **Tradeoff idea:** No “best” choice—depends on traffic pattern, cost, control, ops effort.
- **Common point:** For very large or constant load, **serverless can become expensive**, while server-based can be cheaper + predictable.

### Vertical vs Horizontal Scaling
- **Vertical scaling:** Upgrade one machine (more RAM/CPU/storage). Simple, but limited.
- **Horizontal scaling:** Add more machines + distribute load using a **Load Balancer**. Scales better, but increases complexity (deployment, data sync, failures).

### Core OS/Networking Basics
- **Processes vs Threads:** How execution happens; impacts performance and concurrency.
- **Request–Response cycle:** Client sends request → server processes → sends response.
- **DNS:** Domain name → IP mapping (helps client find the right server).

---

## 2) Databases

**Goal:** Know how to store, query, and scale data reliably.

### SQL vs NoSQL
- **SQL:** Structured data, strong consistency, transactions (often preferred in payments).
- **NoSQL:** Flexible schema, high scalability; common in distributed systems.
- You must be able to explain **when and why** to choose each.

### Types of Databases (Examples)
- **MongoDB:** Document store (NoSQL)
- **Neo4j:** Graph database (best for relationship-heavy data)

### In-memory Databases
- Data stored in RAM for speed (often used for caching/fast reads).

### Replication & Migration
- **Replication:** Copies data for availability + read scaling.
- **Migration:** Move/upgrade data without breaking the system.

### Partitioning & Sharding
- **Partitioning:** Split data into parts.
- **Sharding:** Horizontal partitioning across multiple machines (popular interview topic).
- Needed when a single database cannot handle scale.

---

## 3) Consistency & Availability (CAP Mindset)

**Goal:** Decide what matters more during failures: correct latest data or always responding.

### Consistency Levels
- **Eventual:** Data becomes consistent after some time.
- **Quorum:** Read/write requires majority agreement.
- **Causal:** Maintains cause-effect ordering.
- **Linearizable:** Strongest; reads always return latest write.

### Isolation Levels (Transactions)
- Read uncommitted, read committed, repeatable read  
  (controls how transactions “see” data)

### CAP Theorem
- During a **network partition**, a distributed system must choose:
  - **Consistency (C)** or **Availability (A)** (cannot fully guarantee both).
- **Example in video:** Payments (Paytm/PayPal/PhonePe) prefer **Consistency** → correctness is critical.
- Hence, payment systems often lean toward **SQL**.

---

## 4) Cache + CDN

**Goal:** Reduce latency and server load by serving data faster.

### Cache (Redis / Memcached)
- Stores frequently used data so responses are faster.
- Best for **low latency** access to popular/repeated requests.

### Cache Policies
- **Write policies**
  - Write-through: cache + DB updated together
  - Write-back: cache first, DB later
  - Write-around: write to DB directly, cache on read
- **Replacement policies**
  - **LRU:** remove least recently used
  - **LFU:** remove least frequently used
  - Segmented LRU: improved LRU strategy

### CDN (Content Delivery Network)
- Speeds up delivery of static/large content (images, videos, files).
- Netflix example: store popular episodes closer to users → less buffering.

---

## 5) Networking

**Goal:** Know how data moves and which protocol fits which use-case.

- **TCP vs UDP**
  - TCP: reliable delivery, slower
  - UDP: faster, less reliable (good for real-time)
- **HTTP vs HTTPS**
  - HTTPS adds encryption/security.
- **HTTP 1 vs 2 vs 3**
  - Performance improvements (multiplexing, QUIC, etc.). Know at a high level.
- **WebSockets**
  - Persistent connection for real-time updates (chat, live dashboards).
- **WebRTC**
  - Real-time audio/video communication (Zoom/Google Meet style systems).

---

## 6) Load Balancers

**Goal:** Distribute traffic across multiple servers so the system doesn’t crash under load.

- **Why needed:** Horizontal scaling needs a “traffic manager.”
- **Algorithms**
  - Round robin
  - Least connections
- **Stateless vs Stateful load balancing**
  - Stateless: any request can go to any server
  - Stateful: session stickiness / affinity to one server
- **Consistent hashing**
  - Keeps distribution stable when servers are added/removed.
- **Proxy vs Reverse Proxy**
  - Reverse proxy sits in front of servers for routing, caching, security.
- **Rate limiting**
  - Prevents abuse / DDoS by limiting requests per user/IP.

---

## 7) Message Queues

**Goal:** Handle tasks asynchronously so the main system stays fast and stable.

- Used when work can be delayed and processed later.
- **Non-critical tasks** go into queues; system stays responsive.
- WhatsApp example:
  - Message send = critical
  - “double tick / delivery update” = less critical → can be async
- Tools mentioned:
  - **Kafka**, **RabbitMQ**
- Often follows **pub-sub** model (publish events, subscribers consume).

---

## 8) Monolith vs Microservices

**Goal:** Understand how architecture evolves with growth.

- **Monolith:** Start simple—one codebase/service.
- As users/teams grow, shift toward **microservices**.
- Key concepts:
  - **Single point of failure**
  - **Cascading failures** (one service failure triggers chain failures)
  - **Containerization (Docker)** is common in microservices
  - Migration path monolith → microservices

---

## 9) Monitoring & Logging

**Goal:** Detect issues early and debug failures quickly.

- Logs answer: *“What broke? Where? Why?”*
- Monitoring tracks health metrics (latency, errors, CPU, DB load).
- Netflix sale example: traffic spikes can break systems → logs/metrics help fix + plan better.
- Tools mentioned:
  - AWS CloudWatch, Grafana, Prometheus
- Also mentions anomaly detection (spot unusual patterns).

---

## 10) Security

**Goal:** Protect users, data, and system.

- Authentication (who are you?)
- Authorization (what can you access?)
- Tokens, OAuth
- ACL (Access Control List)
- Encryption (data in transit + at rest)

---

## 11) System Design Tradeoffs

**Goal:** Interviews judge how well you justify design decisions.

- Push vs Pull architecture
- Consistency vs Availability
- SQL vs NoSQL
- Memory vs latency vs throughput vs accuracy

**Key message:** No single right design—your reasoning matters.

---

## 12) Practice Systems

**Goal:** Build intuition by designing real products.

Practice: YouTube, Twitter, WhatsApp, Amazon, Zoom, Instagram, Uber

What you learn:
- Amazon: e-commerce flow + scaling
- Zoom: real-time communication/streaming
- Instagram: media-heavy storage + feed
- Uber: multiple user roles + matching + real-time

---

## 13) Final Example: Netflix HLD Summary

Netflix-like HLD includes:
- **Servers** (compute)
- **CDN** (fast global delivery)
- **Load balancers** (traffic distribution)

Principle: **Graceful degradation**
> Better to satisfy some users than disappoint all (system should partially work even under heavy load).

---
