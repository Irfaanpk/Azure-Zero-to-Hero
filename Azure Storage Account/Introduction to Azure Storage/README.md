# 6.1 Introduction to Azure Storage

Azure Storage is Microsoft's cloud storage platform that provides highly available, durable, and scalable storage services for applications and Azure resources.

Azure Storage can store different types of data such as:

- Files
- Images
- Videos
- Documents
- Application data
- Virtual machine disks
- Structured and unstructured data

---

## Why Use Azure Storage?

Azure Storage provides:

- High durability
- High availability
- Scalability
- Security
- Data redundancy
- Encryption
- Multiple access methods
- Pay-as-you-go pricing

Example:

```text
Application
     │
     ▼
Azure Storage
     │
     ├── Blob Storage
     ├── Azure Files
     ├── Queue Storage
     └── Table Storage
```

> **Note:** Queue Storage and Table Storage exist in Azure Storage, but they are not covered separately in this AZ-104 learning path.

---

# Azure Storage Services

Azure Storage provides several storage services.

## Blob Storage

**Azure Blob Storage** is object storage designed for storing large amounts of unstructured data.

Examples:

```text
Images
Videos
Documents
Backups
Logs
Application files
```

Example:

```text
Storage Account
      │
      └── Container
             │
             ├── image.jpg
             ├── video.mp4
             └── backup.zip
```

Blob Storage is covered in detail in **6.3 Azure Blob Storage**.

---

## Azure Files

**Azure Files** provides fully managed file shares that can be accessed using protocols such as **SMB** and **NFS**.

Example:

```text
Azure File Share
      │
      ├── file1.txt
      ├── report.pdf
      └── backup.zip
```

Azure Files is covered in detail later in this section.

---

## Managed Disks

Azure Managed Disks provide block-level storage for Azure Virtual Machines.

Example:

```text
Virtual Machine
      │
      ├── OS Disk
      └── Data Disk
```

Managed Disks will be covered under the **Azure Virtual Machines** section.

---

# Azure Storage Account

A **Storage Account** provides a namespace and management boundary for Azure Storage services.

Example:

```text
Storage Account
      │
      ├── Blob Storage
      ├── Azure Files
      ├── Queue Storage
      └── Table Storage
```

A storage account provides settings related to:

- Performance
- Redundancy
- Security
- Networking
- Encryption
- Data protection

Storage Accounts are covered in detail in **6.2 Storage Accounts**.

---

# Azure Storage Namespace

A storage account provides a unique namespace for accessing storage data.

Example:

```text
Storage Account
       │
       ▼
https://mystorageaccount.blob.core.windows.net
```

The endpoint depends on the storage service being used.

Example Blob endpoint:

```text
https://<storage-account>.blob.core.windows.net
```

Example File endpoint:

```text
https://<storage-account>.file.core.windows.net
```

---

# Azure Storage Data Types

Azure Storage can be used to store different types of data.

### Structured Data

Data organized according to a defined structure.

Example:

```text
Tables
Records
Entities
```

### Unstructured Data

Data that does not follow a fixed structure.

Examples:

```text
Images
Videos
Documents
Logs
Backups
```

Blob Storage is commonly used for unstructured data.

---

# Azure Storage Access

Azure Storage supports different methods of accessing data.

Common methods include:

- Microsoft Entra ID
- Azure RBAC
- Shared Access Signatures (SAS)
- Storage Account Access Keys

Example:

```text
User / Application
        │
        ▼
Authentication
        │
        ▼
Authorization
        │
        ▼
Azure Storage
```

These access methods are covered in detail in later topics.

---

# Azure Storage Security

Azure Storage provides several security features.

Common security capabilities include:

- Encryption at rest
- Microsoft Entra authentication
- Azure RBAC
- SAS
- Storage account access keys
- Firewall and network restrictions
- Private endpoints
- Data protection features

Example:

```text
Azure Storage
     │
     ├── Authentication
     ├── Authorization
     ├── Encryption
     ├── Network Security
     └── Data Protection
```

---

# Azure Storage Redundancy

Azure Storage can maintain multiple copies of data to protect against failures.

Common redundancy options include:

```text
LRS
ZRS
GRS
GZRS
RA-GRS
RA-GZRS
```

The redundancy configuration is managed at the storage account level.

These options are covered in **6.2 Storage Accounts**.

---

# Azure Storage Use Cases

Azure Storage can be used for many scenarios.

### Application Data

```text
Application
     │
     ▼
Azure Blob Storage
```

### Backup Storage

```text
Backup
  │
  ▼
Azure Storage
```

### File Sharing

```text
Users / Applications
        │
        ▼
Azure Files
```

### Media Storage

```text
Images / Videos
       │
       ▼
Blob Storage
```

---

## 🧪 Lab: Explore Azure Storage

### Objective

Explore the main Azure Storage services through the Azure Portal.

### Prerequisites

- Azure account
- Azure subscription
- Access to the Azure Portal

---

### Step 1: Open Storage Accounts

1. Sign in to the **Azure Portal**.
2. Search for **Storage accounts**.
3. Open **Storage accounts**.

---

### Step 2: Explore Storage Accounts

If you already have a storage account:

1. Select the storage account.
2. Review the **Overview** page.
3. Explore the available configuration options.

Look for sections such as:

```text
Data storage
Security + networking
Data management
Settings
Monitoring
```

---

### Step 3: Explore Data Storage

Inside the storage account, review:

```text
Containers
File shares
Queues
Tables
```

For this AZ-104 learning path, the main focus will be:

```text
Containers → Blob Storage
File shares → Azure Files
```

---

### Step 4: Review Storage Account Settings

Explore the following areas:

```text
Configuration
Networking
Data protection
Encryption
Access control (IAM)
```

Do not change any settings during this introductory lab.

---

## Key Points

- Azure Storage is Microsoft's cloud storage platform.
- A **Storage Account** provides a management boundary for Azure Storage services.
- **Blob Storage** is used for object storage and unstructured data.
- **Azure Files** provides managed file shares.
- **Managed Disks** provide block storage for Azure Virtual Machines.
- Storage accounts provide configuration for performance, redundancy, security, networking, and encryption.
- Azure Storage supports multiple authentication and authorization methods.
- Azure Storage provides different redundancy options for data protection.
- Storage Account configuration and redundancy are covered in **6.2**.
- Blob Storage is covered in detail in **6.3**.
