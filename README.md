# Designing-Data-Intensive-Applications
A visual journey through "Designing Data-Intensive Applications" (Martin Kleppmann). Each chapter's key concepts are distilled into digestible summaries with accompanying diagrams covering reliability, scalability, maintainability, and modern data system architecture.

---

# Chapter 1: Foundations of Data Systems

<img width="1919" height="975" alt="1 0" src="https://github.com/user-attachments/assets/4cc3f40c-17f9-465d-ab5d-429f7d06cbf1" />

---
## Chapter Overview

<img width="1855" height="1079" alt="1 1" src="https://github.com/user-attachments/assets/fcdbea8c-ab9e-442b-8782-d44ef15e79af" />

**Fundamental Concepts Covered**: This chapter introduces the essential building blocks of modern data systems including storage solutions, processing paradigms, data replication strategies, consistency models, and the trade-offs between SQL and NoSQL approaches. Understanding these fundamentals is crucial for designing robust, scalable applications.

---
## System Design Goals

<img width="1246" height="690" alt="1 4" src="https://github.com/user-attachments/assets/dcc0ae37-3cbd-4344-a34d-ae5e4b312ca8" />

**The Three Pillars of System Design**: When building any data-intensive application, we aim to achieve three core objectives:
- **Reliability**: The system should work correctly even when things go wrong
- **Scalability**: The system should handle growth in traffic and data volume
- **Maintainability**: The system should be easy to operate, understand, and modify over time

---
## Reliability Engineering

<img width="1246" height="682" alt="1 5" src="https://github.com/user-attachments/assets/ecf6b1d1-75d5-4b2a-b4e3-9eb7693f2886" />

**Fault vs. Failure**: A system is considered **reliable** when it can tolerate **faults** (component malfunctions) without causing a **failure** (complete system breakdown). Building fault-tolerant systems involves anticipating and designing for various types of problems.

---
<img width="1246" height="678" alt="1 6" src="https://github.com/user-attachments/assets/7d9cd360-38ea-46ba-bc38-598421124d65" />

**Sources of System Failures**: Understanding different fault categories helps in designing appropriate safeguards:
- **Hardware Faults**: Disk failures, memory errors, network partitions
- **Software Faults**: Bugs, configuration errors, resource leaks
- **Human Errors**: Misconfigurations, accidental deletions, deployment mistakes

*Proactive testing like Netflix's Chaos Monkey intentionally induces failures to validate system resilience.*

---
## Scalability Principles

<img width="1243" height="685" alt="1 7" src="https://github.com/user-attachments/assets/5d9ec2b4-35dd-4560-850f-ee19c5530006" />

**Handling Increased Load**: Scalability refers to a system's ability to maintain performance under growing workload. Load parameters vary by application - for Twitter, key metrics include tweet posting rates (4.6K average, 12K+ peak) and timeline requests (300K/sec).

---
<img width="1239" height="675" alt="1 8" src="https://github.com/user-attachments/assets/9414e949-4745-4088-bcaf-3849ddeac750" />

**Real-World Scaling Challenge**: Twitter's initial architecture struggled with the "fan-out" problem - distributing each new tweet to all followers' timelines. Their solution evolved from simple database queries to a hybrid approach combining pre-computed timelines with on-demand fetching.

---
<img width="1245" height="683" alt="1 10" src="https://github.com/user-attachments/assets/a2160b8a-1007-49ae-b605-b97f2fb3ead6" />

**Quantifying Performance**: Response time is a critical metric composed of network time, queuing time, and service time. Using percentiles (95th, 99th) provides better insight than averages. Business impact is significant - Amazon found that every 100ms delay reduces sales by 1%.

---
<img width="1243" height="679" alt="1 9" src="https://github.com/user-attachments/assets/4ee4fb7d-893d-4638-933e-b81105257bff" />

