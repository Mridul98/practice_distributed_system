# Distributed Systems Mastery Curriculum

## A Comprehensive Learning Path from Foundations to Production

**Duration**: 5 months (Intensive)  
**Prerequisites**: Proficiency in Python, basic data structures, algorithms, and networking fundamentals  
**Language**: Python (primary), with exposure to Go for production tools

---

## Table of Contents

1. [Overview & Philosophy](#overview--philosophy)
2. [Phase 1: Foundations](#phase-1-foundations-weeks-1-3)
3. [Phase 2: Storage & Databases](#phase-2-storage--databases-weeks-4-7)
4. [Phase 3: Stream Processing](#phase-3-stream-processing-weeks-8-10)
5. [Phase 4: Orchestration & Operations](#phase-4-orchestration--operations-weeks-11-14)
6. [Phase 5: Production Scenarios](#phase-5-production-scenarios-weeks-15-18)
7. [Exam Specifications](#exam-specifications)
8. [Tools & Environment Setup](#tools--environment-setup)
9. [Assessment Criteria](#assessment-criteria)

---

## Overview & Philosophy

### Learning Approach

This curriculum follows a **"theory → implementation → production"** progression:

1. **Understand the theory** - Read papers, watch lectures, understand why systems are designed this way
2. **Implement from scratch** - Build simplified versions to deeply understand the mechanics
3. **Use production tools** - Learn how real systems solve these problems at scale
4. **Deploy and operate** - Run your systems, observe failures, build observability

### Core Beliefs

- **Build to understand** - You don't truly understand a concept until you've implemented it
- **Failure teaches best** - Introduce failures intentionally to understand resilience
- **Production matters** - Learning on toy systems is necessary but insufficient; understand production realities
- **Community is key** - Read papers, study open-source code, engage with the community

### Recommended Daily Schedule (Intensive Mode)

| Day | Activity | Time |
|-----|----------|------|
| Monday | Theory: Read/watch lecture | 2 hours |
| Tuesday | Theory: Paper discussion | 2 hours |
| Wednesday | Implementation: Lab work | 3 hours |
| Thursday | Implementation: Lab work | 3 hours |
| Friday | Review + Practice problems | 2 hours |
| Weekend | Deep work: Exam implementation | 4-6 hours |

---

## Phase 1: Foundations

**Weeks 1-3**  
**Goal**: Build mental models for distributed computing, understand fundamental challenges

---

### Chapter 1: Distributed Systems Fundamentals

**Week 1**  
**Time**: 10-12 hours

#### Learning Objectives

- Understand what makes distributed systems fundamentally different from single-node systems
- Master the CAP theorem and its practical implications
- Learn failure models and their impact on system design
- Understand the costs of distribution

#### Topics

1. **Introduction to Distributed Systems**
   - Definition and characteristics
   - motivations: scalability, fault tolerance, geographic distribution, security/isolation

2. **The CAP Theorem**
   - Consistency, Availability, Partition tolerance
   - PACELC extension
   - When to prioritize consistency vs availability
   - Real-world examples (Amazon Dynamo, Google Spanner)

3. **Failure Models**
   - Crash-stop failures
   - Crash-recovery failures
   - Byzantine failures
   - Network partitions
   - Failure detectors

4. **Fundamental Challenges**
   - Network latency and unreliability
   - Partial failures
   - Concurrency without shared memory
   - Security across trust boundaries

#### Resources

**Primary**:
- MIT 6.824 Lecture 1: Introduction (https://pdos.csail.mit.edu/6.824/)
- Distributed Systems for Fun and Profit (http://book.mixu.net/distsys/)

**Supplementary - Books**:
- "Designing Data-Intensive Applications" - Martin Kleppmann (./books/ddia-book.pdf)
- Distributed Systems: Concepts and Design - Chapter 1
- Testing Distributed Systems Guide (https://asatarin.github.io/testing-distributed-systems/)

**University Courses**:
- University of Cambridge - Distributed Systems (https://www.cl.cam.ac.uk/teaching/2021/ConcDisSys/)
- Stanford CS244b: Distributed Systems (https://www.scs.stanford.edu/12au-cs244b/)

**Papers**:
- "CAP Twelve Years Later" - Eric Brewer (https://www.infoq.com/articles/cap-twelve-years-later/)
- "Harvest, Yield and Scalable Tolerant Systems" - Brewer et al. (https://arxiv.org/abs/cs/0703000)
- "A Note on Distributed Computing" - https://scholarworks.iu.edu/dspace/bitstream/handle/2022.196/13759/Birman%20-%20A%20Note%20on%20Distributed%20Computing.pdf
- "The Byzantine Generals Problem" - https://lamport.azurewebsites.net/pubs/byz.pdf

**Additional Resources**:
- High Scalability Blog (http://highscalability.com/)
- ACM Queue - Distributed Systems (https://queue.acm.org/listing.cfm?topic=distributed_systems)

#### Lab 1.1: Network Partition Simulator

**Objective**: Experience CAP tradeoffs firsthand

**Steps**:
1. Create a Python class representing a distributed key-value store with 3 nodes
2. Implement write operations that replicate to all nodes synchronously
3. Add network partition simulation (can "disconnect" nodes from each other)
4. Demonstrate: when partition occurs, system must choose between consistency or availability
5. Measure: write latency, read latency, data divergence during partitions

**Deliverable**: Python script demonstrating CAP behavior with configurable consistency levels

**Estimated Time**: 3-4 hours

---

### Chapter 2: Communication & Serialization

**Week 2**  
**Time**: 10-12 hours

#### Learning Objectives

- Understand inter-process communication in distributed systems
- Master serialization formats and their tradeoffs
- Build simple RPC systems from scratch
- Learn protocol buffers for efficient serialization

#### Topics

1. **Communication Models**
   - Synchronous vs asynchronous communication
   - Request-response, pub/sub, message passing
   - Connection-oriented vs connectionless

2. **Remote Procedure Call (RPC)**
   - Stub generation
   - Marshalling/unmarshalling
   - Transport layer (TCP, HTTP)
   - Error handling and retries

3. **Serialization Formats**
   - JSON: Human-readable, slow, verbose
   - MessagePack: Binary, compact
   - Protocol Buffers: Schema-based, efficient, versioning
   - Apache Thrift, Avro

4. **Protocol Buffers Deep Dive**
   - Defining .proto files
   - Code generation
   - Backward/forward compatibility
   - Performance characteristics

#### Resources

**Primary**:
- Protobuf Documentation (https://developers.google.com/protocol-buffers)
- gRPC Basics (https://grpc.io/docs/languages/python/basics/)

**Supplementary**:
- "Designing Data-Intensive Applications" - Chapter 4

#### Lab 2.1: Build Your Own RPC Framework

**Objective**: Understand RPC internals by building a mini-framework

**Steps**:
1. Define a simple protocol buffer schema for RPC requests/responses
2. Implement a TCP server that receives requests
3. Create client stub that serializes requests and sends over TCP
4. Add support for different data types (int, string, list, dict)
5. Implement basic error handling (connection refused, timeout)
6. Add request ID for tracking

**Deliverable**: `pyrpc/` library with server and client implementations

**Estimated Time**: 4-5 hours

#### Lab 2.2: gRPC Service Implementation

**Objective**: Learn production-grade RPC with gRPC

**Steps**:
1. Define a protobuf service for a simple calculator
2. Generate Python code from .proto files
3. Implement the server with error handling
4. Create a client that makes concurrent requests
5. Add streaming support (server-side streaming)

**Deliverable**: Working gRPC calculator service with unit tests

**Estimated Time**: 3-4 hours

---

### Chapter 3: Consistency Models

**Week 3**  
**Time**: 10-12 hours

#### Learning Objectives

- Master different consistency models
- Understand the consistency spectrum
- Learn when to use each model
- Understand causal vs total ordering

#### Topics

1. **Consistency Spectrum**
   - Strong consistency (linearizability)
   - Sequential consistency
   - Causal consistency
   - Eventual consistency
   - Read-your-writes consistency
   - Session guarantees

2. **Ordering and Causality**
   - Lamport clocks
   - Vector clocks
   - Causal ordering protocols
   - Total order broadcast

3. **Practical Consistency**
   - When strong consistency is necessary
   - Accepting eventual consistency
   - Hybrid approaches
   - Amazon's S3 (eventual consistency)

#### Resources

**Primary**:
- "Designing Data-Intensive Applications" - Chapter 5, 6
- "Concurrency and Distributed Systems" - Martin Kleppmann (Cambridge lectures)

**Papers**:
- "Lamport Clocks" - Leslie Lamport
- "Dynamo: Amazon's Highly Available Key-value Store"

#### Lab 3.1: Vector Clock Implementation

**Objective**: Implement causal consistency tracking

**Steps**:
1. Implement a VectorClock class in Python
2. Implement clock comparison (happened-before, concurrent)
3. Add merge function for reconciling concurrent updates
4. Build a simple key-value store that tracks causality
5. Demonstrate conflict detection and resolution

**Deliverable**: Vector clock implementation with test cases

**Estimated Time**: 3-4 hours

#### Lab 3.2: Consistency Model Comparison

**Objective**: Compare consistency behavior in real systems

**Steps**:
1. Set up Redis with replication
2. Test write behavior with synchronous vs asynchronous replication
3. Test read-your-writes consistency in Redis transactions
4. Test eventual consistency with Redis pub/sub
5. Document observations and timing

**Deliverable**: Report with experiments and timing data

**Estimated Time**: 2-3 hours

---

## Phase 2: Storage & Databases

**Weeks 4-7**  
**Goal**: Master distributed data storage, replication, and consensus

---

### Chapter 4: Distributed Replication

**Week 4**  
**Time**: 12-15 hours

#### Learning Objectives

- Understand replication strategies and their tradeoffs
- Implement leader-based and leaderless replication
- Handle replication lag and conflicts
- Build monitoring for replication health

#### Topics

1. **Replication Strategies**
   - Leader-based (primary-backup)
   - Multi-leader (active-active)
   - Leaderless (Dynamo-style)
   - Synchronous vs asynchronous

2. **Replication Implementations**
   - Statement-based replication
   - Row-based replication (binlog)
   - Write-ahead log (WAL) shipping
   - Change Data Capture (CDC)

3. **Conflict Resolution**
   - Last-writer-wins
   - Vector clocks
   - CRDTs (Conflict-free Replicated Data Types)
   - Application-defined resolution

4. **Replication at Scale**
   - Quorums (R/W/N configuration)
   - Sloppy quorums
   - Hinted handoff
   - Anti-entropy (Merkle trees)

#### Resources

**Primary**:
- "Designing Data-Intensive Applications" - Chapter 5
- MIT 6.824 Lecture 4: Go Balkans, Primary-Backup Replication

**Papers**:
- "PacificA: Replication in Log-Based Distributed Storage Systems"
- "Dynamo: Amazon's Highly Available Key-value Store" (Sections on replication)

#### Lab 4.1: Primary-Backup Replication

**Objective**: Build a replicated key-value store

**Steps**:
1. Implement a single-node key-value store with write-ahead log
2. Add a follower node that replays the WAL
3. Implement synchronous replication (wait for follower acknowledgment)
4. Add automatic failover when primary fails
5. Handle split-brain scenarios
6. Add replication lag monitoring

**Deliverable**: Replicated KV store with automatic failover

**Estimated Time**: 6-8 hours

---

### Chapter 5: Consensus & Raft

**Week 5-6**  
**Time**: 15-20 hours

#### Learning Objectives

- Understand the consensus problem and its impossibility results
- Master the Raft consensus algorithm
- Implement leader election and log replication
- Understand Paxos for historical context

#### Topics

1. **The Consensus Problem**
   - Formal definition
   - FLP impossibility result
   - Safety vs liveness
   - Leader-based consensus

2. **Raft Algorithm Deep Dive**
   - Leader election
   - Log replication
   - Safety properties
   - Membership changes
   - Log compaction

3. **Paxos (Historical Context)**
   - Basic Paxos
   - Multi-Paxos
   - Why Raft was created
   - When to use which

4. **Consensus in Practice**
   - etcd/raft implementation
   - Consul
   - CockroachDB
   - TiKV

#### Resources

**Primary**:
- Raft Paper: "In Search of an Understandable Consensus Algorithm"
- Raft Website: https://raft.github.io/
- MIT 6.824 Lab 2: Raft

**Supplementary**:
- "Paxos Made Simple" - Leslie Lamport
- Raft Visualization: http://raft.github.io/

#### Lab 5.1: Mini-Raft Implementation

**Objective**: Implement core Raft algorithm

**Steps**:
1. Implement Raft node with state (leader, follower, candidate)
2. Implement leader election with randomized timeouts
3. Add log replication (append entries)
4. Implement commitment and applies to state machine
5. Add persistence (save/load Raft state)
6. Handle network partitions

**Deliverable**: Working Raft implementation (~500 lines)

**Estimated Time**: 10-15 hours

#### Exam 1: Distributed Configuration Manager

**Problem Statement**:  
Build a distributed configuration management service that stores configuration values and automatically propagates changes to all nodes. The system must remain available during network partitions and ensure all nodes eventually see the same configuration.

**Requirements**:
- **Functional**: Store key-value configurations, watch for changes, handle node additions/removals
- **Non-functional**: High availability, eventual consistency, automatic failover

**Programming Steps**:
1. Set up 3-node Raft cluster using the Lab 5.1 implementation
2. Implement configuration storage API (set, get, delete)
3. Add watch/subscribe functionality for configuration changes
4. Implement snapshotting for log compaction
5. Add cluster membership change (add/remove nodes)
6. Build HTTP API gateway for clients
7. Add health checks and metrics

**Solution Suite**:
- Test leader election within 5 seconds
- Test configuration replication to all followers
- Test configuration persists after leader crash
- Test watch callbacks fire on configuration changes
- Test graceful node addition

---

### Chapter 6: Distributed Databases

**Week 7**  
**Time**: 12-15 hours

#### Learning Objectives

- Understand database architecture at scale
- Master sharding and partitioning strategies
- Implement distributed transactions
- Learn from production systems (Cassandra, CockroachDB)

#### Topics

1. **Sharding & Partitioning**
   - Horizontal partitioning (sharding)
   - Consistent hashing
   - Range-based partitioning
   - Adaptive indexing

2. **Distributed Transactions**
   - Two-phase commit (2PC)
   - Three-phase commit (3PC)
   - Optimistic concurrency control
   - Deterministic transactions

3. **Database Architecture**
   - Google Spanner
   - CockroachDB
   - Apache Cassandra
   - Amazon Aurora

4. **Query Processing**
   - Distributed query planning
   - Distributed joins
   - Query routing
   - Parallel query execution

#### Resources

**Primary**:
- "Designing Data-Intensive Applications" - Chapter 6, 7, 8
- CockroachDB Architecture: https://www.cockroachlabs.com/docs/stable/architecture/overview

**Papers**:
- "Spanner: Google's Globally-Distributed Database"
- "Aurora: On Avoiding Distributed Transactions"

#### Lab 6.1: Sharded Database Design

**Objective**: Design a sharded database system

**Steps**:
1. Implement consistent hashing for data distribution
2. Create shard router that routes requests to appropriate shard
3. Implement cross-shard transactions using 2PC
4. Add shard rebalancing when nodes are added/removed
5. Implement distributed query execution (simple aggregations)
6. Add monitoring for shard health

**Deliverable**: Sharded database with query routing

**Estimated Time**: 8-10 hours

#### Exam 2: Distributed Photo Storage System

**Problem Statement**:  
Design a distributed photo storage and sharing system similar to Instagram. Users upload photos, which are stored and replicated across data centers. The system must handle millions of users with low latency.

**Requirements**:
- **Functional**: Upload photos, view photos, like photos, follow users
- **Non-functional**: High availability, geo-distribution, eventual consistency for likes

**Programming Steps**:
1. Set up Apache Cassandra cluster (or use ScyllaDB)
2. Design data model: users, photos, likes, follows
3. Implement photo upload with sharding by user_id
4. Implement timeline generation (photos from followed users)
5. Add secondary indexes for efficient queries
6. Implement like counter with eventual consistency
7. Set up multi-datacenter replication
8. Add connection pooling and query optimization

**Solution Suite**:
- Test photo upload latency < 500ms
- Test timeline generation for users with 1000+ follows
- Test like counter eventual consistency
- Test cross-datacenter replication
- Test failure handling when a node goes down

---

## Phase 3: Stream Processing

**Weeks 8-10**  
**Goal**: Master real-time data processing, event streaming, and messaging systems

---

### Chapter 7: Message Queues & Event Streaming

**Week 8**  
**Time**: 12-15 hours

#### Learning Objectives

- Understand message queue architecture and patterns
- Master Apache Kafka from fundamentals to production
- Implement exactly-once semantics
- Build event-driven architectures

#### Topics

1. **Message Queue Fundamentals**
   - Point-to-point vs pub/sub
   - Message ordering
   - Delivery semantics (at-least-once, at-most-once, exactly-once)
   - Dead letter queues

2. **Apache Kafka Deep Dive**
   - Broker, topic, partition, offset
   - Producers and consumers
   - Consumer groups
   - Replication and ISR

3. **Exactly-Once Semantics**
   - Idempotent producers
   - Transactions
   - Exactly-once in Kafka Streams

4. **Kafka at Scale**
   - Partition assignment strategies
   - Rebalancing
   - Monitoring and optimization
   - Schema registry

#### Resources

**Primary**:
- Kafka Documentation: https://kafka.apache.org/documentation/
- "Kafka: The Definitive Guide" - Neha Narkhede et al.
- Confluent Blog: https://www.confluent.io/blog/

**Papers**:
- "Kafka: A Distributed Messaging System for Log Processing"

#### Lab 7.1: Kafka from Scratch (Simplified)

**Objective**: Understand Kafka internals by building a mini-message broker

**Steps**:
1. Implement a simple message broker with topic support
2. Add partition support with key-based routing
3. Implement consumer group coordination (simplified)
4. Add offset management
5. Implement disk-based message retention
6. Add producer retries and acknowledgment

**Deliverable**: Mini Kafka implementation

**Estimated Time**: 6-8 hours

#### Lab 7.2: Real-Time Analytics Pipeline

**Objective**: Build production-grade streaming pipeline

**Steps**:
1. Set up Kafka cluster (3 brokers)
2. Create topics for raw events and processed events
3. Implement producer: generate simulated user events (click, view, purchase)
4. Implement stream processor: aggregate metrics in time windows
5. Sink processed data to Elasticsearch
6. Visualize with Kibana dashboards
7. Add monitoring with Prometheus

**Deliverable**: Complete streaming analytics pipeline

**Estimated Time**: 6-8 hours

---

### Chapter 8: Stream Processing Frameworks

**Week 9-10**  
**Time**: 12-15 hours

#### Learning Objectives

- Master stream processing concepts
- Implement windowed computations
- Understand stateful processing
- Learn fault tolerance in streaming

#### Topics

1. **Stream Processing Concepts**
   - Continuous queries
   - Time windows (tumbling, sliding, session)
   - Watermarks and late data handling
   - State and checkpointing

2. **Kafka Streams**
   - KStream, KTable, GlobalKTable
   - Processor API
   - Interactive queries
   - Exactly-once semantics

3. **Apache Flink (Overview)**
   - DataStream API
   - SQL support
   - Complex event processing
   - Savepoints

4. **Advanced Patterns**
   - Pattern matching
   - Stream joins
   - Real-time ML integration

#### Resources

**Primary**:
- Kafka Streams Documentation
- Flink Documentation: https://flink.apache.org/

**Papers**:
- "The Dataflow Model" - Google

#### Lab 8.1: Real-Time Fraud Detection

**Objective**: Build a real-time fraud detection system

**Steps**:
1. Create transaction event stream in Kafka
2. Implement rules engine for fraud detection
3. Build sliding window analysis (e.g., >3 transactions in 5 minutes)
4. Add pattern detection (unusual location, amount spike)
5. Implement alert streaming to notification service
6. Add model inference integration (simple rule-based)
7. Build dashboard for fraud analysts

**Deliverable**: Working fraud detection system

**Estimated Time**: 8-10 hours

#### Exam 3: E-Commerce Event Sourcing System

**Problem Statement**:  
Build an event-sourced order processing system for an e-commerce platform. All order state changes should be captured as events and processed in real-time to update inventory, trigger notifications, and generate analytics.

**Requirements**:
- **Functional**: Process orders, update inventory, send notifications, generate reports
- **Non-functional**: Exactly-once processing, handle 10k events/second, sub-second latency

**Programming Steps**:
1. Design event schema: OrderCreated, PaymentReceived, InventoryReserved, Shipped, Delivered
2. Set up Kafka topics for each event type
3. Implement order service that publishes events
4. Implement inventory consumer: reserve/release inventory
5. Implement notification consumer: send email/SMS
6. Add saga coordinator for distributed transactions
7. Implement event replay for debugging
8. Add backpressure handling
9. Build order state reconstruction from events

**Solution Suite**:
- Test exactly-once processing (idempotency)
- Test saga rollback on failure
- Test replay recovers correct state
- Test throughput > 10k events/sec
- Test latency < 500ms for end-to-end

---

## Phase 4: Orchestration & Operations

**Weeks 11-14**  
**Goal**: Master deployment, operations, and observability of distributed systems

---

### Chapter 9: Service Discovery & Load Balancing

**Week 11**  
**Time**: 10-12 hours

#### Learning Objectives

- Understand service discovery mechanisms
- Implement health checking and failover
- Master load balancing strategies
- Build resilient service communication

#### Topics

1. **Service Discovery**
   - Client-side vs server-side discovery
   - DNS-based discovery
   - Service registries (Consul, etcd)
   - Service meshes

2. **Health Checking**
   - Health check types (HTTP, TCP, custom)
   - Healthy vs unhealthy thresholds
   - Circuit breakers

3. **Load Balancing**
   - Round-robin, least connections
   - Consistent hashing
   - Geographic routing
   - Active-active vs active-passive

4. **Resilience Patterns**
   - Circuit breakers
   - Rate limiting
   - Bulkheads
   - Retries with backoff

#### Resources

**Primary**:
- "Designing Data-Intensive Applications" - Chapter 4
- Netflix Eureka, Ribbon documentation

**Papers**:
- "Building Microservices" - Sam Newman

#### Lab 9.1: Service Mesh Lite

**Objective**: Build service discovery and load balancing

**Steps**:
1. Implement service registry with etcd
2. Add health checking for registered services
3. Implement client-side load balancer
4. Add circuit breaker pattern
5. Implement retry with exponential backoff
6. Build service proxy for inter-service communication

**Deliverable**: Service discovery and load balancing system

**Estimated Time**: 6-8 hours

---

### Chapter 10: Container Orchestration with Kubernetes

**Week 12-13**  
**Time**: 15-20 hours

#### Learning Objectives

- Master Docker fundamentals
- Understand Kubernetes architecture
- Deploy and manage applications on Kubernetes
- Implement scaling and rolling updates

#### Topics

1. **Docker Fundamentals**
   - Containers vs VMs
   - Dockerfile best practices
   - Multi-stage builds
   - Volume management

2. **Kubernetes Architecture**
   - Pod, Service, Deployment, StatefulSet
   - ConfigMap, Secret
   - Ingress
   - Custom Resource Definitions

3. **Kubernetes Operations**
   - Rolling updates and rollbacks
   - Auto-scaling (HPA, VPA)
   - Resource limits and quotas
   - Network policies

4. **Advanced Kubernetes**
   - Operators
   - Helm charts
   - Service mesh (Istio, Linkerd)
   - Multi-cluster

#### Resources

**Primary**:
- Kubernetes Documentation: https://kubernetes.io/docs/
- "Kubernetes in Action" - Marko Lukša

**Hands-on**:
- Kubernetes the Hard Way (optional, for depth)
- Play with Kubernetes

#### Lab 10.1: Deploy Distributed App to Kubernetes

**Objective**: Deploy a distributed application to Kubernetes

**Steps**:
1. Containerize the Exam 2 photo storage application
2. Create Kubernetes manifests (Deployment, Service, ConfigMap)
3. Set up persistent volumes for storage
4. Implement horizontal pod autoscaling
5. Add liveness and readiness probes
6. Set up Ingress for external access
7. Configure secrets for sensitive data
8. Add Helm chart for deployment

**Deliverable**: Deployment manifests and running application

**Estimated Time**: 8-10 hours

---

### Chapter 11: Observability & Monitoring

**Week 14**  
**Time**: 10-12 hours

#### Learning Objectives

- Master the three pillars of observability
- Implement distributed tracing
- Build effective monitoring and alerting
- Understand SLOs and error budgets

#### Topics

1. **Logs**
   - Structured logging
   - Log aggregation (ELK/EFK stack)
   - Log levels and sampling

2. **Metrics**
   - RED method (Rate, Errors, Duration)
   - USE method (Utilization, Saturation, Errors)
   - Prometheus data model
   - Grafana dashboards

3. **Distributed Tracing**
   - OpenTelemetry
   - Jaeger/Zipkin
   - Trace context propagation
   - Sampling strategies

4. **Alerting & SLOs**
   - Alert fatigue prevention
   - SLO vs SLA vs SLI
   - Error budgets
   - On-call best practices

#### Resources

**Primary**:
- "Site Reliability Engineering" - Google
- OpenTelemetry Documentation

#### Lab 11.1: Observability Dashboard

**Objective**: Add full observability to your distributed system

**Steps**:
1. Add structured logging to all services
2. Implement OpenTelemetry tracing
3. Set up Prometheus metrics collection
4. Create Grafana dashboards with RED metrics
5. Implement distributed tracing across services
6. Set up alerting rules
7. Build an error budget dashboard

**Deliverable**: Complete observability stack

**Estimated Time**: 6-8 hours

#### Exam 4: Self-Healing Microservices Platform

**Problem Statement**:  
Build a self-healing microservices platform that automatically detects failures, reroutes traffic, and recovers services without manual intervention.

**Requirements**:
- **Functional**: Service registration, health monitoring, automatic failover, load balancing
- **Non-functional**: 99.9% availability target, sub-minute recovery time

**Programming Steps**:
1. Implement service registry using etcd
2. Build health check system with configurable thresholds
3. Implement circuit breakers
4. Add automatic traffic rerouting on failures
5. Implement rate limiting per service
6. Add auto-scaling based on metrics
7. Build control plane for managing the platform
8. Add comprehensive monitoring and alerting

**Solution Suite**:
- Test automatic failover < 30 seconds
- Test circuit breaker opens after threshold
- Test traffic rerouting around failed nodes
- Test auto-scaling under load
- Test graceful shutdown

---

## Phase 5: Production Scenarios

**Weeks 15-18**  
**Goal**: Apply all knowledge to real-world production scenarios

---

### Chapter 12: Production System Design

**Week 15-16**  
**Time**: 15-20 hours

#### Learning Objectives

- Design systems for production scale
- Understand real-world challenges and solutions
- Learn from production incidents
- Build confidence in system design

#### Topics

1. **Case Studies**
   - Netflix: Global video streaming at scale
   - Uber: Real-time pricing and dispatch
   - Amazon: E-commerce recommendation engine
   - Cloudflare: CDN and DDoS protection

2. **Design Patterns**
   - Lambda architecture
   - Kappa architecture
   - Event sourcing
   - CQRS

3. **Scaling Patterns**
   - Horizontal vs vertical scaling
   - Database scaling
   - Caching strategies
   - CDN integration

4. **Chaos Engineering**
   - Principles
   - Chaos Monkey
   - GameDays

#### Resources

**Primary**:
- Amazon Builders' Library: https://aws.amazon.com/builders-library/
- Google SRE Book: https://sre.google/sre-book/table-of-contents/
- "The Site Reliability Workbook"

#### Exam 5: Twitter-like Distributed System

**Problem Statement**:  
Design and implement a Twitter-like distributed social media platform. The system must handle millions of users posting tweets, following other users, and viewing their timelines in real-time.

**Requirements**:
- **Functional**: Post tweets, follow/unfollow users, view timeline, like/retweet
- **Non-functional**: Handle 100k tweets/second, 1M reads/second, sub-100ms timeline generation

**Programming Steps**:
1. Design system architecture (compute, storage, cache, queue)
2. Set up data stores (Cassandra for tweets, Redis for timelines)
3. Implement tweet service with consistent hashing
4. Implement social graph storage and query
5. Build timeline generation service (pull vs push)
6. Add tweet search with Elasticsearch
7. Implement caching layer
8. Add media storage (S3 or similar)
9. Build real-time notification service
10. Add analytics/metrics pipeline
11. Implement content moderation pipeline
12. Add multi-region replication

**Solution Suite**:
- Test tweet posting latency < 100ms
- Test timeline generation < 200ms
- Test follows propagate to timelines correctly
- Test system handles 10x traffic spike
- Test data consistency after failures

---

### Chapter 13: Capstone Project

**Weeks 17-20**  
**Time**: 30-40 hours

#### Project Options

Choose one of the following:

**Option A: Distributed ML Training Platform**
- Build a parameter server for distributed machine learning
- Implement model parallelism and data parallelism
- Add fault tolerance for long-running training jobs
- Build web UI for monitoring training progress

**Option B: Distributed Search Engine**
- Implement inverted index
- Build query parser and ranking algorithm
- Add distributed indexing
- Implement real-time indexing of new documents

**Option C: Real-Time Collaborative Editor**
- Implement CRDT-based conflict resolution
- Build operational transformation or Yjs
- Add presence indicators
- Implement real-time sync across clients

**Option D: Distributed Job Scheduler**
- Build job queue with priority scheduling
- Implement worker node management
- Add DAG support (like Airflow)
- Build UI for job monitoring and debugging

#### Project Requirements

All projects must include:
1. Working implementation
2. Unit and integration tests
3. Documentation (README, architecture diagram)
4. Deployment to Kubernetes (or local equivalent)
5. Monitoring and observability
6. Presentation/demo

---

## Exam Specifications

### Exam 1: Distributed Configuration Manager
- **Duration**: 2 weeks
- **Technologies**: Raft (from Lab 5.1), HTTP API, etcd
- **Complexity**: Medium

### Exam 2: Distributed Photo Storage System
- **Duration**: 2 weeks
- **Technologies**: Cassandra, Redis, Kafka
- **Complexity**: High

### Exam 3: E-Commerce Event Sourcing System
- **Duration**: 2 weeks
- **Technologies**: Kafka, Kafka Streams, PostgreSQL
- **Complexity**: High

### Exam 4: Self-Healing Microservices Platform
- **Duration**: 2 weeks
- **Technologies**: etcd, Kubernetes, Prometheus
- **Complexity**: Very High

### Exam 5: Twitter-like Distributed System
- **Duration**: 3 weeks
- **Technologies**: Cassandra, Redis, Kafka, Elasticsearch, S3
- **Complexity**: Very High

---

## Tools & Environment Setup

### Required Tools

```bash
# Core
Python 3.10+
Go 1.21+ (for etcd, CockroachDB exploration)
Docker Desktop
kubectl
helm

# Databases
cassandra (via Docker)
redis (via Docker)
postgresql (via Docker)

# Messaging
kafka (via Docker: confluentinc/cp-kafka)
zookeeper (via Docker)

# Orchestration
minikube or kind (local Kubernetes)
etcd (via Docker)

# Monitoring
prometheus (via Docker)
grafana (via Docker)
jaeger (via Docker)
elasticsearch + kibana (via Docker)

# Observability
opentelemetry-collector

# Development
vscode / pycharm
git
tmux
```

### Recommended Docker Compose Setup

See `docker-compose.yml` in this repository for pre-configured local development environment.

---

## Assessment Criteria

### Each Exam Is Evaluated On:

| Criteria | Weight | Description |
|----------|--------|-------------|
| Functionality | 40% | Does it work as specified? |
| Code Quality | 20% | Clean code, proper abstractions, tests |
| Design Decisions | 20% | Are tradeoffs justified? |
| Documentation | 10% | README, comments, architecture |
| Performance | 10% | Meets non-functional requirements |

### Grading Scale

- **A**: Exceeds expectations, production-ready
- **B**: Meets all requirements with good quality
- **C**: Meets basic requirements
- **D**: Partial implementation
- **F**: Not meeting requirements

---

## Additional Resources

### Essential Papers (Must Read)

1. "The Google File System" - Ghemawat et al.
2. "MapReduce" - Dean et al.
3. "BigTable" - Chang et al.
4. "Dynamo" - Decandia et al.
5. "Spanner" - Corbett et al.
6. "Raft" - Ongaro and Ousterhout
7. "Kafka" - Narkhede et al.
8. "The Tail At Scale" - Dean et al.

### Community

- Distributed Systems reading group
- Papers We Love (GitHub)
- Hacker News
- r/distributedsystems

---

## Progress Tracking

Use the checklist below to track your progress:

- [ ] Chapter 1: Distributed Systems Fundamentals
- [ ] Chapter 2: Communication & Serialization
- [ ] Chapter 3: Consistency Models
- [ ] Chapter 4: Distributed Replication
- [ ] Chapter 5: Consensus & Raft
- [ ] Chapter 6: Distributed Databases
- [ ] Chapter 7: Message Queues & Event Streaming
- [ ] Chapter 8: Stream Processing Frameworks
- [ ] Chapter 9: Service Discovery & Load Balancing
- [ ] Chapter 10: Container Orchestration
- [ ] Chapter 11: Observability & Monitoring
- [ ] Chapter 12: Production System Design
- [ ] Chapter 13: Capstone Project

---

*Last Updated: March 2026*  
*Version: 1.0*
