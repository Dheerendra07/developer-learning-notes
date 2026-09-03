# 🗄️ Database Sharding — From Scratch to MongoDB

A complete, beginner-friendly learning repository that takes you from **database fundamentals to practical MongoDB sharding and distributed database concepts**.

`MongoDB` `DBMS` `System Design` `Backend` `Sharding` `Horizontal Scaling` `Distributed Databases`

---

## 📖 Introduction

This repository is a structured learning guide to **Database Sharding**, created for students and developers who want to understand how databases scale from a single server to distributed systems.

The learning journey starts with basic database concepts and gradually moves toward MongoDB's sharded-cluster architecture and practical implementation.

```text
Database Basics
      ↓
Database Scaling
      ↓
Partitioning
      ↓
Replication
      ↓
Horizontal Scaling
      ↓
Sharding
      ↓
MongoDB Sharding
      ↓
Sharded Cluster
      ↓
Query Routing
      ↓
System Design
```

You do **not** need prior knowledge of sharding or distributed databases. Every concept is introduced from the ground up and becomes progressively more technical.

---

## 🎯 Who Is This For?

This repository is useful for:

* 🎓 B.Tech / Computer Science students learning DBMS
* 🛠️ Backend developers working with MongoDB
* 🧠 Developers preparing for System Design interviews
* 📚 Anyone learning distributed database concepts
* 🚀 Anyone interested in how large datasets are distributed across multiple servers

---

## 🚀 What You Will Learn

By completing this repository, you should be able to:

* Understand databases and DBMS fundamentals
* Understand vertical and horizontal scaling
* Differentiate partitioning and replication
* Explain database sharding from first principles
* Understand shard keys and their importance
* Compare range-based and hashed sharding
* Understand MongoDB's sharded-cluster architecture
* Explain `mongos`, config servers, shards, chunks, and balancing
* Build a small local MongoDB sharded cluster
* Perform basic sharding operations
* Understand targeted queries and scatter-gather queries
* Identify common shard-key mistakes
* Design shard keys based on application workload
* Apply sharding concepts to real-world system design

> 💡 **Core idea:** Sharding distributes a collection's data across multiple shards. The shard key influences how MongoDB distributes and routes that data.

---

# 📑 Table of Contents

