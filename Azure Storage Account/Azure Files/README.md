# 6.14 Azure Files

**Azure Files** is a fully managed file-sharing service in Azure.

It provides cloud-based file shares that can be accessed by:

- Azure Virtual Machines
- On-premises servers
- Windows clients
- Linux clients
- Applications

Azure Files supports standard file-sharing protocols such as:

- **SMB**
- **NFS**

---

## Why Use Azure Files?

Azure Files is useful when applications require a shared file system instead of object storage.

Common use cases include:

- Shared application files
- Configuration files
- File shares for multiple virtual machines
- Migrating on-premises file shares to Azure
- Centralized file storage
- Application data that requires a traditional file-system structure

---

## Azure Files Structure

Azure Files uses the following structure:

```text
Storage Account
└── File Share
    ├── Folder
    │   ├── file1.txt
    │   └── file2.txt
    │
    └── file3.txt
```

### Storage Account

The storage account provides the underlying Azure Storage infrastructure.

### File Share

A **file share** is the main storage area where files and directories are stored.

---

## Azure Files vs Blob Storage

Azure Files and Blob Storage are designed for different storage requirements.

| Feature | Azure Files | Blob Storage |
|---|---|---|
| Storage model | File system | Object storage |
| Main structure | Files and directories | Containers and blobs |
| SMB support | ✅ | ❌ |
| NFS support | ✅ | ❌ |
| File share | ✅ | ❌ |
| Object storage | ❌ | ✅ |
| Typical use | Shared file storage | Unstructured object data |

---

## Azure Files Storage Tiers

Azure Files provides different storage options based on performance and cost requirements.

Common tiers include:

- **Premium**
- **Transaction optimized**
- **Hot**
- **Cool**

### Premium

Premium Azure Files provides high-performance file shares using SSD-based storage.

Suitable for:

- High-performance workloads
- Low-latency applications
- High IOPS requirements

---

### Transaction Optimized

Designed for workloads with high transaction requirements but lower storage requirements.

Suitable for:

- File-based applications
- Workloads with frequent file operations

---

### Hot

Designed for frequently accessed file data.

Suitable for:

- Frequently accessed files
- General-purpose file workloads

---

### Cool

Designed for infrequently accessed file data.

Suitable for:

- Older files
- Infrequently accessed data
- Cost-sensitive file storage

---

## Azure Files Performance Comparison

| Tier | Typical Use | Cost Model |
|---|---|---|
| **Premium** | High-performance workloads | Higher storage cost |
| **Transaction optimized** | High transaction workloads | Optimized for transactions |
| **Hot** | Frequently accessed files | Higher access frequency |
| **Cool** | Infrequently accessed files | Lower storage cost |

---

## File Share Quotas

A file share has a configured capacity or quota that determines how much data it can store.

The available limits depend on:

- Storage account type
- File share type
- Protocol
- Azure Files configuration

---

## Azure Files Authentication and Access

Azure Files access can be controlled using different authentication and authorization mechanisms.

Depending on the configuration, Azure Files can use:

- Microsoft Entra-based authentication
- Storage account access keys
- SAS
- Active Directory-based authentication for SMB scenarios

Access permissions determine what users and applications can do with the files.

---

## Azure Files Networking

Azure Files can be accessed through the storage account's network endpoints.

Network access can be controlled using:

- Storage account firewall
- Virtual network rules
- Private endpoints

Example:

```text
Azure VM
   ↓
Virtual Network
   ↓
Private Endpoint
   ↓
Azure Files
```

This allows file shares to be accessed privately from an Azure virtual network.

---

# 🧪 Lab: Create an Azure File Share

### Step 1: Open Storage Account

Go to:

```text
https://portal.azure.com
```

Open:

```text
Storage accounts
```

Select your storage account.

---

### Step 2: Open File Shares

Go to:

```text
Data storage → File shares
```

Click:

```text
+ File share
```

---

### Step 3: Create File Share

Enter:

```text
Name:
azfileshare
```

Select an appropriate access tier.

For a basic lab, use:

```text
Transaction optimized
```

Click:

```text
Review + create
```

Then:

```text
Create
```

---

### Step 4: Open the File Share

Open:

```text
azfileshare
```

You can now manage files and directories inside the share.

---

### Step 5: Create a Directory

Click:

```text
+ Add directory
```

Enter:

```text
documents
```

Click:

```text
OK
```

---

### Step 6: Upload a File

Open:

```text
documents
```

Click:

```text
Upload
```

Select a file from your local machine.

Example:

```text
sample.txt
```

Upload the file.

---

### Step 7: Verify the File

Verify that:

```text
documents/
└── sample.txt
```

exists inside the file share.

---

## Key Points

- **Azure Files** provides fully managed cloud file shares.
- Files are organized using directories and files.
- Azure Files supports **SMB and NFS**.
- Azure Files is different from Blob Storage.
- Azure Files provides Premium, Transaction optimized, Hot, and Cool tiers.
- Premium provides high-performance file shares.
- File access can be controlled using authentication and authorization mechanisms.
- Storage firewall, virtual network rules, and private endpoints can control network access.
- Azure Files is useful for shared file storage and migrating traditional file shares to Azure.
