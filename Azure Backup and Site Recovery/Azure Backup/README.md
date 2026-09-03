# 16.1 Azure Backup

Azure Backup is a fully managed Azure service used to **protect data and workloads by creating backups and recovery points**.

It helps protect Azure resources from:

- Accidental deletion
- Data corruption
- Ransomware and other security incidents
- Application or configuration failures
- VM failures
- Disaster scenarios

---

## What is Azure Backup?

**Azure Backup** provides backup and restore capabilities for supported Azure and on-premises workloads without requiring you to manage traditional backup infrastructure.

For example:

```text
Azure Virtual Machine
        ↓
   Azure Backup
        ↓
Recovery Services Vault
        ↓
 Recovery Points
        ↓
      Restore
```

---

## Why Use Azure Backup?

Without backups:

```text
VM / Data
   ↓
Failure / Deletion
   ↓
Data Lost
```

With Azure Backup:

```text
VM / Data
   ↓
Azure Backup
   ↓
Recovery Point
   ↓
Failure / Deletion
   ↓
Restore Data
```

---

## What Can Azure Backup Protect?

Azure Backup supports multiple workloads, including:

- Azure Virtual Machines
- Azure Files
- Azure Disks
- SQL Server in Azure VMs
- SAP HANA in Azure VMs
- On-premises workloads using supported backup agents
- Other supported Azure workloads

For AZ-104, the most important workload to understand is **Azure Virtual Machine backup**.

---

# Backup Architecture

A basic Azure VM backup architecture looks like this:

```text
┌───────────────────────┐
│   Azure Virtual       │
│       Machine         │
└───────────┬───────────┘
            │
            │ Backup
            ▼
┌───────────────────────┐
│   Recovery Services   │
│        Vault           │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│   Recovery Points     │
│                       │
│  Backup Data          │
│  Retention            │
│  Recovery Information │
└───────────────────────┘
```

The vault acts as a **management and protection boundary** for backup data and recovery operations.

---

# Recovery Services Vault

A **Recovery Services vault** is an Azure resource used to store and manage backup data and recovery points for supported workloads.

It provides capabilities such as:

- Backup management
- Recovery point management
- Restore operations
- Backup policies
- Monitoring
- Backup security settings

Example:

```text
Subscription
     │
     ├── Resource Group
     │      │
     │      ├── Virtual Machine
     │      │
     │      └── Recovery Services Vault
     │
     └── Other Resources
```

---

# Backup Vault

Azure also provides a **Backup vault** for newer Azure Backup workloads.

A Backup vault is designed to store and manage backup data for supported workloads.

Conceptually:

```text
Azure Backup
     │
     ├── Recovery Services Vault
     │       └── Supported workloads
     │
     └── Backup Vault
             └── Supported newer workloads
```

For AZ-104, understand that **Recovery Services vaults and Backup vaults are different vault types used by Azure Backup for different supported workloads**.

---

# Backup and Recovery Points

A **recovery point** represents a point in time from which protected data can be restored.

Example:

```text
Monday
   │
   ├── Recovery Point 01
   │
Tuesday
   │
   ├── Recovery Point 02
   │
Wednesday
   │
   ├── Recovery Point 03
   │
Thursday
   │
   └── Recovery Point 04
```

If data is accidentally deleted on Thursday, you can restore from an appropriate recovery point.

---

# Backup Policy

A **backup policy** defines how backups are performed and how long recovery points are retained.

A policy can define:

- Backup schedule
- Backup frequency
- Retention period
- Recovery point retention

Example:

```text
Backup Policy
     │
     ├── Daily Backup
     │
     ├── Retain Daily Recovery Points
     │
     ├── Weekly Retention
     │
     └── Monthly Retention
```

Backup policies are covered in detail in **16.2 Backup Policies**.

---

# Azure VM Backup

Azure Backup can protect Azure Virtual Machines.

Basic process:

```text
1. Create Recovery Services Vault
              ↓
2. Create Backup Policy
              ↓
3. Select Virtual Machine
              ↓
4. Enable Backup
              ↓
5. Backup runs according to policy
              ↓
6. Recovery Point is created
              ↓
7. Restore when required
```

---

# Backup vs Snapshot

Azure Backup and VM snapshots are not the same thing.

| Feature | Azure Backup | Snapshot |
|---|---|---|
| Purpose | Backup and recovery | Point-in-time disk copy |
| Managed backup service | Yes | No |
| Backup policy | Yes | No |
| Retention management | Yes | Manual |
| Recovery points | Yes | Snapshot |
| Restore capabilities | Multiple backup restore options | Disk-level restore/use |
| Recommended for backup strategy | Yes | Not a complete backup solution |

A snapshot can be useful for certain operational scenarios, but **Azure Backup is designed specifically for backup and recovery**.

---

# Backup Security

Azure Backup provides security capabilities to help protect backup data.

Important concepts include:

- Vault security
- Soft delete
- Immutability
- Encryption
- Multi-user authorization
- Monitoring and alerts

These features help protect backups from accidental deletion and malicious actions.

---

# Azure Backup Workflow

```text
                  Azure Virtual Machine
                           │
                           ▼
                    Azure Backup
                           │
                           ▼
               Recovery Services Vault
                           │
                           ▼
                    Backup Policy
                           │
                           ▼
                    Recovery Point
                           │
                           ▼
                  Restore When Needed
```

---

# Key Points

- **Azure Backup** provides managed backup and recovery.
- **Recovery Services vault** is used to manage backup data and recovery points for supported workloads.
- **Backup vault** is another vault type used for supported Azure Backup workloads.
- A **recovery point** represents a point in time from which data can be restored.
- A **backup policy** controls backup schedules and retention.
- Azure Backup supports Azure VMs and several other workloads.
- Azure Backup is different from a simple VM disk snapshot.
- Backup security features help protect recovery data.

---

## Next

➡️ **16.2 — Backup Policies**
