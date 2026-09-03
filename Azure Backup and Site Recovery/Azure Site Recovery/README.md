# 16.4 Azure Site Recovery

**Azure Site Recovery (ASR)** is a disaster recovery service that helps protect Azure Virtual Machines and other supported workloads by **replicating them to a secondary location**.

If the primary region becomes unavailable, Site Recovery can be used to **fail over workloads to the secondary region**.

---

## What is Azure Site Recovery?

Azure Site Recovery provides **business continuity and disaster recovery (BCDR)** for supported workloads.

Example:

```text
Primary Region
      │
      │ Replication
      ▼
Secondary Region
      │
      │
      ▼
  Disaster
      │
      ▼
   Failover
      │
      ▼
VM runs in Secondary Region
```

---

# Azure Backup vs Azure Site Recovery

Azure Backup and Azure Site Recovery solve different problems.

| Azure Backup | Azure Site Recovery |
|---|---|
| Backup and restore service | Disaster recovery service |
| Creates recovery points | Replicates workloads |
| Used to recover data/resources | Used to recover workloads after a disaster |
| Point-in-time recovery | Failover to another location |
| Focuses on data protection | Focuses on business continuity |

Simple difference:

```text
Azure Backup
     ↓
Backup → Recovery Point → Restore


Azure Site Recovery
     ↓
Replication → Failover → Workload Recovery
```

---

# Disaster Recovery

A disaster could be:

- Azure region failure
- Major infrastructure failure
- Application failure
- Other events affecting workload availability

A disaster recovery architecture can use a secondary region:

```text
┌──────────────────────┐
│    Primary Region    │
│                      │
│      Azure VM        │
└──────────┬───────────┘
           │
           │ Replication
           ▼
┌──────────────────────┐
│   Secondary Region   │
│                      │
│  Replicated Workload │
└──────────────────────┘
```

---

# Replication

**Replication** continuously copies changes from the primary workload to the recovery location.

```text
Primary VM
   │
   │ Changes
   ▼
Site Recovery
   │
   │ Replication
   ▼
Secondary Region
```

The replicated workload can be used during a failover.

---

# Recovery Services Vault

Azure Site Recovery commonly uses a **Recovery Services vault** to manage disaster recovery configuration.

The vault can contain:

- Replication configuration
- Protected items
- Recovery plans
- Failover operations
- Recovery information

Example:

```text
Recovery Services Vault
        │
        ├── Replicated Items
        │
        ├── Recovery Plans
        │
        └── Failover Operations
```

---

# Protected Items

A workload configured for Site Recovery becomes a **protected item**.

Example:

```text
Azure VM
   ↓
Enable Replication
   ↓
Protected Item
   ↓
Replicated to Secondary Region
```

You can monitor the replication status of protected items from the vault.

---

# Failover

**Failover** moves the workload recovery process to the secondary region when the primary environment is unavailable or when a planned disaster recovery test is performed.

Example:

```text
Primary Region
      │
      X
   Failure
      │
      ▼
Site Recovery
      │
      ▼
Secondary Region
      │
      ▼
Recovered VM
```

---

# Types of Failover

### Planned Failover

A planned failover is performed when you intentionally move workloads to the secondary location.

It can be used for:

- Planned maintenance
- Disaster recovery exercises
- Controlled migration scenarios

---

### Unplanned Failover

An unplanned failover is used when the primary environment is unexpectedly unavailable.

```text
Primary Region
      │
      X
   Disaster
      │
      ▼
Unplanned Failover
      │
      ▼
Secondary Region
```

---

### Test Failover

A **test failover** allows you to test the disaster recovery configuration without affecting the production workload.

```text
Production VM
     │
     ▼
Replicated VM
     │
     ▼
Test Failover
     │
     ▼
Test Environment
```

This is useful for validating:

- Replication
- Recovery configuration
- Network configuration
- Application availability
- Recovery procedures

---

# Failback

After the primary environment becomes available again, workloads can be moved back from the secondary environment.

```text
Primary Region
      │
      X
   Failure
      │
      ▼
Secondary Region
      │
    Failover
      │
      ▼
 Workload Running
      │
      │ Primary Recovered
      ▼
    Failback
      │
      ▼
Primary Region
```

**Failback** returns the workload to the original or preferred location after recovery.

---

# Recovery Plans

A **recovery plan** allows you to group and coordinate the recovery of multiple workloads.

For example:

```text
Recovery Plan
     │
     ├── Database VM
     │
     ├── Application VM
     │
     └── Web VM
```

You can define the order in which workloads should be recovered.

Example:

```text
Group 1
Database
   ↓
Group 2
Application
   ↓
Group 3
Web Server
```

This helps maintain application dependencies during recovery.

---

# Recovery Point Objective and Recovery Time Objective

### RPO — Recovery Point Objective

RPO defines the maximum acceptable amount of data loss measured in time.

Example:

```text
RPO = 15 minutes
```

This means the organization aims to recover with no more than approximately 15 minutes of data loss, depending on the actual replication and recovery configuration.

---

### RTO — Recovery Time Objective

RTO defines the target amount of time required to restore service after a disaster.

Example:

```text
Disaster
   ↓
Failover
   ↓
Workload Available

Target RTO = 1 hour
```

Simple difference:

```text
RPO → How much data can we afford to lose?

RTO → How quickly must the service be restored?
```

---

# Site Recovery Workflow

```text
1. Create Recovery Services Vault
              ↓
2. Select Primary Region
              ↓
3. Select Secondary Region
              ↓
4. Enable Replication
              ↓
5. Monitor Replication
              ↓
6. Perform Test Failover
              ↓
7. Perform Failover When Required
              ↓
8. Validate Workload
              ↓
9. Reprotect / Failback
```

---

# Practical Lab

## Lab — Configure Azure Site Recovery and Test Failover

### Objective

Configure replication for an Azure VM to another Azure region and perform a **test failover**.

### Architecture

```text
Primary Region
      │
      │ Site Recovery
      │ Replication
      ▼
Secondary Region
      │
      ▼
Test Failover
      │
      ▼
Recovered Test VM
```

### Steps

1. Create an Azure VM in the primary region.
2. Create a **Recovery Services vault**.
3. Open **Site Recovery** in the vault.
4. Select the Azure VM to protect.
5. Configure the target/secondary region.
6. Configure the required replication settings.
7. Enable replication.
8. Wait until the VM shows a healthy replication status.
9. Start a **Test Failover**.
10. Select the appropriate recovery point.
11. Configure the test network.
12. Start the test failover.
13. Verify that the recovered VM is created in the secondary region.
14. Verify connectivity and application availability.
15. Clean up the test failover resources.
16. Review the replication and failover status.

> **Note:** A test failover is designed to validate the recovery configuration without disrupting the primary production VM.

---

# Key Points

- **Azure Site Recovery** provides disaster recovery and business continuity.
- Site Recovery **replicates workloads** to a recovery location.
- A **Recovery Services vault** manages Site Recovery configuration.
- A protected workload is represented as a **replicated/protected item**.
- **Failover** recovers workloads in the secondary location.
- **Test failover** validates disaster recovery without affecting production.
- **Failback** moves workloads back after the primary environment is recovered.
- **Recovery plans** coordinate the recovery of multiple workloads.
- **RPO** defines acceptable data loss.
- **RTO** defines the target recovery time.
- Azure Backup focuses primarily on **backup and restore**, while Site Recovery focuses on **disaster recovery and failover**.

---

## Next

➡️ **16.5 — Backup Monitoring and Alerts**
