# 6.2 Storage Accounts

An **Azure Storage Account** provides a unique namespace in Azure for storing and accessing data using Azure Storage services.

A storage account can provide access to services such as:

- Azure Blob Storage
- Azure Files
- Azure Queue Storage
- Azure Table Storage

For AZ-104, the main focus is on **Blob Storage and Azure Files**.

---

## What is a Storage Account?

A storage account is the top-level container for Azure Storage data.

It provides:

- A unique storage namespace
- Data storage and access endpoints
- Performance options
- Redundancy options
- Security and network controls
- Access through Microsoft Entra ID, RBAC, SAS, and access keys

A storage account name must be **globally unique** because it is used in the storage endpoint.

Example:

```text
mystorageaccount123
```

Blob endpoint:

```text
https://mystorageaccount123.blob.core.windows.net
```

---

## Storage Account Types

Azure provides different storage account types based on the type of data and performance requirements.

### 1. General-Purpose v2

**General-purpose v2 (GPv2)** is the recommended and most commonly used storage account type.

It supports:

- Blob Storage
- Azure Files
- Queue Storage
- Table Storage

It supports both **Standard** and certain storage features such as access tiers for Blob Storage.

For most Azure Storage workloads, **GPv2** should be the default choice.

---

### 2. Premium Block Blob

**BlockBlobStorage** provides premium performance for block blob workloads.

Use it when applications require:

- High transaction rates
- Low latency
- High-performance blob storage

---

### 3. Premium File Shares

**FileStorage** provides premium performance for Azure Files.

Use it when applications require:

- High-performance file shares
- Low latency
- High IOPS
- Consistent performance

---

## Storage Account Performance

Azure Storage accounts primarily use two performance options:

| Performance | Description |
|---|---|
| **Standard** | Cost-effective storage for most workloads |
| **Premium** | Higher performance and lower latency |

### Standard

Standard storage uses **HDD-based storage** and is suitable for many general-purpose workloads.

Common use cases:

- General Blob Storage
- Backup data
- Logs
- Documents
- General file shares

---

### Premium

Premium storage uses **SSD-based storage** and provides higher performance and lower latency.

Common use cases:

- High-performance applications
- Low-latency workloads
- High transaction workloads
- Premium Azure Files
- Premium block blobs

---

## Storage Redundancy

Azure Storage automatically maintains copies of your data based on the selected **redundancy option**.

Redundancy protects data against:

- Hardware failures
- Datacenter failures
- Availability Zone failures
- Regional failures

The main redundancy options are:

- **LRS**
- **ZRS**
- **GRS**
- **GZRS**
- **RA-GRS**
- **RA-GZRS**

---

## 1. Locally Redundant Storage (LRS)

**LRS** keeps multiple copies of data within a single physical location in the primary region.

```text
Primary Region
└── Storage Location
    ├── Copy 1
    ├── Copy 2
    └── Copy 3
```

### Advantages

- Lowest-cost redundancy option
- Protects against hardware failures
- Suitable when regional protection is not required

### Limitation

LRS does not protect against a complete regional or datacenter-level failure.

---

## 2. Zone-Redundant Storage (ZRS)

**ZRS** synchronously replicates data across multiple **Availability Zones** in the primary Azure region.

```text
Primary Region

Zone 1 ── Copy
Zone 2 ── Copy
Zone 3 ── Copy
```

### Advantages

- Protects against Availability Zone failures
- Data remains within the primary region
- Higher availability than LRS

---

## 3. Geo-Redundant Storage (GRS)

**GRS** replicates data to a secondary Azure region.

Data is stored in:

- Primary region
- Secondary region

Replication to the secondary region is **asynchronous**.

```text
Primary Region
      │
      │ Asynchronous Replication
      ▼
Secondary Region
```

### Advantages

- Provides protection against regional failures
- Data is replicated to another Azure region

---

## 4. Geo-Zone-Redundant Storage (GZRS)

**GZRS** combines:

- Zone redundancy in the primary region
- Geo-replication to a secondary region

```text
Primary Region
├── Zone 1
├── Zone 2
└── Zone 3
       │
       │ Asynchronous Replication
       ▼
Secondary Region
```

This provides protection against both:

- Availability Zone failures
- Regional failures

---

## 5. Read-Access Geo-Redundant Storage (RA-GRS)

**RA-GRS** provides the same geo-replication as GRS but also allows **read access to the secondary region**.

```text
Primary Region
      │
      │ Replication
      ▼
Secondary Region
      │
      └── Read Access
```

