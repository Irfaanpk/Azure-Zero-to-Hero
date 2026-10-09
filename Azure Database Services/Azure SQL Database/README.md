# Azure SQL Database

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

# Lab: Create Azure SQL Database and Connect from an Ubuntu VM

## Overview

In this lab, you will create an Azure SQL Database, configure firewall rules, install SQL command-line tools on an Ubuntu virtual machine, and connect to the database over TCP port 1433.

## Objectives

- Create an Azure SQL logical server and SQL database.
- Configure SQL server firewall rules.
- Understand public endpoint connectivity.
- Install `sqlcmd` and Microsoft ODBC Driver on Ubuntu.
- Connect to Azure SQL Database from a Linux VM.
- Create a table, insert records, and execute SQL queries.

## Architecture

```
                Azure Cloud
                     |
             +---------------+
             | Ubuntu VM     |
             | SQL Client    |
             | sqlcmd        |
             +---------------+
                     |
                     | TCP 1433
                     | TLS Encryption
                     |
             +---------------+
             | SQL Firewall  |
             | Allow VM IP   |
             +---------------+
                     |
             +----------------------+
             | Azure SQL Logical    |
             | Server               |
             |                      |
             | Database: sqldemo    |
             +----------------------+
```

## Prerequisites

- An active Azure subscription.
- An Ubuntu VM with SSH access.
- Outbound internet connectivity from the VM.
- Permission to create Azure SQL resources.
- A SQL administrator username and password.
- Outbound TCP connectivity to port 1433.

## Step 1: Create an Azure SQL Database

