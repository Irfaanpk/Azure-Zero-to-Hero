# 6.9 Blob Object Replication

**Azure Blob Object Replication** asynchronously copies block blobs from a **source storage account** to a **destination storage account**.

It can be used to maintain copies of blob data in different storage accounts, typically in different Azure regions.

---

## Why Use Object Replication?

Object Replication can help with:

- Data redundancy
- Disaster recovery
- Maintaining a copy of data in another region
- Read-access scenarios
- Processing data closer to users or applications

Example:

```text
Source Storage Account
        │
        │ Asynchronous Replication
        ▼
Destination Storage Account
```

---

## How Object Replication Works

Object Replication uses a **replication policy** between two storage accounts.

```text
Source Account
      │
      │ Replication Policy
      ▼
Destination Account
```

When a qualifying blob is created or modified in the source account, Azure asynchronously copies the blob to the destination account.

---

## Source and Destination

### Source Storage Account

The storage account containing the original blobs.

```text
Source
└── Container
    ├── file1.txt
    └── file2.txt
```

### Destination Storage Account

The storage account receiving replicated copies.

```text
Destination
└── Container
    ├── file1.txt
    └── file2.txt
```

---

## Asynchronous Replication

Object Replication is **asynchronous**.

This means changes are not guaranteed to appear in the destination immediately.

```text
Blob Created
     ↓
Source Account
     ↓
Replication
     ↓
Destination Account
```

There can be a delay between the source and destination.

---

## Object Replication Requirements

Important requirements include:

- Both storage accounts must support Blob Object Replication.
- Blob versioning must be enabled on both the source and destination accounts.
- The source and destination containers must exist.
- Object replication works with **block blobs**.
- The accounts must be configured appropriately for replication.

---

## Replication Policy

A **replication policy** defines how blobs are replicated between the source and destination storage accounts.

A policy contains:

- Source account
- Destination account
- Source container
- Destination container
- Replication rules

Example:

```text
Source Account
└── images
      │
      │ Replication Rule
      ▼
Destination Account
└── images
```

---

## Replication Rules

A replication rule determines which blobs should be replicated.

Rules can use:

- Source container
- Destination container
- Blob prefix
- Blob index tags

Example:

```text
Source Container: images
Blob Prefix: production/
        ↓
Replicate
        ↓
Destination Container: images
```

---

## Blob Prefix

A replication rule can use a **prefix** to select specific blobs.

Example:

```text
production/image1.jpg
production/image2.jpg
test/image1.jpg
```

If the prefix is:

```text
production/
```

only:

```text
production/image1.jpg
production/image2.jpg
```

will be selected for replication.

---

## Blob Index Tags

Blob index tags can also be used to identify blobs for replication.

Example:

```text
environment = production
```

The replication rule can target blobs matching the specified tag.

---

## Object Replication and Versioning

Object Replication requires **Blob Versioning**.

Versioning allows Azure to track changes to replicated blobs.

Example:

```text
Source
Blob Version 1
      ↓
Blob Version 2
      ↓
Blob Version 3

        ↓ Replication

Destination
Blob Version 1
      ↓
Blob Version 2
      ↓
Blob Version 3
```

---

## Object Replication vs Storage Redundancy

These are different features.

| Feature | Object Replication | Storage Redundancy |
|---|---|---|
| Purpose | Replicate selected blobs | Maintain copies for availability |
| Storage accounts | Two storage accounts | Usually one storage account |
| Control | Container/rule based | Storage account configuration |
| Replication | Asynchronous | Depends on redundancy type |
| Use case | Application/data replication | Availability and durability |

---

## Object Replication vs GRS

**GRS** and **GZRS** are storage redundancy options managed by Azure.

Object Replication provides more control over **which blobs** are replicated between storage accounts.

```text
GRS
Storage Account
      ↓
Azure-managed geographic replication
```

```text
Object Replication
Source Account
      ↓
Replication Rules
      ↓
Destination Account
```

---

# 🧪 Lab: Configure Blob Object Replication

### Step 1: Create Source Storage Account

Create or select a storage account.

Example:

```text
azstorage-source
```

Make sure Blob Versioning is enabled.

Go to:

```text
Data protection
```

Enable:

```text
Blob versioning
```

---

### Step 2: Create Destination Storage Account

Create another storage account.

Example:

```text
azstorage-destination
```

Enable:

```text
Blob versioning
```

---

### Step 3: Create Containers

In the source account, create:

```text
source-data
```

In the destination account, create:

```text
destination-data
```

---

### Step 4: Open Object Replication

In the source storage account, go to:

```text
Data management → Object replication
```

Select:

```text
Add replication rules
```

---

### Step 5: Configure Source and Destination

Select:

```text
Source storage account
```

and:

```text
Destination storage account
```

Configure:

```text
Source container:
source-data

Destination container:
destination-data
```

---

### Step 6: Configure Replication Rule

For the lab, configure the rule to replicate all applicable block blobs.

You can optionally use a prefix or blob index tag to select specific blobs.

Click:

```text
Save
```

---

### Step 7: Upload a Blob

Open:

```text
source-data
```

Upload:

```text
test.txt
```

---

### Step 8: Verify Replication

Open:

```text
destination-data
```

After asynchronous replication completes, verify that:

```text
test.txt
```

appears in the destination container.

> Replication is asynchronous, so the blob may not appear immediately.

---

## Important Points

- Object Replication copies blobs between **storage accounts**.
- Replication is **asynchronous**.
- It works with **block blobs**.
- **Blob Versioning must be enabled** on both accounts.
- Replication is configured using **replication policies and rules**.
- Rules can target blobs using prefixes and blob index tags.
- Source and destination containers must be configured.
- Object Replication is different from storage account redundancy such as GRS and GZRS.
- Object Replication provides more control over which blobs are replicated.
