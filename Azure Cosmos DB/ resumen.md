# Developing a Solution for Azure Cosmos DB

## What is Azure Cosmos DB?

Azure Cosmos DB is a globally distributed, fully managed **NoSQL database service** provided by Microsoft Azure. It is designed for modern application development, offering **low latency**, **high availability**, and **automatic scalability**.

---

## Key Advantages

- **High availability** with strong SLAs
- **Fully managed PaaS** (Platform as a Service)
- **Serverless and provisioned throughput**, including autoscaling
- **Multiple consistency levels**
- **Flexible pricing**, including a free tier
- **Support for popular APIs**

These benefits make Cosmos DB an ideal choice for migrating from on-premises databases to the Azure platform.

---

## Cosmos DB APIs Overview

Each API provides the benefits of Cosmos DB’s scalability and global distribution. You can choose the API based on your existing development experience or application requirements.

---

### 1. Core (SQL) API

- **Data format:** JSON documents
- **Query language:** SQL-like syntax based on T-SQL
- **Use case:** Ideal for developers familiar with relational databases
- **Features:** Supports stored procedures, triggers, and UDFs in JavaScript
- **Recommended for:** New projects and migrations from PostgreSQL/Oracle

---

### 2. Table API

- **Data format:** Key-value (similar to Azure Table Storage)
- **Use case:** Simple object storage with minimal code changes
- **Advantages over Azure Table Storage:** Global distribution, auto-scaling, improved performance
- **Limitation:** Only supports **OLTP** workloads

---

### 3. MongoDB API

- **Data format:** BSON documents
- **Compatibility:** MongoDB tools (e.g., Compass, Shell, Robo 3T)
- **Supported versions:** MongoDB 3.2 to 5.0
- **Migration:** Easy switch by updating connection strings
- **Note:** Cosmos DB is Mongo-compatible but not built on native MongoDB code

---

### 4. Cassandra API

- **Data format:** Column-family (Cassandra-style)
- **Compatibility:** CQL and Cassandra tools (e.g., CQLSH)
- **Use case:** Applications needing horizontally scalable, column-oriented storage
- **Benefit:** No need to manage infrastructure or JVM
- **Limitation:** Only supports **OLTP** workloads

---

### 5. Gremlin API

- **Data format:** Graph (vertices and edges)
- **Query language:** Apache TinkerPop's Gremlin
- **Use case:** Applications with complex relationships (e.g., social graphs, recommendation engines)
- **Driver support:** Requires version 3.4.*
- **Limitation:** Only supports **OLTP** workloads

---

### 6. PostgreSQL API

- **Type:** Combines NoSQL scaling with relational database capabilities
- **Compatibility:** PostgreSQL (up to version 15), tools like pgAdmin
- **Features:** Supports DDL, CTEs, JSONB, extensions, and functions
- **Migration:** Simple lift-and-shift for PostgreSQL apps
- **Advantage:** Scalable without changing code

---

## Important Note

The **Core (SQL) API** is the native API of Cosmos DB and is the main focus for certification exams and most code samples.

---
