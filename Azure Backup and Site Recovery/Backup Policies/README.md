# 16.2 Backup Policies

A **backup policy** defines **when backups are taken and how long recovery points are retained**.

It determines the backup schedule and retention settings for protected workloads.

---

## What is a Backup Policy?

A backup policy controls:

- Backup frequency
- Backup schedule
- Retention period
- Retention of recovery points

Example:

```text
Backup Policy
      │
      ├── Backup Schedule
      │       └── Daily
      │
      └── Retention
              ├── Daily → 30 days
              ├── Weekly → 12 weeks
              └── Monthly → 12 months
```

---

# Backup Schedule

The **backup schedule** determines when Azure Backup creates recovery points.

For example:

```text
Daily Backup
     │
     ├── Monday
     ├── Tuesday
     ├── Wednesday
     ├── Thursday
     ├── Friday
     ├── Saturday
     └── Sunday
```

The available schedule and configuration options depend on the workload and backup policy type.

---

# Retention

**Retention** determines how long recovery points are kept.

For example:

```text
Daily Recovery Point
        │
        └── Retain for 30 days

Weekly Recovery Point
        │
        └── Retain for 12 weeks

Monthly Recovery Point
        │
        └── Retain for 12 months
```

Retention allows you to restore data from an appropriate point in time.

---

# Recovery Points

Each successful backup creates a **recovery point**.

Example:

```text
Backup Policy
     │
     ▼
Daily Backup
     │
     ├── Recovery Point 1
     ├── Recovery Point 2
     ├── Recovery Point 3
     └── Recovery Point 4
```

If a VM is damaged or data is deleted, you can select an appropriate recovery point during the restore operation.

---

# Retention Types

Azure Backup policies can provide different retention periods depending on the workload.

Common retention concepts include:

| Retention | Purpose |
|---|---|
| Daily | Keep recent daily recovery points |
| Weekly | Keep selected weekly recovery points |
| Monthly | Keep selected monthly recovery points |
| Yearly | Keep selected yearly recovery points |

The exact options depend on the workload and backup policy configuration.

---

# Example Backup Policy

Suppose an organization wants:

```text
Backup Schedule
       │
       └── Daily

Retention
       │
       ├── Daily   → 30 days
       ├── Weekly  → 12 weeks
       └── Monthly → 12 months
```

This provides:

- Recent recovery points for everyday recovery
- Longer-term weekly recovery points
- Long-term monthly recovery points

---

# Backup Policy vs Recovery Point

These two concepts are different.

| Backup Policy | Recovery Point |
|---|---|
| Defines backup rules | Represents backed-up data at a point in time |
| Controls schedule | Created by a backup |
| Controls retention | Can be used for restore |
| Configuration | Recovery data |

Simple relationship:

```text
Backup Policy
      │
      ▼
Backup Schedule
      │
      ▼
Backup Operation
      │
      ▼
Recovery Point
      │
      ▼
Restore
```

---

# Changing a Backup Policy

A protected resource can be associated with a backup policy.

If the policy needs to change, you can modify the backup configuration according to the supported workload and vault settings.

Example:

```text
Existing Policy
Daily Backup
30 Days Retention
       │
       ▼
Updated Policy
Daily Backup
60 Days Retention
```

The new policy affects future backup and retention behavior; existing recovery points are handled according to Azure Backup's retention rules.

---

# Backup Policy Considerations

When designing a backup policy, consider:

### 1. Recovery Point Objective (RPO)

**RPO** defines how much recent data loss is acceptable.

For example:

```text
RPO = 24 hours
```

A daily backup may be suitable for this requirement.

---

### 2. Retention Requirements

Determine how long backups need to be available.

```text
Short-term
   ↓
Daily recovery points

Long-term
   ↓
Weekly / Monthly / Yearly recovery points
```

---

### 3. Storage Cost

Keeping more recovery points for longer periods can increase backup storage consumption and cost.

Therefore:

```text
Longer Retention
       +
More Recovery Points
       ↓
Potentially Higher Storage Cost
```

---

# Practical Example

### Requirement

A company wants to protect an Azure VM with:

- Daily backups
- 30-day daily retention
- Weekly retention
- Monthly long-term retention

### Configuration

```text
Azure VM
   │
   ▼
Recovery Services Vault
   │
   ▼
Backup Policy
   │
   ├── Daily Backup
   ├── Daily Retention
   ├── Weekly Retention
   └── Monthly Retention
```

The policy automatically creates and retains recovery points according to the configured schedule and retention rules.

---

# Key Points

- A **backup policy** controls when backups are taken and how long recovery points are retained.
- Backup schedules determine **when recovery points are created**.
- Retention determines **how long recovery points are kept**.
- Daily, weekly, monthly, and yearly retention can be used depending on the workload and policy.
- **RPO** helps determine how frequently backups should run.
- Longer retention can increase backup storage consumption and cost.
- Backup policies are associated with protected workloads.
- Recovery points created by the policy can later be used for restore operations.

---

## Next

➡️ **16.3 — Backup and Restore**
