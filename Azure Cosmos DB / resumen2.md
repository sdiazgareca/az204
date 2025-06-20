# Provisioning Azure Cosmos DB

## Overview

You can provision a Cosmos DB account using:
- **Azure Portal**
- **Azure CLI**
- **Azure PowerShell**

When provisioning a new account, you must:
1. Provide a **unique DNS name**.
2. Select the **primary region**.
3. Optionally add **additional regions** for **global distribution** to achieve **99.999% SLA**.

---

## Capacity Modes

### Provisioned Throughput Plan
- Best for **constantly loaded databases**.
- Lets you allocate throughput **per container** — useful for boosting performance of heavily used containers.
- **Fixed monthly cost** based on:
  - Throughput (RU/s)
  - Storage
- Supports **Free Tier** for learning and exploration:
  - One free tier allowed per subscription.
  - Creates an instance with **minimum throughput**.

### Serverless Plan
- Optimized for:
  - **Occasionally loaded databases**.
  - **Reporting workloads**.
- **Pay-as-you-go** model:
  - Charges based on **actual usage/consumption**.

---

## API Selection

**Important:**  
When creating a Cosmos DB account, you must select an API.  
⚠️ **You cannot change the API after the account is provisioned.**

Available APIs:
- **SQL API** → Containers with documents.
- **MongoDB API** → Collections with documents.
- **Cassandra API** → Tables with rows/items.
- **Table API** → Tables with rows/items.
- **Gremlin API** → Graphs with nodes/edges.

---

## Post-Provisioning

After provisioning:
- You build the necessary structures:
  - Containers
  - Collections
  - Tables
  - Graphs
- Additional settings can be configured later:
  - **Backup**
  - **Networking**
  - **Encryption**

---

## Visual Structure (Figure 5.2)

```

Cosmos DB Account
├── Databases / Graphs / Keyspaces
│     ├── Containers / Collections / Tables / Graphs
│            ├── Items (Documents, Rows, Nodes)

```

---