1. [What is a Database?](#1-what-is-a-database)
2. [What Happens When Data Becomes Huge?](#2-what-happens-when-data-becomes-huge)
3. [What is Database Scaling?](#3-what-is-database-scaling)
4. [Vertical vs Horizontal Scaling](#4-vertical-vs-horizontal-scaling)
5. [What is Database Partitioning?](#5-what-is-database-partitioning)
6. [What is Database Replication?](#6-what-is-database-replication)
7. [Replication vs Partitioning vs Sharding](#7-replication-vs-partitioning-vs-sharding)
8. [What is Sharding?](#8-what-is-sharding)
9. [Why Do We Need Sharding?](#9-why-do-we-need-sharding)
10. [Sharding Terminology](#10-sharding-terminology)
11. [What is a Shard Key?](#11-what-is-a-shard-key)
12. [Characteristics of a Good Shard Key](#12-characteristics-of-a-good-shard-key)
13. [Bad Shard Key Examples](#13-bad-shard-key-examples)
14. [Shard Key Selection Checklist](#14-shard-key-selection-checklist)
15. [Range-Based Sharding](#15-range-based-sharding)
16. [Hash-Based Sharding](#16-hash-based-sharding)
17. [Directory-Based & Compound Sharding](#17-directory-based--compound-sharding)
18. [MongoDB Sharding Overview](#18-mongodb-sharding-overview)
19. [How MongoDB Sharding Works](#19-how-mongodb-sharding-works)
20. [Targeted Query vs Scatter-Gather](#20-targeted-query-vs-scatter-gather)
21. [Chunks and Balancing](#21-chunks-and-balancing)
22. [Practical: Local MongoDB Sharded Cluster](#22-practical-local-mongodb-sharded-cluster)
23. [Important Sharding Commands](#23-important-sharding-commands)
24. [Code Example: Distributed Quiz Attempts](#24-code-example-distributed-quiz-attempts)
25. [Combining Sharding and Replication](#25-combining-sharding-and-replication)
26. [Performance & Real-World Design](#26-performance--real-world-design)
27. [Common Sharding Mistakes](#27-common-sharding-mistakes)
28. [Advantages and Disadvantages](#28-advantages-and-disadvantages)
29. [Interview Preparation](#29-interview-preparation)
30. [Hands-On Mini Project](#30-hands-on-mini-project)
31. [Learning Path & Cheat Sheet](#31-learning-path--cheat-sheet)
32. [FAQ & Final Summary](#32-faq--final-summary)
33. [Resources](#33-resources)

---

# 1. What is a Database?

A **database** is an organized collection of data that can be stored, accessed, updated, and managed efficiently.

A **Database Management System (DBMS)** is software that provides the tools required to create, store, retrieve, update, and manage data.

```text
Database
   │
   ├── Relational
   │     ├── MySQL
   │     ├── PostgreSQL
   │     └── SQL Server
   │
   └── NoSQL
         ├── Document → MongoDB
         ├── Key-Value → Redis
         ├── Wide-Column → Cassandra
         └── Graph → Neo4j
```

## Relational Database

Relational databases organize data into tables.

```text
Users

+----+--------+-----+
| id | name   | age |
+----+--------+-----+
| 1  | Rahul  | 21  |
| 2  | Aman   | 22  |
| 3  | Priya  | 20  |
+----+--------+-----+
```

## MongoDB

MongoDB is a document-oriented NoSQL database.

It stores BSON documents inside collections.

```javascript
{
  userId: 1001,
  name: "Rahul",
  age: 21,
  course: "DBMS"
}
```

MongoDB's basic hierarchy can be understood as:

```text
MongoDB Deployment
      ↓
Database
      ↓
Collection
      ↓
Document
      ↓
Fields
```

### 🧠 Key Takeaway

> A database stores data, while a DBMS provides the software used to manage that data.

---

# 2. What Happens When Data Becomes Huge?

An application can start with a small database and gradually grow into a system handling millions or even billions of records.

```text
1,000 users
    ↓
10,000 users
    ↓
1 Million users
    ↓
100 Million users
    ↓
1 Billion+ records
```

An important point:

> **The number of users is not the same as the amount of data.**

For example, an application with 1 million users may generate billions of activity, transaction, log, or event records.

## Single-Server Bottleneck

```text
                 Application
                      │
                      ▼
              ┌──────────────┐
              │   Database   │
              │              │
              │ CPU          │
              │ RAM          │
              │ Storage      │
              │ Disk I/O     │
              │ Network      │
              └──────────────┘
```

As workload increases, a single server may become constrained by:

* CPU
* RAM
* Storage capacity
* Disk I/O
* Network bandwidth
* Query throughput
* Write throughput
* Query latency

### Example

Suppose:

```text
Database Size     = 5 TB
Available Storage = 4 TB
```

The database cannot continue growing indefinitely on that server.

Similarly:

```text
Required write throughput = 100,000 writes/sec
Single server capacity    = 20,000 writes/sec
```

At this point, simply optimizing queries may not be enough.

The architecture may need to distribute the workload.

### 🧠 Key Takeaway

> At large scale, the problem can become not only **"How do I optimize the query?"**, but also **"How do I distribute the data and workload?"**

---

# 3. What is Database Scaling?

**Scaling** means increasing the capacity of a system so that it can handle more data, users, requests, or workload.

There are two major approaches:

1. Vertical Scaling
2. Horizontal Scaling

## Vertical Scaling

Also called **Scale-Up**.

You make one server more powerful.

```text
Small Server
4 CPU
16 GB RAM
1 TB Storage
       ↓
       ↓
More Resources
       ↓
64 CPU
512 GB RAM
20 TB Storage
```

### Advantages

* Simple architecture
* Fewer servers
* Easier to manage
* Usually requires fewer application changes

### Disadvantages

* Hardware has limits
* Powerful machines can become expensive
* Eventually constrained by the capacity of one machine

---

## Horizontal Scaling

Also called **Scale-Out**.

Instead of continuously making one server larger, you add more servers.

```text
Server 1
Server 2
Server 3
Server 4
Server 5
```

Workload or data can then be distributed across these machines.

### Advantages

* Capacity can grow by adding nodes
* Suitable for distributed architectures
* Storage and workload can be distributed

### Disadvantages

* Higher operational complexity
* Distributed systems are harder to design
* Routing and data distribution become important

### 🧠 Key Takeaway

> Vertical scaling makes one machine stronger. Horizontal scaling adds more machines.

---

# 4. Vertical vs Horizontal Scaling

| Aspect            | Vertical Scaling          | Horizontal Scaling             |
| ----------------- | ------------------------- | ------------------------------ |
| Also called       | Scale-Up                  | Scale-Out                      |
| Approach          | Increase server resources | Add more servers               |
| Architecture      | Usually single-node       | Distributed                    |
| Complexity        | Lower                     | Higher                         |
| Hardware limit    | Yes                       | Capacity grows by adding nodes |
| Data distribution | Usually unnecessary       | Often required                 |
| Example           | Add RAM/CPU               | Add database nodes             |

> 💡 Sharding is one technique used to distribute database data across multiple servers for horizontal scalability.

---

# 5. What is Database Partitioning?

**Partitioning** means dividing a large logical dataset into smaller pieces called partitions.

Partitioning can make large datasets easier to manage and can allow operations to work with smaller portions of data.

## Horizontal Partitioning

Rows or documents are divided.

```text
Users

Partition 1 → Users 1–1000
Partition 2 → Users 1001–2000
Partition 3 → Users 2001–3000
```

The structure of the records remains broadly the same.

---

## Vertical Partitioning

Fields or columns are divided.

Original data:

```javascript
{
  userId,
  name,
  email,
  address,
  bio,
  profilePicture,
  activity
}
```

Can conceptually become:

```text
User Core
→ userId, name

User Profile
→ email, address, bio

User Activity
→ activity
```

### Partitioning vs Sharding

Partitioning is the broader concept of dividing data.

Sharding commonly refers to distributing partitions of a collection across multiple database servers.

```text
Logical Dataset
       ↓
   Partitions
       ↓
Multiple Shards
```

### 🧠 Key Takeaway

> Horizontal partitioning divides records. Vertical partitioning divides attributes. Sharding distributes data partitions across multiple database servers.

---

# 6. What is Database Replication?

**Replication** means maintaining copies of data on multiple database nodes.

Conceptually:

```text
                 Primary
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      Secondary           Secondary
```

Replication is primarily useful for:

* High availability
* Fault tolerance
* Failover
* Additional read capacity in appropriate configurations

### Important Difference

Suppose the dataset is:

```text
5 TB
```

With three full copies, the cluster may store approximately:

```text
5 TB × 3 = 15 TB
```

Replication **copies** data.

It does not divide the original dataset into independent subsets.

### 🧠 Key Takeaway

> Replication mainly answers: **"What happens if a database node fails?"**

---

# 7. Replication vs Partitioning vs Sharding

| Concept      | Main Idea                     | Primary Purpose                          |
| ------------ | ----------------------------- | ---------------------------------------- |
| Replication  | Copy data                     | Availability / fault tolerance           |
| Partitioning | Divide data logically         | Data organization / scalability          |
| Sharding     | Distribute data across shards | Horizontal data/storage/workload scaling |

```text
REPLICATION

[A B C]
  ↓
[A B C] [A B C] [A B C]


SHARDING

[A B C]
  ↓
[A] [B] [C]
```

They can also be combined:

```text
              Sharded Cluster
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      Shard 1     Shard 2     Shard 3
        │           │           │
    Replica Set  Replica Set  Replica Set
```

### 🧠 Remember

> **Replication copies. Sharding distributes.**

---

# 8. What is Sharding?

**Sharding is a database architecture in which a collection's data is distributed across multiple shards.**

Each shard contains a subset of the total data.

```text
                Complete Dataset
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Shard 1      Shard 2      Shard 3
       Part A       Part B       Part C
```

## Real-World Analogy

Imagine a library containing one million books.

Instead of storing every book in one enormous building:

```text
Building A → Science
Building B → History
Building C → Literature
```

A rule determines which building contains a particular book.

In a sharded database, the **shard key** plays an important role in determining data placement and query routing.

### Important

Sharding is not simply:

> "Put server 1's data here and server 2's data there."

A sharded database needs mechanisms for:

* Data distribution
* Data placement
* Query routing
* Metadata management
* Balancing

### 🧠 Key Takeaway

> Sharding distributes a large logical dataset across multiple database nodes so the system can scale horizontally.

---

# 9. Why Do We Need Sharding?

Sharding can help when a workload exceeds what a single database server can reasonably handle.

Possible reasons include:

## 1. Storage

```text
Dataset = 50 TB
Single server = insufficient
```

The dataset can be distributed across multiple shards.

## 2. Write Throughput

```text
Very high write workload
        ↓
Single-server bottleneck
        ↓
Distribute writes across shards
```

## 3. Large Working Set

If the active dataset becomes too large for the resources available on one server, distributing data can allow each shard to manage a smaller subset.

## 4. Horizontal Growth

Additional shards can provide additional storage and processing capacity.

---

## When Should You NOT Shard?

Do not shard simply because:

> "Sharding is an advanced technology."

Avoid unnecessary sharding when:

* Data comfortably fits on one server
* Workload is manageable
* Vertical scaling is sufficient
* Replication solves the availability requirement
* Operational complexity is not justified

### 🧠 Rule

> **Solve the actual scaling problem, not an imaginary one.**

---

# 10. Sharding Terminology

| Term               | Meaning                                                        |
| ------------------ | -------------------------------------------------------------- |
| **Shard**          | A database deployment responsible for a subset of sharded data |
| **Shard Key**      | Field or fields used to distribute and route data              |
| **Chunk**          | A range of shard-key values managed as a unit of distribution  |
| **`mongos`**       | MongoDB query router                                           |
| **Config Server**  | Stores metadata about the sharded cluster                      |
| **Balancer**       | Helps maintain appropriate data distribution                   |
| **Targeted Query** | Query that can be routed to relevant shard(s)                  |
| **Scatter-Gather** | Query sent to multiple/all relevant shards                     |
| **Zone**           | Associates shard-key ranges with specific shards               |

---

# 11. What is a Shard Key?

The **shard key** is a field or set of fields MongoDB uses to distribute documents across shards and route queries.

Example:

```javascript
{
  userId: 101,
  name: "Rahul",
  age: 21
}
```

Suppose:

```text
Shard Key = userId
```

MongoDB uses the shard-key value to determine data placement and query routing.

## Single-Field Shard Key

```javascript
{ userId: 1 }
```

## Hashed Shard Key

```javascript
{ userId: "hashed" }
```

## Compound Shard Key

```javascript
{ tenantId: 1, userId: 1 }
```

### 🧠 Key Takeaway

> The shard key is one of the most important decisions in a sharded database because it affects both data distribution and query routing.

---

# 12. Characteristics of a Good Shard Key

A good shard key should always be evaluated against the application's actual workload.

## 1. Cardinality

**Cardinality** means the number of distinct values.

Example:

```text
gender  → very low cardinality
country → relatively low cardinality
userId  → potentially millions of values
```

Higher cardinality generally provides more opportunities for distributing data.

---

## 2. Even Distribution

The key should avoid concentrating a disproportionate amount of data on one shard.

```text
Good:

Shard 1 → 33%
Shard 2 → 34%
Shard 3 → 33%
```

---

## 3. Query Patterns

If common queries include the shard key, MongoDB can often target the relevant shard(s).

Example:

```javascript
db.users.find({ userId: 101 })
```

---

## 4. Write Distribution

The shard key should not unnecessarily concentrate new writes on one shard.

---

## 5. Avoid Hotspots

A poor shard key can create a **hot shard**, where one shard receives most of the workload.

### 🧠 Important

> There is no universally perfect shard key. The right choice depends on the application's data and query patterns.

---

# 13. Bad Shard Key Examples

A field is not automatically "bad."

It becomes a poor choice **for a particular workload**.

| Field                 | Potential Problem          |
| --------------------- | -------------------------- |
| `gender`              | Very low cardinality       |
| `country`             | Data may be highly skewed  |
| Increasing timestamp  | Can concentrate new writes |
| Boolean/status fields | Too few distinct values    |

## Monotonically Increasing Key

Suppose new documents use:

```text
1001
1002
1003
1004
1005
...
```

With a ranged strategy, new values can concentrate at the active end of the key space.

This can create a write hotspot.

For workloads where a monotonically increasing field is required, hashed sharding can sometimes improve distribution, although this can make range queries less naturally targetable.

---

# 14. Shard Key Selection Checklist

Before selecting a shard key:

```text
1. Understand application queries
            ↓
2. Identify common access patterns
            ↓
3. Check cardinality
            ↓
4. Check data distribution
            ↓
5. Analyze write patterns
            ↓
6. Check hotspot risk
            ↓
7. Check range-query requirements
            ↓
8. Test using realistic data
```

Ask:

```text
Does this key distribute data?

Does this key support important queries?

Can it handle future growth?

Can it avoid hotspots?
```

### 🧠 Golden Rule

> **Choose a shard key based on workload, not just on the field with the highest cardinality.**

---

# 15. Range-Based Sharding

**Range-based sharding** divides the shard-key value space into ranges.

Example:

```text
Shard 1 → 1 – 1,000
Shard 2 → 1,001 – 2,000
Shard 3 → 2,001 – 3,000
```

Conceptually:

```text
Shard Key Space

1 ------------------------------ 3000
|              |                 |
Shard 1        Shard 2           Shard 3
```

MongoDB manages ranges of shard-key values as chunks and distributes those ranges across shards.

## Advantages

* Good fit for range-oriented access patterns
* Related key values can remain close together
* Suitable shard-key ranges can be targeted efficiently

## Disadvantages

* Poor keys can cause uneven distribution
* Monotonically increasing keys can create write hotspots

### Example

```javascript
db.orders.find({
  orderId: {
    $gte: 1000,
    $lte: 2000
  }
})
```

When `orderId` is the shard key and the query aligns with the shard-key ranges, MongoDB can potentially target the relevant shard(s).

### 🧠 Key Takeaway

> Range sharding preserves the ordering of shard-key values.

---

# 16. Hash-Based Sharding

In hashed sharding, MongoDB hashes the shard-key value before using the hashed value for distribution.

```text
Original Value
      ↓
Hash Function
      ↓
Hashed Value
      ↓
Shard-Key Range
      ↓
Shard
```

Example:

```javascript
sh.shardCollection(
  "learning_db.users",
  { userId: "hashed" }
)
```

MongoDB calculates the hash automatically.

## Advantages

* Generally provides more even distribution
* Can help distribute monotonically increasing values
* Reduces some hotspot risks

## Disadvantages

* Range queries on the original values are less naturally targetable
* Some queries may require multiple shards to participate

### 🧠 Key Takeaway

> Range sharding preserves ordering. Hashed sharding prioritizes distribution.

---

# 17. Directory-Based & Compound Sharding

## Directory-Based Sharding

Directory-based sharding uses a separate mapping mechanism to determine where data belongs.

Conceptually:

```text
Key
 ↓
Directory / Mapping
 ↓
Shard
```

This approach can provide flexible placement, but the mapping system itself becomes another component that must be maintained.

MongoDB provides **zones** for associating shard-key ranges with particular shards.

---

## Compound Shard Keys

A compound shard key contains multiple fields.

Example:

```javascript
{
  tenantId: 1,
  userId: 1
}
```

The order of fields matters.

For:

```javascript
{
  tenantId: 1,
  userId: 1
}
```

this query contains the shard-key prefix:

```javascript
{
  tenantId: "tenant-1",
  userId: 101
}
```

But this query only contains:

```javascript
{
  userId: 101
}
```

It does not contain the shard-key prefix and therefore may require broadcasting to multiple shards.

### 🧠 Remember

> With compound shard keys, **field order matters**.

---

# 18. MongoDB Sharding Overview

A MongoDB sharded cluster contains several major components.

```text
                       Application
                            │
                            ▼
                         mongos
                      Query Router
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
          Shard 1        Shard 2        Shard 3
             │              │              │
        Replica Set    Replica Set    Replica Set
```

And:

```text
              Config Server Replica Set
                       │
                       ▼
                Cluster Metadata
```

## Main Components

### `mongos`

The query router.

It receives client operations and routes them to the appropriate shard(s).

### Config Servers

Config servers store metadata describing the sharded cluster, including information about shards and data ranges.

### Shards

Shards store the actual application data.

For production deployments, shards are commonly deployed as replica sets for high availability.

### Balancer

MongoDB can move data ranges between shards to maintain appropriate distribution.

---

# 19. How MongoDB Sharding Works

Consider:

```text
Application
     │
     ▼
   mongos
     │
     ▼
Cluster Metadata
     │
     ▼
Determine Target Shard(s)
     │
     ├──────────────┐
     ▼              ▼
 Shard 1         Shard 2
     │              │
     └──────┬───────┘
            ▼
       Merge Results
            │
            ▼
        Application
```

`mongos` uses sharding metadata to determine where operations should be sent.

Applications normally connect to the **`mongos` router** when working with a sharded cluster.

### 🧠 Simplified Flow

```text
Client
  ↓
mongos
  ↓
Shard Key Analysis
  ↓
Target Shard(s)
  ↓
Database Operation
  ↓
Result
```

---

# 20. Targeted Query vs Scatter-Gather

## Targeted Query

A query that provides enough shard-key information for `mongos` to identify the relevant shard(s).

Example:

```javascript
db.users.findOne({
  userId: 101
})
```

If:

```text
Shard Key = userId
```

MongoDB can use the shard-key information for targeting.

```text
          mongos
             │
             ▼
          Shard 2
             │
             ▼
           Result
```

---

## Scatter-Gather

Suppose:

```javascript
db.users.find({
  age: 21
})
```

and `age` is not part of the shard key.

The query may need to be sent to multiple/all relevant shards.

```text
             mongos
           /   |   \
          ↓    ↓    ↓
       Shard1 Shard2 Shard3
          \    |    /
           \   |   /
            Results
               ↓
          mongos merges
               ↓
          Application
```

### 🧠 Key Takeaway

> A good shard key can make important application queries targetable.

---

# 21. Chunks and Balancing

## What is a Chunk?

MongoDB divides the shard-key value space into ranges.

These ranges are managed as units of distribution.

Conceptually:

```text
Shard Key Space

Range A → Chunk 1
Range B → Chunk 2
Range C → Chunk 3
Range D → Chunk 4
```

Chunks/ranges are distributed across shards.

MongoDB can split and migrate ranges as the cluster changes.

---

## Balancing

Imagine:

```text
Before:

Shard 1 → 10 chunks
Shard 2 → 2 chunks
Shard 3 → 3 chunks
```

MongoDB can move ranges between shards when necessary.

Conceptually:

```text
After:

Shard 1 → 5 chunks
Shard 2 → 5 chunks
Shard 3 → 5 chunks
```

The exact distribution depends on the collection, shard key, zones, cluster state, and MongoDB version/configuration.

> ⚠️ Do not assume a fixed chunk size such as `128 MB` for every MongoDB deployment. Chunk behavior and balancing details are version and configuration dependent.

---

# 22. Practical: Local MongoDB Sharded Cluster

> ⚠️ This tutorial is for **local learning only**. It is not a production deployment.

For production, use a properly designed deployment with replica sets, authentication, security, TLS where appropriate, monitoring, backups, capacity planning, and tested recovery procedures.

## Architecture

For this tutorial:

```text
Config Server
     │
     ▼
   mongos
    /   \
   /     \
Shard 1  Shard 2
```

Each shard in this tutorial uses a single-node replica set.

---

## Step 1 — Create Directories

Linux/macOS:

```bash
mkdir -p ~/mongo-shard-tutorial/{configdb,shard1,shard2}
```

Windows users can create equivalent folders manually.

---

## Step 2 — Start Config Server

```bash
mongod \
  --configsvr \
  --replSet cfgrs \
  --port 27019 \
  --dbpath ~/mongo-shard-tutorial/configdb \
  --bind_ip localhost
```

Open another terminal:

```bash
mongosh --port 27019
```

Initialize:

```javascript
rs.initiate({
  _id: "cfgrs",
  configsvr: true,
  members: [
    {
      _id: 0,
      host: "localhost:27019"
    }
  ]
})
```

---

## Step 3 — Start Shard 1

```bash
mongod \
  --shardsvr \
  --replSet shard1rs \
  --port 27018 \
  --dbpath ~/mongo-shard-tutorial/shard1 \
  --bind_ip localhost
```

Initialize:

```bash
mongosh --port 27018
```

```javascript
rs.initiate({
  _id: "shard1rs",
  members: [
    {
      _id: 0,
      host: "localhost:27018"
    }
  ]
})
```

---

## Step 4 — Start Shard 2

```bash
mongod \
  --shardsvr \
  --replSet shard2rs \
  --port 27020 \
  --dbpath ~/mongo-shard-tutorial/shard2 \
  --bind_ip localhost
```

Initialize:

```bash
mongosh --port 27020
```

```javascript
rs.initiate({
  _id: "shard2rs",
  members: [
    {
      _id: 0,
      host: "localhost:27020"
    }
  ]
})
```

---

## Step 5 — Start `mongos`

Open another terminal:

```bash
mongos \
  --configdb cfgrs/localhost:27019 \
  --port 27017 \
  --bind_ip localhost
```

---

## Step 6 — Connect Through `mongos`

```bash
mongosh --port 27017
```

Add the shards:

```javascript
sh.addShard("shard1rs/localhost:27018")
sh.addShard("shard2rs/localhost:27020")
```

Check:

```javascript
sh.status()
```

---

## Step 7 — Create a Collection

```javascript
use learning_db

db.users.insertOne({
  userId: 1,
  name: "Rahul",
  age: 21
})
```

---

## Step 8 — Create Supporting Index

For a populated collection, create an index that supports the shard key before sharding it.

For a hashed shard key:

```javascript
db.users.createIndex({
  userId: "hashed"
})
```

---

## Step 9 — Shard the Collection

```javascript
sh.shardCollection(
  "learning_db.users",
  {
    userId: "hashed"
  }
)
```

> 💡 On modern MongoDB versions, a separate `sh.enableSharding()` step is not required simply to shard a collection.

---

## Step 10 — Insert Sample Data

```javascript
for (let i = 1; i <= 100000; i++) {
  db.users.insertOne({
    userId: i,
    name: "User" + i,
    age: 18 + (i % 30)
  })
}
```

---

## Step 11 — Check Distribution

```javascript
db.users.getShardDistribution()
```

Do **not** assume an exact 50/50 distribution.

Actual distribution depends on:

* Shard key
* Data
* Chunks
* Balancing
* Cluster state
* MongoDB version
* Configuration

---

## Step 12 — Check Cluster Status

```javascript
sh.status()
```

This provides information about the current sharded cluster.

---

# 23. Important Sharding Commands

> ⚠️ Command availability and behavior can vary by MongoDB version. Always verify commands against the documentation for the version you are using.

| Command                                | Purpose                           |
| -------------------------------------- | --------------------------------- |
| `sh.status()`                          | Display sharded-cluster status    |
| `sh.addShard()`                        | Add a shard                       |
| `sh.shardCollection()`                 | Shard a collection                |
| `sh.reshardCollection()`               | Reshard a collection              |
| `db.collection.getShardDistribution()` | Inspect distribution              |
| `sh.getBalancerState()`                | Check balancer state              |
| `sh.isBalancerRunning()`               | Check whether balancing is active |
| `sh.startBalancer()`                   | Start the balancer                |
| `sh.stopBalancer()`                    | Stop the balancer                 |
| `db.printShardingStatus()`             | Display sharding status           |

### Examples

```javascript
sh.status()
```

```javascript
sh.getBalancerState()
```

```javascript
sh.isBalancerRunning()
```

```javascript
db.users.getShardDistribution()
```

## Resharding

MongoDB supports changing the shard key through resharding and also supports refining an existing shard key by adding suffix fields.

Example:

```javascript
sh.reshardCollection(
  "learning_db.users",
  {
    key: {
      tenantId: 1,
      userId: 1
    }
  }
)
```

> ⚠️ Always check the MongoDB version-specific syntax and operational requirements before using resharding in a real environment.

---

# 24. Code Example: Distributed Quiz Attempts

Imagine an online learning platform storing quiz attempts.

```javascript
{
  attemptId: "att_9001",
  userId: 1001,
  quizId: 5001,
  score: 8,
  subject: "DBMS",
  createdAt: ISODate("2026-09-03T10:30:00Z")
}
```

Suppose the application's most common query is:

```text
"Show all quiz attempts made by user X."
```

Example:

```javascript
db.quiz_attempts.find({
  userId: 1001
})
```

This makes `userId` an interesting shard-key candidate.

---

## Candidate 1 — `userId`

```javascript
{
  userId: 1
}
```

Potential benefit:

```text
Query by userId
       ↓
Target relevant shard(s)
```

But ask:

> What happens if one user generates an extremely large amount of data?

That is why workload analysis is important.

---

## Candidate 2 — Compound Key

```javascript
{
  userId: 1,
  attemptId: 1
}
```

This may provide additional distribution characteristics.

However, adding a field does **not automatically make a shard key better**.

The correct choice depends on:

* Query patterns
* Data volume per user
* Distribution
* Write behavior
* Cardinality
* Range-query requirements

---

## Example Query

```javascript
db.quiz_attempts.find({
  userId: 1001
})
```

This contains the shard-key prefix and can be targetable.

Another query:

```javascript
db.quiz_attempts.find({
  subject: "DBMS"
})
```

does not contain the shard-key prefix and may require querying multiple shards.

### 🧠 Lesson

> Shard-key design should start from **how the application reads and writes data**, not simply from which field has the most values.

---

# 25. Combining Sharding and Replication

Sharding and replication solve different problems and can be combined.

```text
                    mongos
                      │
       ┌──────────────┼──────────────┐
       ▼              ▼              ▼
    Shard 1         Shard 2         Shard 3
       │              │              │
   Replica Set    Replica Set    Replica Set
       │              │              │
   ┌───┴───┐      ┌───┴───┐      ┌───┴───┐
 Primary Secondary Primary Secondary Primary Secondary
```

## Responsibilities

| Problem                   | Main Mechanism                                   |
| ------------------------- | ------------------------------------------------ |
| Distribute data           | Sharding                                         |
| Increase storage capacity | Sharding                                         |
| Distribute write workload | Sharding                                         |
| High availability         | Replication                                      |
| Failover                  | Replication                                      |
| Additional read capacity  | Replication, depending on workload/configuration |

### 🧠 Remember

```text
Sharding
   ↓
Scalability

Replication
   ↓
Availability
```

---

# 26. Performance & Real-World Design

Sharding does **not automatically make every query faster**.

Performance depends heavily on:

* Shard-key choice
* Query patterns
* Indexes
* Data distribution
* Network traffic
* Cross-shard operations
* Hardware
* Workload characteristics

## When Sharding Can Help

### Parallel Work

Different shards can process portions of a workload.

```text
Query
 ├──→ Shard 1
 ├──→ Shard 2
 └──→ Shard 3
```

### Data Distribution

Each shard stores only a portion of the total dataset.

### Write Distribution

An appropriately selected shard key can distribute writes across multiple shards.

---

## When Sharding Can Hurt

### Scatter-Gather

```text
mongos
 ├──→ Shard 1
 ├──→ Shard 2
 └──→ Shard 3
```

The query may require more network communication and result merging.

### Poor Shard Key

A poor shard key can produce:

```text
Shard 1 → 90%
Shard 2 → 5%
Shard 3 → 5%
```

The cluster has multiple servers, but the workload remains concentrated.

### Cross-Shard Operations

Operations involving data across many shards can be more complex and potentially more expensive.

---

# Architecture Evolution

A system might evolve conceptually like:

```text
Stage 1
Single Database
      ↓
Stage 2
Vertical Scaling
      ↓
Stage 3
Replication
      ↓
Stage 4
Sharding
      ↓
Stage 5
Sharding + Replication
```

However, this is **not a mandatory sequence**.

A real architecture should be based on actual application requirements.

---

# 27. Common Sharding Mistakes

## 1. Choosing a Shard Key Without Studying Queries

Do not start with:

> "Which field has the highest cardinality?"

Start with:

> "How does the application access the data?"

---

## 2. Using Low-Cardinality Fields

Examples:

```text
gender
status
boolean flags
```

These may provide poor distribution.

---

## 3. Ignoring Write Hotspots

Monotonically increasing keys can create concentrated write patterns with ranged sharding.

---

## 4. Assuming Sharding Fixes Indexing

Every shard still needs appropriate indexes for its workload.

---

## 5. Sharding Too Early

If one server comfortably handles the workload, unnecessary sharding adds complexity without solving a real problem.

---

## 6. Ignoring Scatter-Gather Queries

A technically valid shard key can still perform poorly if important application queries cannot target specific shards.

---

## 7. Assuming More Shards Always Means Better Performance

More shards also mean:

```text
More Nodes
    ↓
More Networking
    ↓
More Operations
    ↓
More Monitoring
    ↓
More Complexity
```

---

# 28. Advantages and Disadvantages

| Advantages               | Disadvantages               |
| ------------------------ | --------------------------- |
| Horizontal scalability   | Operational complexity      |
| Distributed storage      | Shard-key design complexity |
| Distributed workload     | Scatter-gather queries      |
| Additional capacity      | Cross-shard operations      |
| Can scale large datasets | More complex monitoring     |
| Can distribute writes    | Data movement/rebalancing   |

### 🧠 Key Takeaway

> Sharding provides scalability, but that scalability comes with architectural and operational complexity.

---

# 29. Interview Preparation

## Beginner Questions

### 1. What is sharding?

Sharding is the distribution of a collection's data across multiple database shards.

### 2. Why is sharding used?

To scale storage and workload horizontally when a single server is not sufficient.

### 3. What is a shard?

A database deployment responsible for a subset of the total data.

### 4. What is a shard key?

A field or set of fields used for data distribution and query routing.

### 5. Sharding vs replication?

Sharding distributes data. Replication maintains copies of data.

### 6. What is horizontal scaling?

Adding more machines/nodes instead of only increasing the resources of one machine.

### 7. What is vertical scaling?

Increasing CPU, RAM, storage, or other resources of a server.

### 8. What is partitioning?

Dividing a dataset into smaller logical parts.

### 9. What is `mongos`?

MongoDB's query router for sharded clusters.

### 10. What is a config server?

A deployment that stores sharded-cluster metadata.

---

## Intermediate Questions

### 11. What is a targeted query?

A query that MongoDB can route to the relevant shard(s) using shard-key information.

### 12. What is scatter-gather?

A query that must be sent to multiple/all relevant shards because it cannot be sufficiently targeted.

### 13. What is cardinality?

The number of distinct values in a field.

### 14. What is hotspotting?

A situation where disproportionate workload is concentrated on one shard.

### 15. Range vs hashed sharding?

Range sharding organizes data according to shard-key ranges. Hashed sharding hashes shard-key values to improve distribution.

### 16. What is a compound shard key?

A shard key consisting of multiple fields.

### 17. Why does field order matter in compound shard keys?

Because the shard-key prefix affects query targeting.

### 18. Why can low-cardinality keys be problematic?

They provide fewer distinct values for distributing data and may lead to poor distribution.

---

## Advanced Questions

### 19. Why can monotonically increasing keys be problematic?

With ranged distribution, new values can concentrate in the active range and create a hotspot.

### 20. What is the role of the balancer?

It manages data movement to maintain appropriate distribution across shards.

### 21. Can a shard key be changed?

MongoDB supports refining an existing shard key and resharding a collection with a different shard key, subject to version and operational requirements.

### 22. What is a zone?

A zone associates shard-key ranges with particular shards to support data-placement requirements.

### 23. How do you choose a shard key?

Analyze query patterns, cardinality, distribution, write behavior, hotspot risk, and range-query requirements.

### 24. How does `mongos` route a query?

It uses sharding metadata to determine which shard or shards contain the relevant data and routes the operation accordingly.

### 25. What happens when a query cannot be targeted?

`mongos` may broadcast the query to multiple/all relevant shards and merge the results.

### 26. How do you handle a hot shard?

Investigate the shard key and workload first. Possible solutions include refining or changing the shard key, resharding, or changing application access patterns.

### 27. Range vs hash trade-off?

Range distribution is useful for range-oriented access patterns, while hashing generally improves distribution but makes range queries less naturally targetable.

### 28. How do you design for failure?

Use appropriately designed replica sets for shards and config servers, multiple routers where appropriate, monitoring, backups, security, and tested recovery procedures.

### 29. When would you reshard?

When the existing shard key no longer provides suitable distribution or query performance, or when workload requirements change.

### 30. How do you test a shard key?

Use production-like data and queries, inspect distribution, analyze query plans, measure latency, and monitor workload distribution.

---

# 30. Hands-On Mini Project

# 🎓 Distributed Quiz Attempts Database

Let's apply the concepts to a learning platform.

## Technology

```text
Node.js
Express
MongoDB
MongoDB Sharded Cluster
```

---

## Data Model

```javascript
{
  attemptId: "att_10001",
  userId: 15000,
  quizId: 2001,
  score: 8,
  subject: "DBMS",
  createdAt: new Date()
}
```

---

## Step 1 — Create Database

```javascript
use quiz_app
```

---

## Step 2 — Create Collection

```javascript
db.createCollection("quiz_attempts")
```

---

## Step 3 — Create Supporting Index

For a compound shard key:

```javascript
db.quiz_attempts.createIndex({
  userId: 1,
  attemptId: 1
})
```

---

## Step 4 — Shard the Collection

```javascript
sh.shardCollection(
  "quiz_app.quiz_attempts",
  {
    userId: 1,
    attemptId: 1
  }
)
```

---

## Step 5 — Insert Sample Data

```javascript
for (let i = 1; i <= 100000; i++) {
  db.quiz_attempts.insertOne({
    attemptId: "att_" + i,
    userId: 1000 + (i % 50000),
    quizId: 2000 + (i % 100),
    score: Math.floor(Math.random() * 11),
    subject: "DBMS",
    createdAt: new Date()
  })
}
```

---

## Step 6 — Targeted Query

```javascript
db.quiz_attempts.find({
  userId: 15000
})
```

Because `userId` is the prefix of the shard key, this query provides useful routing information.

---

## Step 7 — Non-Targeted Query

```javascript
db.quiz_attempts.find({
  subject: "DBMS"
})
```

This query does not contain the shard-key prefix.

It may therefore require a broadcast/scatter-gather operation.

---

## Step 8 — Analyze Distribution

```javascript
db.quiz_attempts.getShardDistribution()
```

---

## Step 9 — Analyze Query Execution

```javascript
db.quiz_attempts
  .find({ userId: 15000 })
  .explain("executionStats")
```

Use the execution information to understand how MongoDB handled the operation.

### 🎯 Project Goal

The goal is **not simply to create a sharded collection**.

The goal is to understand:

```text
Query Pattern
      ↓
Shard Key
      ↓
Data Distribution
      ↓
Query Routing
      ↓
Performance
```

---

# 31. Learning Path & Cheat Sheet

## Recommended Learning Path

```text
Database Basics
      ↓
DBMS
      ↓
MongoDB Basics
      ↓
Indexes
      ↓
Replication
      ↓
Partitioning
      ↓
Horizontal Scaling
      ↓
Sharding
      ↓
Shard Keys
      ↓
MongoDB Sharding
      ↓
System Design
```

---

## Sharding Cheat Sheet

```text
Database
→ Organized collection of data.

DBMS
→ Software used to manage databases.

Scaling
→ Increasing system capacity.

Vertical Scaling
→ Make one server more powerful.

Horizontal Scaling
→ Add more servers/nodes.

Partitioning
→ Divide a dataset into smaller logical parts.

Replication
→ Maintain copies of data.

Sharding
→ Distribute a collection's data across multiple shards.

Shard
→ Database deployment responsible for part of the data.

Shard Key
→ Field(s) used for data distribution and routing.

Chunk
→ Range of shard-key values managed as a unit.

mongos
→ MongoDB query router.

Config Server
→ Stores sharded-cluster metadata.

Balancer
→ Manages data movement for distribution.

Range Sharding
→ Distribution based on shard-key ranges.

Hashed Sharding
→ Distribution based on hashed shard-key values.

Targeted Query
→ Query that can be routed to relevant shard(s).

Scatter-Gather
→ Query sent to multiple/all relevant shards.

Hotspot
→ Disproportionate workload concentrated on one shard.

Resharding
→ Reorganizing the distribution of a collection, including changing its shard key.
```

---

# 32. FAQ & Final Summary

### Is sharding the same as replication?

No.

```text
Sharding    → Distributes data
Replication → Copies data
```

---

### Is sharding always faster?

No.

Poor shard keys, scatter-gather queries, cross-shard operations, and network overhead can reduce performance.

---

### Does every application need sharding?

No.

Sharding should be introduced when the application's scale and requirements justify its complexity.

---

### Can MongoDB work without sharding?

Yes.

MongoDB can run as a standalone deployment or replica set.

---

### Can sharding and replication be used together?

Yes.

A common production architecture uses replica sets as shards.

---

### What happens if a shard becomes unavailable?

If the shard is a properly configured replica set, another member can provide availability for that shard's data.

However, data stored on an unavailable shard cannot simply be retrieved from another shard because sharding does not create copies of the data.

---

### Can I change the shard key?

MongoDB supports:

1. Refining an existing shard key by adding suffix fields.
2. Resharding a collection with a different shard key.

Both have operational requirements and should be planned carefully.

---

### Is sharding only used in MongoDB?

No.

Sharding/data partitioning is a general distributed-database concept. Different database systems implement it differently.

---

### Is sharding the same as partitioning?

Not exactly.

Partitioning is the broader concept of dividing data.

Sharding commonly refers to distributing those data partitions across multiple database servers.

---

### Does a good shard key guarantee good performance?

No.

Performance also depends on:

* Indexes
* Queries
* Hardware
* Data distribution
* Network
* Workload
* Aggregations
* Application design

---

# 🎯 What You Should Remember

If you remember only these points, remember these:

1. **Vertical scaling** makes one server stronger.
2. **Horizontal scaling** adds more servers.
3. **Partitioning** divides data.
4. **Replication** copies data.
5. **Sharding** distributes data across multiple shards.
6. The **shard key** is a critical design decision.
7. A good shard key should be evaluated using **query patterns and data distribution**.
8. **Range sharding** is useful for range-oriented workloads.
9. **Hashed sharding** generally improves distribution but can make range queries less naturally targetable.
10. `mongos` acts as the query router.
11. Config servers store sharded-cluster metadata.
12. Queries that cannot be targeted may become scatter-gather operations.
13. Sharding does not replace indexes.
14. Sharding and replication solve different problems and can be combined.
15. Sharding adds complexity, so use it when the problem justifies it.
16. MongoDB supports **refining** and **resharding** shard keys.
17. Always test your shard-key choice with realistic data and queries.

---

# 33. Resources

For MongoDB-specific concepts and commands, prefer the official MongoDB documentation.

* [MongoDB Sharding Documentation](https://www.mongodb.com/docs/manual/sharding/)
* [MongoDB Shard Keys](https://www.mongodb.com/docs/manual/core/sharding-shard-key/)
* [MongoDB Sharded Cluster Architecture](https://www.mongodb.com/docs/manual/core/sharded-cluster-architectures/)
* [MongoDB Config Servers](https://www.mongodb.com/docs/manual/core/sharded-cluster-config-servers/)
* [MongoDB Resharding](https://www.mongodb.com/docs/manual/core/sharding-reshard-a-collection/)
* [MongoDB Sharding Reference](https://www.mongodb.com/docs/v8.0/reference/sharding/)

> ⚠️ MongoDB evolves continuously. Always verify commands, deployment requirements, and behavior against the documentation for the MongoDB version you are using.

---

# 👨‍💻 Author

**Dheerendra Singh**

*B.Tech Software Engineering*

---

### ⭐ If this repository helped you understand database sharding, consider giving it a star.

**Learn → Build → Test → Understand**
