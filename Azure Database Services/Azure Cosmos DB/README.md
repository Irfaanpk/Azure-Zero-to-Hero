# 12.3 Azure Cosmos DB

## What is Azure Cosmos DB?

**Azure Cosmos DB** is a fully managed NoSQL database service designed for highly scalable and globally distributed applications.

It provides low-latency access to data and supports automatic scaling and global distribution.

```text
Application
     │
     ▼
 Azure Cosmos DB
     │
 ┌───┼───────────┐
 │   │           │
Data Data      Data
```

---

# Why Use Azure Cosmos DB?

Azure Cosmos DB provides:

- Fully managed NoSQL database
- Global distribution
- Low-latency access
- Automatic scaling
- High availability
- Multiple consistency levels
- Flexible schema
- Multiple APIs

---

# NoSQL Database

Unlike traditional relational databases, NoSQL databases do not require a fixed table-based schema.

Example:

```json
{
    "id": "101",
    "name": "John",
    "department": "IT"
}
```

Different documents can contain different fields.

---

# Cosmos DB Architecture

The basic structure is:

```text
Cosmos DB Account
       │
       ▼
   Database
       │
       ▼
   Container
       │
       ▼
     Items
```

---

# Cosmos DB Account

The **Cosmos DB account** is the top-level Azure resource.

It defines important configuration such as:

- API
- Region
- Consistency
- Networking
- Global distribution

---

# Database

A database provides a logical grouping of containers.

```text
Cosmos DB Account
       │
       ├── Database
       │      ├── Container
       │      └── Container
```

---

# Container

A **container** stores the actual data items.

In a document-based API, a container can be compared conceptually to a table in a relational database.

```text
Container
   │
   ├── Item 1
   ├── Item 2
   └── Item 3
```

---

# Items

**Items** are individual data records stored inside a container.

Example:

```json
{
    "id": "1",
    "name": "Alice",
    "role": "Developer"
}
```

---

# Partition Key

A **partition key** determines how data is distributed across physical partitions.

Example:

```text
Container
   │
   ├── Partition: IT
   │      ├── Item 1
   │      └── Item 2
   │
   └── Partition: HR
          ├── Item 3
          └── Item 4
```

Choosing an appropriate partition key is important for:

- Scalability
- Performance
- Even data distribution

Example:

```text
Partition Key:
/department
```

---

# Request Units (RU/s)

Cosmos DB uses **Request Units (RU)** to measure the resources required to perform database operations.

Different operations consume different amounts of RU.

Example:

```text
Simple Read
   ↓
Lower RU

Complex Query
   ↓
Higher RU
```

RU/s represents the provisioned request processing capacity.

---

# Provisioned Throughput

With provisioned throughput, you specify the amount of RU/s available for the workload.

```text
Provisioned RU/s
       ↓
Cosmos DB
       ↓
Application Requests
```

---

# Serverless

Cosmos DB also supports a serverless capacity mode for workloads with intermittent or unpredictable traffic.

```text
No Requests
     ↓
No Continuous Provisioned Capacity

Requests
     ↓
Consume RUs
```

Serverless is useful for workloads that do not require continuously provisioned throughput.

---

# Global Distribution

Cosmos DB can replicate data across multiple Azure regions.

```text
                 Cosmos DB
                     │
          ┌──────────┼──────────┐
          │          │          │
       Region 1   Region 2   Region 3
          │          │          │
        Data       Data       Data
```

Global distribution can provide:

- Lower latency for users in different regions
- High availability
- Regional redundancy

---

# Consistency Levels

Cosmos DB provides multiple consistency levels that allow you to balance consistency, availability, and latency.

Common consistency levels include:

- Strong
- Bounded Staleness
- Session
- Consistent Prefix
- Eventual

```text
Strong
  ↓
More consistency

Eventual
  ↓
More availability / lower latency
```

The appropriate level depends on application requirements.

---

# Cosmos DB APIs

Azure Cosmos DB supports multiple APIs.

Examples include:

- NoSQL
- MongoDB
- Cassandra
- Gremlin
- Table

The API is selected when creating the Cosmos DB account.

---

# Networking

Cosmos DB provides networking controls such as:

- Public network access
- Firewall and IP rules
- Virtual network integration
- Private endpoints

Example:

```text
Application
     │
     ▼
Network Controls
     │
     ▼
Cosmos DB
```

---

# Security

Cosmos DB supports security features including:

- Microsoft Entra authentication
- Role-based access control
- Encryption at rest
- Network access controls
- Private endpoints

---

# Cosmos DB vs Azure SQL Database

| Cosmos DB | Azure SQL Database |
|---|---|
| NoSQL | Relational |
| Flexible schema | Structured schema |
| Documents/items | Tables/rows |
| Partition-based scaling | Relational scaling |
| Global distribution | Supports regional options |
| Multiple APIs | T-SQL |

---

# Common Use Cases

Cosmos DB is commonly used for:

- Globally distributed applications
- Web and mobile applications
- IoT applications
- Real-time applications
- Catalogs
- User profiles
- Applications requiring low-latency access

---

# Practical Lab

## Lab: Create and Manage Azure Cosmos DB

### Objective

Create a Cosmos DB account, database, container, configure a partition key, and store/query items.

### Step 1: Create Cosmos DB Account

1. Open **Azure Portal**.
2. Search for **Azure Cosmos DB**.
3. Select **Create**.
4. Select the **Azure Cosmos DB for NoSQL** API.
5. Select your subscription.
6. Create a resource group.

Example:

```text
Resource Group:
rg-cosmos-lab
```

7. Enter a unique account name.
8. Select the required region.
9. Review the configuration.
10. Select **Create**.

---

### Step 2: Create a Database

Open the Cosmos DB account.

Go to:

```text
Data Explorer
    ↓
New Database
```

Create:

```text
Database:
companydb
```

---

### Step 3: Create a Container

Create a container:

```text
Container:
employees
```

Configure the partition key:

```text
/department
```

---

### Step 4: Create an Item

Create a new item:

```json
{
    "id": "1",
    "name": "John",
    "department": "IT"
}
```

Create another item:

```json
{
    "id": "2",
    "name": "Sarah",
    "department": "HR"
}
```

---

### Step 5: Query Items

Use Data Explorer to query the container.

Example:

```sql
SELECT * FROM c
```

Verify that the created items are returned.

---

### Step 6: Test Partitioning

Create additional items using different department values.

Example:

```json
{
    "id": "3",
    "name": "David",
    "department": "Finance"
}
```

Observe how the partition key is used to distribute the data.

---

### Step 7: Explore Request Units

Run queries and observe the **Request Unit (RU)** consumption.

Compare a simple query with a more complex query.

---

### Step 8: Clean Up

Delete the resource group:

```text
rg-cosmos-lab
```

This removes the Cosmos DB resources created for the lab.

---

# Key Points

- Azure Cosmos DB is a fully managed NoSQL database service.
- A Cosmos DB account contains databases.
- Databases contain containers.
- Containers store items.
- Partition keys determine how data is distributed.
- RU/s measures database request capacity.
- Cosmos DB supports provisioned and serverless capacity modes.
- Cosmos DB supports global distribution across Azure regions.
- Multiple consistency levels are available.
- Cosmos DB supports multiple APIs.
- Networking and access can be controlled using firewall rules, RBAC, and private endpoints.
