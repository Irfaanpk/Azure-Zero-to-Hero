# 16.3 Backup and Restore

Azure Backup allows you to **create backups of protected workloads and restore data or entire resources from recovery points** when required.

For AZ-104, the main focus is backing up and restoring **Azure Virtual Machines**.

---

## Backup Process

The basic Azure VM backup workflow is:

```text
Azure Virtual Machine
        │
        ▼
Recovery Services Vault
        │
        ▼
Backup Policy
        │
        ▼
Backup Operation
        │
        ▼
Recovery Point
```

Once a VM is protected, Azure Backup automatically creates recovery points according to the configured backup policy.

---

# On-Demand Backup

An **on-demand backup** allows you to create a backup immediately instead of waiting for the scheduled backup.

Example:

```text
Scheduled Backup
       ↓
Runs according to policy

On-Demand Backup
       ↓
Run Backup Now
       ↓
Recovery Point Created
```

This is useful before:

- Major application changes
- Configuration changes
- Software upgrades
- Maintenance activities

---

# Backup a Virtual Machine

The general process is:

```text
1. Create Recovery Services Vault
            ↓
2. Configure Backup
            ↓
3. Select Azure Virtual Machine
            ↓
4. Select Backup Policy
            ↓
5. Enable Backup
            ↓
6. Run Initial Backup
            ↓
7. Recovery Point Created
```

After protection is enabled, the VM appears under the vault's protected items.

---

# Restore Operations

When a VM or its data needs to be recovered:

```text
Recovery Services Vault
        │
        ▼
Protected Item
        │
        ▼
Recovery Points
        │
        ▼
Select Recovery Point
        │
        ▼
Choose Restore Option
        │
        ▼
Restore
```

You select the appropriate recovery point based on the required point in time.

---

# Restore Options

Azure VM Backup provides different restore options depending on the scenario and supported configuration.

Common restore scenarios include:

### Restore VM

Restore the backed-up VM as a new VM.

```text
Recovery Point
      ↓
Restore VM
      ↓
New Azure VM
```

This is useful when the original VM is unavailable or needs to be recovered separately.

---

### Restore Disks

Restore the VM's disks from a recovery point.

```text
Recovery Point
      ↓
Restore Disks
      ↓
Managed Disks
      ↓
Attach / Use with VM
```

This provides more control over how the recovered disks are used.

---

### Restore Files

For supported Azure VM backup scenarios, individual files can be recovered from a recovery point without restoring the entire VM.

```text
Recovery Point
      ↓
File Recovery
      ↓
Select Files
      ↓
Recover Required Data
```

---

# Restore to Another Region

Azure Backup can support restoring data to another region when the vault and backup configuration support the required recovery scenario.

Example:

```text
Primary Region
      │
      ▼
Azure VM
      │
      ▼
Azure Backup
      │
      ▼
Recovery Point
      │
      ▼
Secondary Region
      │
      ▼
Restore
```

This can help with regional disaster recovery scenarios.

---

# Restore Point Selection

Choosing the correct recovery point is important.

Example:

```text
Monday      Tuesday      Wednesday
  │            │             │
  ▼            ▼             ▼
RP-01        RP-02         RP-03
                              │
                         Data corrupted
                              │
                              ▼
                    Restore from RP-02
```

You should select a recovery point that contains the required version of the data.

---

# Backup vs Restore

| Backup | Restore |
|---|---|
| Creates recovery data | Recovers data |
| Protects workloads | Recovers protected workloads |
| Creates recovery points | Uses recovery points |
| Usually scheduled | Performed when recovery is required |

Simple relationship:

```text
Backup
  ↓
Recovery Point
  ↓
Restore
```

---

# Backup Job

A **backup job** represents a backup operation being performed or completed.

You can use the vault to review information such as:

- Backup jobs
- Job status
- Start and end time
- Protected item
- Operation details

Typical job states include:

```text
In Progress
     ↓
Completed

or

In Progress
     ↓
Failed
```

Monitoring backup jobs helps verify whether backups are completing successfully.

---

# Restore Job

Restore operations are also tracked as jobs.

Example:

```text
Restore Request
      ↓
Restore Job
      ↓
Processing
      ↓
Completed / Failed
```

If a restore fails, the job details can help identify the problem.

---

# Practical Lab

## Lab — Configure Azure VM Backup and Restore

### Objective

Create an Azure VM backup, create a recovery point, and restore the VM from that recovery point.

### Architecture

```text
Azure VM
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
Restore
   │
   ▼
Recovered VM
```

### Steps

1. Create an **Azure Virtual Machine**.
2. Open **Backup** from the VM menu.
3. Select **Recovery Services vault**.
4. Create or select a Recovery Services vault.
5. Select an appropriate backup policy.
6. Enable backup.
7. Trigger an **on-demand backup**.
8. Wait for the backup job to complete.
9. Open the vault and verify the recovery point.
10. Open the protected VM.
11. Select **Restore VM**.
12. Select the required recovery point.
13. Configure the restore destination and VM settings.
14. Start the restore operation.
15. Verify that the recovered VM is created successfully.
16. Review the backup and restore jobs.

---

# Key Points

- Azure Backup creates **recovery points** that can be used for recovery.
- Backups can run automatically according to a **backup policy**.
- **On-demand backup** allows an immediate backup.
- Azure VM backups can be restored using supported restore options.
- You can restore an entire VM, disks, or files depending on the scenario.
- Backup and restore operations can be monitored through **backup jobs** and **restore jobs**.
- Recovery point selection determines which point in time is recovered.
- Azure Backup can support cross-region recovery scenarios when configured and supported.

---

## Next

➡️ **16.4 — Azure Site Recovery**