**System Behavior Under Load**: As load increases, systems may exhibit degraded performance, head-of-line blocking, or resource exhaustion. Understanding the relationship between throughput, latency, and resource utilization is essential for capacity planning.

---
## Scaling Strategies

<img width="1245" height="676" alt="1 11" src="https://github.com/user-attachments/assets/f7954d75-fea6-41a7-8f7f-1b2da1f82e4e" />

**Vertical vs. Horizontal Scaling**: Two fundamental approaches to handle growth:
- **Vertical Scaling (Scaling Up)**: Adding more resources (CPU, RAM, storage) to a single machine
- **Horizontal Scaling (Scaling Out)**: Distributing load across multiple machines or instances

---
<img width="1237" height="683" alt="1 12" src="https://github.com/user-attachments/assets/05e0f01b-e575-4312-87eb-402dc9388d47" />

**No Universal Solution**: The optimal scaling strategy depends on multiple factors:
- Read/write volume ratios
- Response time requirements
- Data access patterns
- Data volume and complexity
- Storage and processing needs

---
## Maintainability and Operations

<img width="1238" height="683" alt="1 13" src="https://github.com/user-attachments/assets/af8b1c8e-3de3-413c-bfcf-08e11c3a606f" />

**Long-Term System Health**: Maintainable systems focus on:
- **Operability**: Easy monitoring, deployment, and troubleshooting
- **Simplicity**: Clean abstractions and modular design
- **Evolvability**: Ability to adapt to changing requirements

---
## Tool Diversity and Architecture

<img width="1919" height="895" alt="1 2" src="https://github.com/user-attachments/assets/a88d6bf4-0d1f-4cc7-97ac-7034b15c0d19" />

**Why So Many Tools?**: Different problems require specialized solutions:
- **Databases**: Structured data storage with ACID guarantees
- **Caches**: Low-latency data access
- **Search Engines**: Full-text search and relevance ranking
- **Stream Processing**: Real-time data analysis
- **Batch Processing**: Large-scale data transformation

---
<img width="1853" height="1079" alt="1 3" src="https://github.com/user-attachments/assets/55ebb26b-9640-4fac-ab8b-0d98d95c845d" />

**Example Architecture**
* User sends a GET request → checks cache → if miss, queries the database → returns data.
* Updates (POST requests) update both the database and auxiliary systems like cache and search indexes.
* Background jobs handle time-consuming tasks asynchronously to avoid blocking user interactions.

**Typical Data System Components**: Modern applications combine multiple specialized tools:
1. **Application Code** handling business logic
2. **Relational Databases** (PostgreSQL) for transactional data
3. **Search Engines** (Elasticsearch) for complex queries
4. **Message Queues** (RabbitMQ) for asynchronous processing
5. **Caching Layers** for performance optimization

---

* This chapter establishes the foundational mindset for designing data-intensive applications: there are no universally perfect solutions, only appropriate trade-offs. By understanding reliability patterns, scaling strategies, and maintenance considerations, engineers can make informed decisions about architecture and tool selection based on specific application requirements.

---

# Chapter 2: Data Models and Query Languages

## The Central Role of Data Models

<img width="2000" height="1125" alt="2 1" src="https://github.com/user-attachments/assets/1f6fb95a-aa69-465a-b5b4-0cb816cb6051" />

**Why Data Models Matter Most**: The data model is arguably the most critical part of software development because it defines how we conceptualize and interact with our data. A well-designed data model shapes our entire application architecture, determines what queries are efficient or awkward, and influences how we solve business problems.

---
## Data Model Abstraction Layers

<img width="2000" height="1125" alt="2 2" src="https://github.com/user-attachments/assets/96034d25-d91a-4221-8f42-1583fc4fac7f" />

**From Real World to Physical Storage**: Data models exist at multiple abstraction levels:
- **Layer 0**: Real-world entities (people, orders, companies)
- **Layer 1**: Application objects and data structures
- **Layer 2**: Data representations (JSON, XML, tables, graphs)
- **Layer 3**: Physical storage (bytes in RAM, files on disk)
- **Layer 4**: Hardware representation (electric currents, magnetic fields, light pulses)

