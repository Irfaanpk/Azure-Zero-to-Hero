# 6.8 Blob Lifecycle Management

**Azure Blob Storage Lifecycle Management** allows you to automatically move blobs between access tiers or delete blobs when they meet specific conditions.

It helps reduce storage costs and automatically manage data throughout its lifecycle.

---

## Why Use Lifecycle Management?

Without lifecycle management, administrators may need to manually:

- Move old blobs to cheaper storage tiers
- Archive rarely accessed data
- Delete old data

Lifecycle Management automates these operations.

Example:

```text
New Data
   ↓
   Hot
   ↓
After 30 Days
   ↓
  Cool
   ↓
After 90 Days
   ↓
 Archive
   ↓
After 7 Years
   ↓
 Delete
```

---

## Lifecycle Management Policy

A lifecycle management policy consists of:

- **Rules**
- **Conditions**
- **Actions**

Example:

```text
Rule
 ↓
Blob age > 30 days
 ↓
Move to Cool
```

---

## Lifecycle Rule

A **lifecycle rule** defines when and what action should be performed on blobs.

Example:

```text
Rule: Move old blobs to Cool
```

Conditions:

```text
Blob has been modified more than 30 days ago
```

Action:

```text
Move to Cool tier
```

---

## Lifecycle Actions

Common lifecycle actions include:

### Tiering Actions

Move blobs to a different access tier:

```text
Hot → Cool
Hot → Cold
Hot → Archive
Cool → Cold
Cool → Archive
Cold → Archive
```

The available transitions depend on Azure Storage lifecycle management rules and blob configuration.

---

### Delete Action

Lifecycle Management can automatically delete blobs after a specified period.

Example:

```text
Blob modified
     ↓
180 days
     ↓
Delete Blob
```

---

## Conditions

Lifecycle rules can use conditions based on blob age and other properties.

Common conditions include:

- Days since modification
- Days since creation
- Days since last access
- Blob index tag conditions

Example:

```text
If blob has not been modified for 30 days
        ↓
Move to Cool
```

---

## Last Modified Date

Lifecycle rules can use the blob's **last modified date** to determine when an action should occur.

Example:

```text
Last Modified
     ↓
30 Days
     ↓
Move to Cool
```

This is useful when data becomes less important as it gets older.

---

## Last Access Time

Azure Storage can also use **last access time** for lifecycle management.

Example:

```text
Blob accessed
     ↓
No access for 90 days
     ↓
Move to Cool
```

This is useful when the access frequency is more important than when the blob was modified.

---

## Blob Index Tags

Lifecycle rules can also use **blob index tags** to target specific blobs.

Example:

```text
environment = archive
```

A lifecycle rule can target blobs with a particular tag.

```text
Blob Index Tag
      ↓
environment = archive
      ↓
Lifecycle Rule
      ↓
Move to Archive
```

---

## Example Lifecycle Policy

Suppose an organization stores application logs.

The policy could be:

```text
0–30 Days
    ↓
Hot

31–90 Days
    ↓
Cool

91–365 Days
    ↓
Archive

After 365 Days
    ↓
Delete
```

This reduces storage costs while automatically managing old data.

---

## Lifecycle Management and Blob Versions

Lifecycle policies can also be used to manage **previous blob versions**.

Example:

```text
Current Blob
     ↓
Previous Versions
     ↓
Keep for 90 Days
     ↓
Delete Previous Versions
```

This helps prevent old versions from accumulating indefinitely.

---

## Lifecycle Management and Snapshots

Lifecycle policies can also manage **blob snapshots**.

Example:

```text
Snapshot
   ↓
Keep for 30 Days
   ↓
Delete Snapshot
```

This can help control storage usage from old snapshots.

---

## Lifecycle Management vs Soft Delete

These features have different purposes.

| Feature | Purpose |
|---|---|
| **Lifecycle Management** | Automatically moves or deletes data based on rules |
| **Soft Delete** | Allows recovery of deleted data during a retention period |

Example:

```text
Lifecycle Management
        ↓
Automatically Delete
        ↓
Soft Delete Protection
        ↓
Recover During Retention Period
```

---

## Lifecycle Management vs Versioning

| Feature | Purpose |
|---|---|
| **Versioning** | Keeps previous versions of modified blobs |
| **Lifecycle Management** | Automatically manages current/previous data based on rules |

They can be used together.

---

# 🧪 Lab: Configure a Blob Lifecycle Management Rule

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

### Step 2: Open Lifecycle Management

Go to:

```text
Data management → Lifecycle management
```

Click:

```text
Add a rule
```

---

### Step 3: Configure the Rule

Enter a rule name:

```text
move-old-blobs
```

Select the blob types:

```text
Block blobs
```

---

### Step 4: Configure Tiering

Configure the rule to move blobs to a cooler tier based on age.

Example:

```text
Last modified > 30 days
```

Action:

```text
Move to Cool tier
```

---

### Step 5: Add Delete Condition

You can also configure a delete action.

Example:

```text
Last modified > 180 days
```

Action:

```text
Delete the blob
```

---

### Step 6: Review and Add

Review the rule and click:

```text
Add
```

Azure will automatically evaluate the lifecycle policy and perform the configured actions when the conditions are met.

---

## Important Points

- Lifecycle Management automates Blob Storage data management.
- It can automatically move blobs between access tiers.
- It can automatically delete blobs.
- Rules contain conditions and actions.
- Rules can use blob age and other conditions.
- Last modified time can be used to trigger actions.
- Last access time can be used when access tracking is configured.
- Blob index tags can be used to target specific blobs.
- Lifecycle policies can manage previous blob versions and snapshots.
- Lifecycle Management helps reduce storage costs.
- Lifecycle Management is different from Blob Soft Delete.
