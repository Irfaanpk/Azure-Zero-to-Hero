# 6.3 Azure Blob Storage

**Azure Blob Storage** is Azure's object storage service for storing large amounts of unstructured data.

It is commonly used to store:

- Images
- Videos
- Documents
- Backups
- Logs
- Application files
- Large datasets

Blob Storage is designed to store data as **objects called blobs** inside **containers**.

---

## Blob Storage Structure

Blob Storage has three main components:

```text
Storage Account
└── Container
    ├── Blob
    ├── Blob
    └── Blob
```

### Storage Account

The storage account provides the namespace and configuration for the stored data.

Example:

```text
azstorage2026demo
```

### Container

A **container** organizes blobs inside a storage account.

Example:

```text
images
documents
backups
logs
```

### Blob

A **blob** is an individual object stored inside a container.

Example:

```text
images/
├── photo1.jpg
├── photo2.png
└── logo.png
```

---

# Blob Containers

A **container** is a logical grouping of blobs.

Containers help organize data within a storage account.

Example:

```text
Storage Account
│
├── images
│   ├── photo1.jpg
│   └── photo2.jpg
│
├── documents
│   ├── report.pdf
│   └── invoice.pdf
│
└── backups
    └── backup.zip
```

A container name must follow Azure naming requirements.

Container names:

- Must be lowercase
- Can contain letters, numbers, and hyphens
- Must start and end with a letter or number
- Must be between 3 and 63 characters

Example:

```text
application-logs
```

---

## Blob Types

Azure Blob Storage supports three types of blobs:

1. Block blobs
2. Append blobs
3. Page blobs

---

## 1. Block Blobs

**Block blobs** are the most commonly used blob type.

They are designed for storing:

- Documents
- Images
- Videos
- Backups
- Application files

Example:

```text
report.pdf
image.jpg
video.mp4
backup.zip
```

For most general object storage requirements, **Block Blob** is the primary blob type to understand.

---

## 2. Append Blobs

**Append blobs** are optimized for data that is continuously added to the end of the blob.

Common use case:

```text
Application Logs
      ↓
Append Blob
      ↓
New log entries added
```

They are useful for workloads such as:

- Logging
- Audit data
- Sequentially appended information

---

## 3. Page Blobs

**Page blobs** are optimized for random read and write operations.

They are primarily used for scenarios such as:

- Virtual hard disks
- Azure Managed Disks

For AZ-104, understand that page blobs are designed for random-access workloads and are associated with disk storage scenarios.

---

# Blob Storage Tiers

Blob Storage provides different **access tiers** to optimize storage cost based on how frequently data is accessed.

The main tiers are:

- **Hot**
- **Cool**
- **Cold**
- **Archive**

> **Access tier is different from Blob Access Level.**
>
> **Access tier** controls the cost model based on data access frequency.
>
> **Access level** controls anonymous/public access to blob data.

---

## 1. Hot Tier

The **Hot** tier is designed for data that is accessed frequently.

Examples:

- Frequently accessed images
- Application data
- Frequently used documents
- Active website content

```text
Frequent Access
      ↓
    HOT
```

It has higher storage costs but lower access costs compared with cooler tiers.

---

## 2. Cool Tier

The **Cool** tier is designed for data that is accessed less frequently.

Examples:

- Short-term backups
- Older documents
- Infrequently accessed data

```text
Less Frequent Access
        ↓
      COOL
```

It generally provides lower storage costs than Hot but higher access costs.

---

## 3. Cold Tier

The **Cold** tier is designed for data that is rarely accessed but still needs to remain immediately available.

Examples:

- Long-term data that may occasionally be accessed
- Older backups
- Infrequently accessed datasets

```text
Rare Access
    ↓
  COLD
```

It provides lower storage costs than Cool, with higher access costs.

---

## 4. Archive Tier

The **Archive** tier is designed for data that is rarely accessed and can tolerate a longer retrieval time.

Examples:

- Long-term backups
- Compliance data
- Historical records
- Archived documents

```text
Very Rare Access
       ↓
    ARCHIVE
```

Archive provides very low storage costs but requires **rehydration** before the data can be read.

---

## Blob Access Tier Comparison

| Tier | Access Frequency | Storage Cost | Access Cost | Availability |
|---|---|---|---|---|
| **Hot** | Frequent | Higher | Lower | Online |
| **Cool** | Infrequent | Lower | Higher | Online |
| **Cold** | Rare | Lower | Higher | Online |
| **Archive** | Very rare | Lowest | Highest | Offline until rehydrated |

---

# Blob Tier Use Cases

```text
Frequently accessed
        ↓
       Hot

Infrequently accessed
        ↓
      Cool

Rarely accessed
        ↓
      Cold

Very rarely accessed
        ↓
     Archive
```

Choose the tier based on **how frequently the data is accessed**.

---

# Blob Storage URLs

A blob can be accessed using a URL based on the storage account, container, and blob name.

Example:

```text
https://<storage-account>.blob.core.windows.net/<container>/<blob>
```

Example:

```text
https://azstorage2026demo.blob.core.windows.net/images/photo.jpg
```

---

# Blob Metadata and Properties

Azure Blob Storage maintains information about stored blobs.

Common properties include:

- Blob name
- Blob type
- Size
- Content type
- Last modified time
- Access tier
- ETag

Blob metadata can also be used to store additional key-value information about an object.

Example:

```text
department = finance
document-type = invoice
```

---

# Blob Storage and Access

Blob data can be accessed using different authentication and authorization mechanisms.

Common methods include:

- Microsoft Entra ID
- Azure RBAC
- Shared Access Signatures (SAS)
- Storage account access keys

These access mechanisms will be covered in more detail in later topics.

---

# 🧪 Lab: Create a Blob Container and Upload a Blob

### Step 1: Open the Storage Account

Open:

```text
https://portal.azure.com
```

Go to:

```text
Storage accounts
```

Select the storage account created in the previous lab.

---

### Step 2: Open Containers

In the storage account, select:

```text
Data storage → Containers
```

Click:

```text
+ Container
```

---

### Step 3: Create a Container

Enter:

```text
images
```

For **Anonymous access level**, keep:

```text
Private (no anonymous access)
```

Click:

```text
Create
```

---

### Step 4: Open the Container

Select:

```text
images
```

---

### Step 5: Upload a Blob

Click:

```text
Upload
```

Select an image or document from your local machine.

Example:

```text
photo.jpg
```

Click:

```text
Upload
```

The blob will now appear inside the container.

---

### Step 6: View Blob Properties

Select the uploaded blob.

You can view information such as:

- Name
- URL
- Size
- Blob type
- Access tier
- Last modified time
- Metadata

---

### Step 7: Change the Access Tier

Select the blob and choose:

```text
Change tier
```

You can select:

```text
Hot
Cool
Cold
Archive
```

For this lab, keep the blob as:

```text
Hot
```

---

# Key Points

- **Azure Blob Storage** is an object storage service.
- Blobs are stored inside **containers**.
- Containers are stored inside a **storage account**.
- **Block blobs** are the most commonly used blob type.
- **Append blobs** are useful for continuously appended data such as logs.
- **Page blobs** are designed for random read/write workloads.
- Blob access tiers are **Hot, Cool, Cold, and Archive**.
- **Archive** requires rehydration before the data can be accessed.
- Access tier is different from anonymous access level.
- Blob Storage can store large amounts of unstructured data.