Each layer must be efficiently mapped to the next, and the choice of data model affects these transformations significantly.

---
## The Relational Model: Tried and True

<img width="2000" height="1125" alt="2 3" src="https://github.com/user-attachments/assets/226b5480-ad04-45eb-ac41-5deeeed81425" />

**SQL's Dominant Paradigm**: The relational model organizes data into **relations** (tables) consisting of rows and columns. Born from 1960s-70s business data processing, its genius lies in hiding implementation details behind a clean interface. It excels at both **transactional processing** (ACID-compliant operations) and **batch processing** (large-scale data transformations).

### Transactional Processing in Action

<img width="2000" height="1125" alt="2 4" src="https://github.com/user-attachments/assets/7c50c8cb-83d2-4cac-9077-3d98235b533c" />

**Guaranteeing Data Integrity**: Transactional processing ensures operations either complete fully or not at all. This SQL transaction shows the database's atomicity - it begins, executes multiple operations, and commits only if all succeed. The rollback example demonstrates how failures are handled gracefully without corrupting data.

---
## The Relational vs Document Model Comparison

### Relational Approach: Normalized Tables

<img width="481" height="635" alt="2 6" src="https://github.com/user-attachments/assets/213d4c83-0e1d-48c6-992a-da9b18cefcff" />

**Bill Gates' LinkedIn Profile in SQL**: This normalized relational schema separates data into multiple tables (users, regions, industries, positions, education, contact_info) with foreign key relationships. This design minimizes redundancy and maintains data integrity but requires JOIN operations to assemble complete records.

### Document Approach: Self-Contained Documents

<img width="2000" height="1125" alt="2 7" src="https://github.com/user-attachments/assets/9d179468-da22-4f53-96f9-a26953f8340c" />

**The Same Profile as JSON**: Document databases store related information together in a single JSON document. This provides better **locality** - all Bill Gates' data loads at once without JOINs. However, it duplicates data (region names appear in multiple documents) and makes many-to-many relationships more challenging.

---
## The Rise of NoSQL

<img width="2000" height="1125" alt="2 5" src="https://github.com/user-attachments/assets/1cb9e6b2-dba6-4014-b2c2-450b7df81ae2" />

**Challenging Relational Dominance**: NoSQL emerged to address specific limitations:
- **Scalability issues** with traditional relational databases
- **Schema flexibility** needs for rapidly evolving applications
- **Specialized query patterns** not well-supported by SQL
- **Distributed architectures** by default (sharding, replication)

NoSQL isn't "anti-SQL" but rather "not only SQL," embracing polyglot persistence where different data stores serve different purposes.

---
## Historical Echoes: Are Document Databases New?

<img width="2000" height="1125" alt="2 8" src="https://github.com/user-attachments/assets/50ff9b17-2041-418a-b3e0-ecc5a1f42bef" />

**History Repeating Itself?**: Document databases surprisingly resemble the **hierarchical** and **network** models that relational databases displaced in the 1980s. The debate over nested documents (hierarchical) versus normalized tables (relational) echoes earlier database wars, with modern document databases incorporating lessons from both historical approaches.

---

## Key Trade-offs & Decision Framework

### When to Choose Document Databases:
- **Self-contained documents** with one-to-many relationships
- **Rapid schema evolution** requirements
- **Read-heavy workloads** where locality improves performance
- **Simple access patterns** without complex joins
- **Developer productivity** with JSON-native applications

### When to Choose Relational Databases:
- **Complex many-to-many relationships**
- **Strong data integrity** and consistency requirements
- **Ad-hoc querying** needs
- **Well-understood, stable schemas**
- **Transaction-heavy applications** (banking, e-commerce)

---
