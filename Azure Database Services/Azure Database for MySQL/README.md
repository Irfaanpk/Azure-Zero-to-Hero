# 12.2 Azure Database for MySQL

## What is Azure Database for MySQL?

**Azure Database for MySQL** is a fully managed relational database service that allows you to run MySQL databases in Azure without managing the underlying virtual machines or operating system.

Azure Database for MySQL currently uses **Flexible Server** as the deployment option.

```text
Application
     │
     ▼
Azure Database for MySQL
     │
     ▼
Managed Azure Infrastructure
```

---

# Why Use Azure Database for MySQL?

Azure Database for MySQL provides:

- Fully managed MySQL database
- Automatic patching and maintenance
- Built-in high availability options
- Automated backups
- Scaling
- Encryption
- Network security
- Monitoring
- Integration with Azure applications

---

# MySQL Flexible Server

**Azure Database for MySQL Flexible Server** provides a managed MySQL database environment with more flexibility over compute, networking, and availability configuration.

```text
              MySQL Flexible Server
                       │
              ┌────────┼────────┐
              │        │        │
           Database  Database  Database
```

---

# Server and Database

A MySQL Flexible Server provides the database server environment where databases are created.

```text
MySQL Flexible Server
        │
   ┌────┼────┐
   │    │    │
 DB-1 DB-2 DB-3
```

---

# Compute and Storage

When creating a MySQL Flexible Server, you configure resources such as:

- Compute size
- vCores
- Memory
- Storage capacity
- Storage performance

The resources can be adjusted based on workload requirements.

---

# Scaling

MySQL Flexible Server supports scaling to accommodate changing workloads.

```text
Low Workload
     ↓
Smaller Compute

High Workload
     ↓
Larger Compute
```

You can scale resources such as:

- Compute
- Storage

---

# High Availability

MySQL Flexible Server supports high availability configurations for supported tiers and regions.

```text
             MySQL Flexible Server
                      │
             ┌────────┴────────┐
             │                 │
          Primary           Standby
```

High availability helps reduce downtime caused by infrastructure failures.

---

# Networking

MySQL Flexible Server supports network connectivity through:

- Public access
- Private access using Azure Virtual Network integration

### Public Access

```text
Internet
    │
    ▼
Firewall
    │
    ▼
MySQL Flexible Server
```

### Private Access

```text
Azure VNet
    │
    ▼
MySQL Flexible Server
```

Private access keeps database connectivity within Azure networking.

---

# Firewall Rules

When public access is enabled, firewall rules control which IP addresses can connect to the MySQL server.

```text
Client IP
    │
    ▼
MySQL Firewall
    │
 ┌──┴──┐
Allow  Deny
```

You can configure firewall rules for:

- Individual client IP addresses
- IP address ranges

---

# Authentication

MySQL Flexible Server supports MySQL authentication using database credentials.

Example:

```text
Username
Password
     ↓
MySQL Flexible Server
```

Azure integrations can also provide identity-based access options where supported.

---

# Backup

MySQL Flexible Server provides automated backups.

```text
MySQL Database
      ↓
Automated Backup
      ↓
Restore
```

Backups can be used for database recovery and point-in-time restore within the supported retention period.

---

# Encryption

Azure Database for MySQL provides encryption for data at rest.

```text
MySQL Database
      ↓
Encrypted Storage
```

Connections can also use TLS to protect data in transit.

```text
Application
     │
    TLS
     ↓
MySQL Flexible Server
```

---

# Monitoring

Azure Monitor can be used to monitor MySQL Flexible Server.

Examples of monitored information include:

- CPU usage
- Storage
- Connections
- Memory
- Network activity

```text
MySQL Flexible Server
        ↓
   Azure Monitor
        ↓
     Metrics
```

---

# Azure Database for MySQL vs MySQL on Azure VM

| MySQL Flexible Server | MySQL on Azure VM |
|---|---|
| PaaS | IaaS |
| Azure manages infrastructure | Customer manages VM |
| Managed patching | Customer manages OS and software |
| Built-in backup features | Customer manages backup |
| Less administration | More administration |
| Limited OS-level control | Full OS control |

---

# Common Use Cases

Azure Database for MySQL is commonly used for:

- Web applications
- CMS applications
- E-commerce applications
- Application backends
- Development environments
- Production applications

---

# Practical Lab

## Lab: Create and Connect to MySQL Flexible Server

### Objective

Create an Azure Database for MySQL Flexible Server, configure network access, connect to the database, and perform basic SQL operations.

### Step 1: Create Resource Group

```bash
az group create \
  --name rg-mysql-lab \
  --location eastus
```

---

### Step 2: Create MySQL Flexible Server

```bash
az mysql flexible-server create \
  --resource-group rg-mysql-lab \
  --name <unique-server-name> \
  --location eastus \
  --admin-user mysqladmin \
  --admin-password '<StrongPassword>' \
  --sku-name Standard_B1ms \
  --storage-size 32
```

> Use a strong password and replace the placeholder values with your own values.

---

### Step 3: Configure Firewall

Allow your current public IP address:

```bash
az mysql flexible-server firewall-rule create \
  --resource-group rg-mysql-lab \
  --name <unique-server-name> \
  --rule-name AllowMyIP \
  --start-ip-address <YOUR_PUBLIC_IP> \
  --end-ip-address <YOUR_PUBLIC_IP>
```

---

### Step 4: Connect to MySQL

Using the MySQL client:

```bash
mysql -h <unique-server-name>.mysql.database.azure.com \
  -u mysqladmin \
  -p
```

Enter the administrator password when prompted.

---

### Step 5: Create a Database

```sql
CREATE DATABASE companydb;
```

Select the database:

```sql
USE companydb;
```

---

### Step 6: Create a Table

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100)
);
```

---

### Step 7: Insert Data

```sql
INSERT INTO employees
VALUES
(1, 'John', 'IT'),
(2, 'Sarah', 'HR'),
(3, 'David', 'Finance');
```

---

### Step 8: Query the Data

```sql
SELECT * FROM employees;
```

Verify that the records are returned.

---

### Step 9: Test Firewall

Remove or modify the firewall rule and test the connection.

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

### Step 10: Clean Up

```bash
az group delete \
  --name rg-mysql-lab
```

---

# Key Points

- Azure Database for MySQL is a managed PaaS database service.
- MySQL Flexible Server is the current Azure Database for MySQL deployment option.
- Azure manages the underlying infrastructure and database maintenance.
- Flexible Server supports configurable compute and storage.
- High availability is available for supported configurations.
- Public access uses firewall rules to control connectivity.
- Private access can be configured using Azure networking.
- Automated backups support database recovery.
- Data at rest is encrypted and connections can use TLS.
- Azure Monitor provides monitoring capabilities.
- MySQL Flexible Server requires less infrastructure management than running MySQL on an Azure VM.
