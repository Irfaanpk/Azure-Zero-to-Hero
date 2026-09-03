# 12.1 Azure SQL Database

## What is Azure SQL Database?

Azure SQL Database is a fully managed **Platform as a Service (PaaS)** relational database service based on Microsoft SQL Server.

It allows you to run SQL databases without managing the underlying virtual machines, operating system, or database infrastructure.

```text
Application
     │
     ▼
Azure SQL Database
     │
     ▼
Managed Azure Infrastructure
```

---

# Why Use Azure SQL Database?

Azure SQL Database provides:

- Fully managed SQL database
- Automatic patching and maintenance
- Built-in high availability
- Automatic backups
- Scalability
- Security and encryption
- Monitoring and diagnostics
- Integration with Azure applications

---

# Azure SQL Database Architecture

```text
                    Azure SQL
                       │
                Logical Server
                       │
          ┌────────────┼────────────┐
          │            │            │
       Database 1   Database 2   Database 3
```

A logical server provides a management boundary for Azure SQL databases.

---

# Azure SQL Database vs SQL Server on Azure VM

| Azure SQL Database | SQL Server on Azure VM |
|---|---|
| PaaS | IaaS |
| Microsoft manages infrastructure | Customer manages VM |
| OS management handled by Azure | Customer manages OS |
| Less administrative work | More control |
| Built-in managed features | More configuration required |

---

# Deployment Options

Azure SQL Database supports different deployment models.

### Single Database

A single database is deployed independently.

```text
Azure SQL
    │
    └── Database
```

### Elastic Pool

An **elastic pool** allows multiple databases to share a pool of compute resources.

```text
          Elastic Pool
               │
       ┌───────┼───────┐
       │       │       │
     DB-1    DB-2    DB-3
```

Elastic pools are useful when multiple databases have varying or unpredictable workloads.

---

# Logical Server

A logical server is used to manage Azure SQL databases.

It provides:

- Server-level configuration
- Firewall rules
- Authentication configuration
- Database management boundary

Example:

```text
Logical Server
      │
 ┌────┼────┐
 │    │    │
DB-1 DB-2 DB-3
```

---

# Compute and Service Tiers

Azure SQL Database provides different purchasing and performance options.

Common concepts include:

- DTU-based purchasing model
- vCore-based purchasing model
- Serverless
- Provisioned compute

### DTU

**DTU (Database Transaction Unit)** combines CPU, memory, and data I/O into a single performance measure.

### vCore

**vCore** provides more direct control over compute resources such as CPU and memory.

---

# Serverless

The **serverless** compute tier automatically adjusts compute resources based on workload.

```text
Low Workload
     ↓
Less Compute

High Workload
     ↓
More Compute
```

Serverless can be useful for databases with intermittent or unpredictable workloads.

---

# Scaling

Azure SQL Database supports scaling based on workload requirements.

```text
Current Resources
       ↓
Increase Compute
       ↓
Higher Capacity
```

Scaling can involve:

- Increasing compute
- Decreasing compute
- Changing service tiers
- Adjusting vCores or DTUs

---

# Connectivity

Applications can connect to Azure SQL Database using supported database connection methods.

```text
Application
     │
     │ SQL Connection
     ▼
Azure SQL Database
```

Common connection information includes:

```text
Server Name
Database Name
Username
Password
Port: 1433
```

---

# Firewall Rules

Azure SQL Database uses firewall rules to control network access.

```text
Client
   │
   ▼
SQL Firewall
   │
   ├── Allowed
   └── Denied
          │
          ▼
    Azure SQL Database
```

Firewall rules can allow specific public IP addresses or ranges.

---

# Authentication

Azure SQL Database supports different authentication methods.

Common options include:

- SQL authentication
- Microsoft Entra authentication

### SQL Authentication

Uses:

```text
Username + Password
```

### Microsoft Entra Authentication

Uses Microsoft Entra identities for authentication.

```text
User
 ↓
Microsoft Entra ID
 ↓
Azure SQL Database
```

---

# Backup

Azure SQL Database provides automated backups and supports point-in-time restore for supported configurations.

```text
Database
    ↓
Automatic Backup
    ↓
Restore
    ↓
Previous Database State
```

Backups help protect against accidental data deletion and database failures.

---

# Security

Azure SQL Database provides security features such as:

- Encryption at rest
- Encryption in transit
- Microsoft Entra authentication
- Firewall rules
- Network access controls
- Auditing and monitoring

---

# Practical Lab

## Lab: Create and Connect to Azure SQL Database

### Objective

Create an Azure SQL Database, configure network access, and connect to the database.

### Step 1: Create SQL Database

1. Open **Azure Portal**.
2. Search for **SQL databases**.
3. Select **Create**.
4. Select your subscription.
5. Create a resource group.

Example:

```text
Resource Group:
rg-sql-lab
```

6. Enter a unique database name.

```text
Database:
myazuresqldb
```

7. Create a new logical server.

---

### Step 2: Configure Server

Configure:

```text
Server Name
Location
Authentication Method
Administrator Login
Password
```

Save the configuration.

---

### Step 3: Configure Compute

Select an appropriate compute and service tier for the lab.

For a learning environment, choose a low-cost option.

---

### Step 4: Configure Networking

Open the networking configuration.

Configure the firewall to allow your current public IP address.

```text
Your Public IP
      ↓
SQL Firewall
      ↓
Allowed
```

---

### Step 5: Create the Database

Review the configuration and select:

```text
Create
```

Wait for deployment to complete.

---

### Step 6: Connect to the Database

Use a SQL-compatible client or the Azure Portal query editor.

Connect using:

```text
Server:
<server-name>.database.windows.net

Database:
myazuresqldb
```

Use the configured authentication method.

---

### Step 7: Create a Table

Run:

```sql
CREATE TABLE Employees (
    ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Department VARCHAR(100)
);
```

---

### Step 8: Insert Data

```sql
INSERT INTO Employees
VALUES
(1, 'John', 'IT'),
(2, 'Sarah', 'HR'),
(3, 'David', 'Finance');
```

---

### Step 9: Query the Data

```sql
SELECT * FROM Employees;
```

Verify that the inserted records are returned.

---

### Step 10: Test Firewall

Remove or modify the firewall rule and test the connection again.

```text
Allowed IP
    ↓
Connection Successful

Blocked IP
    ↓
Connection Denied
```

Restore the required firewall configuration after testing.

---

# Key Points

- Azure SQL Database is a managed PaaS relational database service.
- A logical server provides a management boundary for databases.
- A single database can run independently.
- Elastic pools allow multiple databases to share compute resources.
- DTU and vCore are Azure SQL purchasing/performance models.
- Serverless can automatically adjust compute based on workload.
- Azure SQL Database supports SQL and Microsoft Entra authentication.
- Firewall rules control network access.
- Azure SQL Database provides automated backups and point-in-time restore.
- Azure manages the underlying infrastructure, reducing administrative overhead.
