# 6.6 Blob Versioning and Immutability

Azure Blob Storage provides **Blob Versioning** and **Blob Immutability** to protect data from accidental modification or deletion.

These features are useful for:

- Data recovery
- Accidental overwrite protection
- Compliance
- Data retention
- Protection against unwanted changes

---

## Blob Versioning

**Blob Versioning** automatically maintains previous versions of a blob whenever the blob is modified.

Instead of replacing the previous data permanently, Azure creates a new version.

Example:

```text
photo.jpg

Version 1 → Original
Version 2 → Modified
Version 3 → Modified again
```

The current blob remains available while previous versions are retained.

---

## How Blob Versioning Works

```text
Upload photo.jpg
       ↓
   Version 1
       ↓
Modify photo.jpg
       ↓
   Version 2
       ↓
Modify photo.jpg
       ↓
   Version 3
```

The latest version becomes the current version.

Previous versions can be used to recover earlier data.

---

## Why Use Blob Versioning?

Blob versioning helps protect against:

- Accidental overwrites
- Unwanted modifications
- Application errors
- Data corruption caused by updates

Example:

```text
Original File
     ↓
Application modifies file
     ↓
Wrong data uploaded
     ↓
Previous version available
     ↓
Recover original data
```

---

## Viewing Blob Versions

In the Azure Portal, you can view previous versions of a blob.

Go to:

```text
Storage Account
    ↓
Containers
    ↓
Container
    ↓
Blob
    ↓
Versions
```

You can view the available versions and their timestamps.

---

## Restoring a Previous Version

If a blob was accidentally modified, you can use an earlier version to restore the data.

Example:

```text
Current Version
      ↓
Incorrect Data

Previous Version
      ↓
Correct Data
```

You can copy the required previous version to restore the blob.

---

# Enable Blob Versioning

Blob versioning is configured at the **storage account level**.

Portal path:

```text
Storage Account
    ↓
Data protection
    ↓
Tracking
    ↓
Enable versioning for blobs
```

Enable:

```text
Blob versioning
```

Click:

```text
Save
```

---

# Blob Immutability

**Blob immutability** prevents data from being modified or deleted during a defined retention period.

It provides **WORM (Write Once, Read Many)** protection.

```text
Write Data
    ↓
Immutable
    ↓
Cannot Modify
    ↓
Cannot Delete
    ↓
Until Retention Period Ends
```

This is useful when data must remain unchanged for a specific period.

---

## Common Use Cases

Blob immutability can be used for:

- Compliance data
- Audit records
- Financial records
- Legal documents
- Regulatory requirements
- Long-term retention

---

## Immutability Policies

Azure Blob Storage supports retention policies that determine how long data must remain protected.

The main concepts are:

- Time-based retention policy
- Legal hold

---

## Time-Based Retention Policy

A **time-based retention policy** protects blob data for a specified period.

Example:

```text
Retention Period = 7 Years

Upload
  ↓
Data becomes immutable
  ↓
7 Years
  ↓
Retention expires
```

During the retention period:

```text
Modify → ❌
Delete → ❌
Read   → ✅
```

---

## Legal Hold

A **legal hold** protects data without specifying a fixed retention period.

It is useful when data must be retained because of:

- Legal investigations
- Litigation
- Regulatory requirements

Example:

```text
Legal Hold
    ↓
Blob Protected
    ↓
Cannot Modify/Delete
    ↓
Legal Hold Removed
```

A legal hold can use **tags** to identify the reason for the hold.

Example:

```text
Legal
Audit
Investigation
```

---

## Time-Based Retention vs Legal Hold

| Feature | Time-Based Retention | Legal Hold |
|---|---|---|
| Retention period | Defined | No fixed expiration |
| Purpose | Fixed retention requirement | Legal/compliance requirement |
| Data modification | ❌ | ❌ |
| Data deletion | ❌ | ❌ |
| Protection ends | After retention period | When legal hold is removed |

---

## Locked and Unlocked Retention Policies

A time-based retention policy can be:

### Unlocked

The policy can be changed during the allowed configuration period.

Useful while configuring and testing the retention settings.

### Locked

The policy becomes protected and cannot be shortened or deleted.

A locked policy is intended for compliance scenarios where the retention requirement must be enforced.

```text
Unlocked
   ↓
Configure/Test
   ↓
Lock Policy
   ↓
Retention Protected
```

---

## Versioning and Immutability Together

Blob versioning and immutability solve different problems.

### Versioning

Protects previous versions of modified blobs.

```text
Version 1
Version 2
Version 3
```

### Immutability

Prevents protected data from being modified or deleted during retention.

```text
Protected Data
      ↓
Cannot Modify
Cannot Delete
```

They can be used together when both **recovery** and **retention protection** are required.

---

# 🧪 Lab: Enable Blob Versioning and Configure Immutability

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

### Step 2: Enable Blob Versioning

Go to:

```text
Data protection
```

Enable:

```text
Enable versioning for blobs
```

Click:

```text
Save
```

---

### Step 3: Create a Container

Go to:

```text
Data storage → Containers
```

Create a container:

```text
versioning-lab
```

Keep the access level:

```text
Private
```

---

### Step 4: Upload a Blob

Upload a file such as:

```text
document.txt
```

---

### Step 5: Modify the Blob

Change the contents of:

```text
document.txt
```

Upload the modified file using the same blob name.

Azure creates another version.

Example:

```text
document.txt
├── Current Version
├── Previous Version
└── Older Version
```

---

### Step 6: View Versions

Open the blob and view its versions.

Verify that multiple versions are available.

---

### Step 7: Configure Immutability

For the lab, configure an appropriate **time-based retention policy** on the blob/container according to the options available in the Portal.

Example:

```text
Retention Period: 1 Day
```

> Use a short period for a learning lab so you can test the behavior without creating a long-term retention commitment.

---

### Step 8: Test Protection

During the retention period, attempt to:

```text
Modify → ❌
Delete  → ❌
Read    → ✅
```

Verify that the retention policy protects the data.

---

## Important Points

- Blob Versioning keeps previous versions when blobs are modified.
- Versioning helps recover from accidental overwrites.
- Blob immutability provides **WORM** protection.
- Immutable blobs cannot be modified or deleted during the applicable retention period.
- **Time-based retention** protects data for a defined period.
- **Legal hold** protects data until the hold is removed.
- A **locked** retention policy provides stronger compliance protection.
- Versioning and immutability can be used together.
- Versioning is configured at the storage account level.
- Immutability is primarily used for retention and compliance requirements.
