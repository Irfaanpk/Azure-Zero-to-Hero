# 6.7 Blob Soft Delete and Snapshots

Azure Blob Storage provides **Blob Soft Delete** and **Blob Snapshots** to help protect and recover blob data.

These features can help protect against:

- Accidental deletion
- Accidental modification
- Application errors
- Unwanted data changes

---

## Blob Soft Delete

**Blob Soft Delete** allows deleted blobs to be recovered during a configured retention period.

When a blob is deleted, Azure does not immediately remove it permanently.

Instead, the blob is retained for the configured period.

```text
Blob
 ↓
Delete
 ↓
Soft Deleted
 ↓
Retention Period
 ↓
Permanent Deletion
```

During the retention period, the deleted blob can be restored.

---

## Why Use Blob Soft Delete?

Blob Soft Delete helps protect against:

- Accidental deletion
- User mistakes
- Application bugs
- Unexpected data loss

Example:

```text
Important-file.pdf
       ↓
Accidentally Deleted
       ↓
Soft Deleted
       ↓
Restore
       ↓
Important-file.pdf
```

---

## Soft Delete Retention Period

When enabling Blob Soft Delete, you configure a **retention period**.

Example:

```text
Retention Period = 7 Days
```

If a blob is deleted:

```text
Day 1 → Deleted
Day 2 → Recoverable
Day 3 → Recoverable
...
Day 7 → Recoverable
After retention → Permanently deleted
```

The exact available retention period can be configured according to Azure Storage limits.

---

## Restore a Soft-Deleted Blob

A soft-deleted blob can be restored while it is within the retention period.

Example:

```text
Container
├── file1.txt
├── file2.txt
└── file3.txt

file2.txt
    ↓
Deleted
    ↓
Soft Deleted
    ↓
Undelete
    ↓
file2.txt restored
```

---

# Blob Snapshots

A **Blob Snapshot** is a read-only point-in-time copy of a blob.

It allows you to preserve the state of a blob at a specific point in time.

Example:

```text
Current Blob
    ↓
Create Snapshot
    ↓
Snapshot 1
```

The original blob can continue to change while the snapshot preserves its earlier state.

---

## Why Use Blob Snapshots?

Snapshots are useful when you want to preserve a previous state of a blob before making changes.

Example:

```text
Original Blob
     ↓
Create Snapshot
     ↓
Modify Blob
     ↓
Something goes wrong
     ↓
Use Snapshot to recover data
```

---

## Snapshot Example

Suppose:

```text
document.txt
```

contains:

```text
Version A
```

Create a snapshot:

```text
Snapshot → Version A
```

Then modify the blob:

```text
document.txt → Version B
```

The snapshot still represents:

```text
Version A
```

---

## Snapshot Characteristics

A blob snapshot:

- Is a point-in-time copy of a blob
- Is read-only
- Preserves the blob's state at that point in time
- Can be used for recovery and backup scenarios
- Does not replace the original blob

---

# Soft Delete vs Snapshots

| Feature | Blob Soft Delete | Blob Snapshot |
|---|---|---|
| Purpose | Recover deleted blobs | Preserve a point-in-time state |
| Trigger | Blob deletion | Manually or programmatically created |
| Recovery | Restore deleted blob | Use snapshot data |
| Read-only | Not applicable | Yes |
| Retention | Configured retention period | Managed separately |
| Protects against deletion | ✅ | ❌ |
| Preserves previous state | ✅ During deletion recovery | ✅ |

---

# Soft Delete vs Versioning

Blob Soft Delete and Blob Versioning are also different.

| Feature | Soft Delete | Versioning |
|---|---|---|
| Main purpose | Recover deleted blobs | Keep previous blob versions |
| Trigger | Blob deletion | Blob modification |
| Previous versions | ❌ | ✅ |
| Recover deleted blob | ✅ | Can help depending on configuration |
| Retention | Retention period | Versions retained according to configuration |

---

# Enable Blob Soft Delete

Blob Soft Delete can be enabled from the Storage Account.

Portal path:

```text
Storage Account
    ↓
Data protection
    ↓
Recovery
    ↓
Enable soft delete for blobs
```

Configure the required retention period.

Example:

```text
Retention Period: 7 Days
```

Click:

```text
Save
```

---

# Create a Blob Snapshot

Snapshots can be created for an individual blob.

Portal path:

```text
Storage Account
    ↓
Containers
    ↓
Container
    ↓
Blob
    ↓
Create Snapshot
```

Azure creates a point-in-time snapshot of the selected blob.

---

# 🧪 Lab: Test Blob Soft Delete and Snapshots

### Step 1: Enable Soft Delete

Open your Storage Account.

Go to:

```text
Data protection
```

Enable:

```text
Soft delete for blobs
```

Set a short retention period for the lab.

Example:

```text
1 Day
```

Click:

```text
Save
```

---

### Step 2: Upload a Blob

Go to:

```text
Data storage → Containers
```

Open an existing container or create:

```text
soft-delete-lab
```

Upload:

```text
important.txt
```

---

### Step 3: Delete the Blob

Select:

```text
important.txt
```

Click:

```text
Delete
```

Confirm the deletion.

---

### Step 4: Restore the Blob

Open the container and locate the deleted blob.

Select the deleted blob and choose:

```text
Undelete
```

The blob should be restored.

---

### Step 5: Create a Snapshot

Upload or select another blob.

Example:

```text
document.txt
```

Open the blob and select:

```text
Create Snapshot
```

Azure creates a point-in-time snapshot.

---

### Step 6: Modify the Original Blob

Change the contents of:

```text
document.txt
```

Upload the modified version using the same blob name.

The original snapshot remains unchanged.

---

### Step 7: Verify the Snapshot

Open the blob and view its snapshots.

Verify that the snapshot represents the earlier state of the blob.

---

## Important Points

- **Blob Soft Delete** protects against accidental blob deletion.
- Deleted blobs remain recoverable during the configured retention period.
- **Blob Snapshots** preserve a point-in-time state of a blob.
- Snapshots are read-only.
- Soft Delete and Snapshots provide different types of data protection.
- Soft Delete is mainly for **deletion recovery**.
- Snapshots are mainly for **point-in-time recovery and preservation**.
- These features can be used together with **Blob Versioning** for additional data protection.