1. Sign in to the [Azure Portal](https://portal.azure.com/).
2. Search for **SQL databases**.
3. Select **Create**.
4. Configure the following settings.

| Setting        | Example                                       |
| -------------- | --------------------------------------------- |
| Resource group | `rg-sql-lab`                                  |
| Database name  | `sqldemo`                                     |
| Server         | Create new                                    |
| Server name    | `sql-lab-unique-name`                         |
| Region         | A region available to your subscription       |
| Authentication | SQL authentication, if offered                |
| Compute        | An available development or serverless option |

5. Configure compute and storage according to your lab requirements.
6. Under **Networking**, select **Public endpoint**.
7. Keep broad Azure service access disabled.
8. Select **Review + create**.
9. Select **Create** and wait for deployment to finish.

**Note:** Azure SQL Database is a managed database service. The logical server is a management and connection endpoint, not a VM that you administer.

## Step 2: Obtain the SQL Server Details

1. Open the created SQL database.
2. On the Overview page, locate the server name.
3. Copy the fully qualified server name.

Example:

```
sql-lab-unique-name.database.windows.net
```

Record these values:

```
Server:   sql-lab-unique-name.database.windows.net
Database: sqldemo
Port:     1433
```

## Step 3: Configure SQL Server Firewall

1. Open the SQL database resource.
2. Select **Set server firewall**.
3. Add a firewall rule for the VM's actual outbound public IP address.
4. Enter the same IP as the start and end IP.
5. Save the firewall configuration.

To check the VM's current outbound public IP, run on Ubuntu:

```
curl -4 https://api.ipify.org
echo
```

Use the actual outbound IP used to reach Azure SQL. If the VM uses a NAT Gateway or shared outbound IP, configure the rule accordingly.

**Security:** Do not allow all IP addresses or enable broad Azure service access just to simplify the lab.

## Step 4: Connect to the Ubuntu VM

Connect to the VM using SSH:

```
ssh azureuser@<VM_PUBLIC_IP>
```

Replace `azureuser` and `<VM_PUBLIC_IP>` with your actual VM username and public IP.

Update the package list:

```
sudo apt update
```

Check the Ubuntu version:

```
cat /etc/os-release
```

## Step 5: Install SQL Command-Line Tools

Configure Microsoft's package repository:

```
curl -sSL -O https://packages.microsoft.com/config/ubuntu/$(grep VERSION_ID /etc/os-release | cut -d '"' -f 2)/packages-microsoft-prod.deb

sudo dpkg -i packages-microsoft-prod.deb

rm packages-microsoft-prod.deb
```

Install the ODBC driver and SQL tools:

```
sudo apt update

sudo ACCEPT_EULA=Y apt install -y \
  msodbcsql18 \
  mssql-tools18 \
  unixodbc-dev
```

Add the SQL tools directory to the PATH:

```
echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc

source ~/.bashrc
```

Verify the installation:

```
sqlcmd --version
```

If the package repository does not support your Ubuntu release, follow Microsoft's official installation guide:

[https://learn.microsoft.com/en-us/sql/linux/install-upgrade/setup-tools](https://learn.microsoft.com/en-us/sql/linux/install-upgrade/setup-tools)

## Step 6: Verify DNS and Network Connectivity

Check DNS resolution:

```
nslookup sql-lab-unique-name.database.windows.net
```

Install network diagnostic utilities if required:

```
sudo apt install -y netcat-openbsd dnsutils
```

Test TCP port 1433:

```
nc -zv sql-lab-unique-name.database.windows.net 1433
```

A successful result indicates that the TCP connection was established.

If it fails, check:

- SQL server firewall rules.
- VM outbound network rules.
- DNS resolution.
- Network firewalls restricting TCP 1433.
- Whether the database uses a public or private endpoint.

## Step 7: Connect to Azure SQL Database

Run:

```
sqlcmd \
  -S tcp:sql-lab-unique-name.database.windows.net,1433 \
  -d sqldemo \
  -U <SQL_ADMIN_USERNAME> \
  -N
```

Replace the server name, database name, and username with your actual values.

Enter the password when prompted, if supported by your `sqlcmd` version. Otherwise, use the documented password option for your installed version and avoid putting passwords in shell history.

The `-N` option enables encrypted transport. Keep certificate validation enabled; do not bypass certificate checks in production.

### Command Explanation

| Option   | Purpose                            |
| -------- | ---------------------------------- |
| `sqlcmd` | Starts the SQL command-line client |
| `-S`     | Specifies the SQL server and port  |
| `-d`     | Selects the database               |
| `-U`     | Specifies the SQL login            |
| `-N`     | Enables encrypted transport        |

## Step 8: Verify the Database Connection

After connecting, execute:

```
SELECT DB_NAME() AS CurrentDatabase;
GO
```

Expected output:

```
CurrentDatabase
---------------
sqldemo
```

This confirms which database your session is using.

## Step 9: Create a Table and Insert Data

Create a sample table:

```
CREATE TABLE dbo.Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(100),
    Course VARCHAR(100)
);
GO
```

Insert sample records:

```
INSERT INTO dbo.Students
    (StudentID, StudentName, Course)
VALUES
    (1, 'Irfan', 'Azure'),
    (2, 'Rahul', 'DevOps');
GO
```

Retrieve the records:

```
SELECT * FROM dbo.Students;
GO
```

Expected output:

| StudentID | StudentName | Course |
| --------- | ----------- | ------ |
| 1         | Irfan       | Azure  |
| 2         | Rahul       | DevOps |

Exit the SQL session:

```
QUIT
```

## Step 10: Troubleshooting

| Error                       | Possible cause                                   | Solution                                                    |
| --------------------------- | ------------------------------------------------ | ----------------------------------------------------------- |
| Login failed                | Incorrect username or password                   | Verify SQL credentials and authentication configuration     |
| Connection timed out        | Firewall or outbound networking issue            | Check the SQL firewall and outbound TCP 1433                |
| DNS resolution failed       | DNS configuration issue                          | Verify DNS and the server hostname                          |
| Cannot open database        | Wrong database name or missing permissions       | Verify the database name and SQL permissions                |
| `sqlcmd: command not found` | Tool not installed or PATH not configured        | Verify installation and PATH                                |
| TLS or certificate error    | Certificate validation or CA configuration issue | Check system CA certificates and use supported TLS settings |

## Cleanup

To avoid ongoing charges:

1. Open the Azure SQL Database resource.
2. Delete the database when it is no longer needed.
3. If the logical server and resource group are dedicated to this lab, remove them when no longer needed.
4. Verify the remaining resources before deleting a resource group.

**Important:** Serverless auto-pause does not necessarily eliminate all charges. Review your selected compute, storage, backup, and networking costs.

## Key Takeaways

- Azure SQL Database is a managed relational database service.
- The logical server provides the endpoint and manages server-level settings.
- SQL firewall rules control which client IP addresses can connect through a public endpoint.
- `sqlcmd` lets administrators and developers execute SQL statements from Linux.
- TCP port 1433 is used for the standard Azure SQL connection.
- TLS protects data in transit.
- A private endpoint can provide private network access when the required networking is configured.

## References

- [Azure SQL Database documentation](https://learn.microsoft.com/en-us/azure/azure-sql/database/)
- [Configure Azure SQL firewall rules](https://learn.microsoft.com/en-us/azure/azure-sql/database/firewall-configure)
- [Install SQL command-line tools on Linux](https://learn.microsoft.com/en-us/sql/linux/install-upgrade/setup-tools)
- [Azure SQL connectivity architecture](https://learn.microsoft.com/en-us/azure/azure-sql/database/connectivity-architecture)