This can be useful when applications need to read data from the secondary region.

---

## 6. Read-Access Geo-Zone-Redundant Storage (RA-GZRS)

**RA-GZRS** combines:

- Zone redundancy in the primary region
- Geo-replication to a secondary region
- Read access to the secondary region

It provides a high level of availability and geographic protection.

---

## Storage Redundancy Comparison

| Redundancy | Primary Region | Availability Zones | Secondary Region | Read Secondary |
|---|---|---|---|---|
| **LRS** | ✅ | ❌ | ❌ | ❌ |
| **ZRS** | ✅ | ✅ | ❌ | ❌ |
| **GRS** | ✅ | ❌ | ✅ | ❌ |
| **GZRS** | ✅ | ✅ | ✅ | ❌ |
| **RA-GRS** | ✅ | ❌ | ✅ | ✅ |
| **RA-GZRS** | ✅ | ✅ | ✅ | ✅ |

### Simple way to remember

```text
LRS      → Local copies
ZRS      → Zone copies
GRS      → Geographic copies
GZRS     → Zone + Geographic copies
RA-GRS   → GRS + Read access
RA-GZRS  → GZRS + Read access
```

---

## Primary and Secondary Region

When using geo-redundant storage such as GRS or GZRS:

- **Primary region** contains the primary copy of the data.
- **Secondary region** contains the replicated copy.
- Data is replicated from the primary region to the secondary region asynchronously.

The secondary region is typically the Azure paired region associated with the primary region.

---

## Choosing Storage Redundancy

Choose redundancy based on the level of protection required.

| Requirement | Recommended Option |
|---|---|
| Lowest-cost redundancy | LRS |
| Protection from Availability Zone failure | ZRS |
| Protection from regional failure | GRS |
| Zone + regional protection | GZRS |
| Read access to secondary region | RA-GRS / RA-GZRS |
| Highest availability requirements | GZRS / RA-GZRS |

---

## Storage Account Naming

Storage account names must follow Azure naming requirements.

A storage account name:

- Must be globally unique
- Must contain only lowercase letters and numbers
- Must be between 3 and 24 characters
- Cannot contain spaces or special characters

Example:

```text
azstorage2026demo
```

---

## Storage Account Endpoints

Azure provides service endpoints based on the storage services being used.

### Blob Storage

```text
https://<storage-account-name>.blob.core.windows.net
```

### Azure Files

```text
https://<storage-account-name>.file.core.windows.net
```

Example:

```text
https://azstorage2026demo.blob.core.windows.net
```

---

# 🧪 Lab: Create an Azure Storage Account

### Step 1: Open Azure Portal

Go to:

```text
https://portal.azure.com
```

Search for:

```text
Storage accounts
```

Select **Storage accounts**.

---

### Step 2: Create Storage Account

Click:

```text
+ Create
```

---

### Step 3: Basics

Configure:

**Subscription**

Select your Azure subscription.

**Resource group**

Create or select a resource group.

Example:

```text
storage-rg
```

**Storage account name**

Example:

```text
azstorage2026demo123
```

The name must be globally unique.

**Region**

Select a region close to your users or workload.

Example:

```text
Central India
```

**Performance**

Select:

```text
Standard
```

**Redundancy**

Select:

```text
Locally-redundant storage (LRS)
```

For a learning environment, LRS is usually sufficient.

---

### Step 4: Review

Click:

```text
Review
```

Azure validates the configuration.

---

### Step 5: Create

Click:

```text
Create
```

Wait for the deployment to complete.

---

### Step 6: Open the Storage Account

Click:

```text
Go to resource
```

You can now view the storage account and its available storage services.

---

## Verify Using Azure CLI

After creating the storage account, you can verify it using:

```bash
az storage account list --output table
```

Show details of a specific storage account:

```bash
az storage account show \
  --name <storage-account-name> \
  --resource-group <resource-group-name> \
  --output table
```

---

## Key Points

- A **Storage Account** is the top-level container for Azure Storage data.
- **General-purpose v2 (GPv2)** is the recommended general-purpose storage account type.
- **Standard** provides cost-effective storage for most workloads.
- **Premium** provides higher performance and lower latency.
- **LRS** provides local redundancy.
- **ZRS** provides zone redundancy.
- **GRS** provides geo-redundancy.
- **GZRS** combines zone and geo-redundancy.
- **RA-GRS** and **RA-GZRS** provide read access to the secondary region.
- Storage account names must be globally unique.
- Storage account redundancy should be selected based on availability and disaster-recovery requirements.
