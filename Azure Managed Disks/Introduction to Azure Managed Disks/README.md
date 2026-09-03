# 9.1 Introduction to Azure Managed Disks

## What are Azure Managed Disks?

**Azure Managed Disks** are block-level storage volumes managed by Azure and used with Azure Virtual Machines.

Azure handles the underlying storage management, so you do not need to manage storage accounts for the disks.

```text
                Azure Virtual Machine
                        │
             ┌──────────┼──────────┐
             │          │          │
          OS Disk    Data Disk   Temporary Disk
             │          │          │
             └──────────┼──────────┘
                        │
                Azure Managed Disks
```

---

## Why Use Managed Disks?

Managed Disks provide:

- Simplified disk management
- High availability and durability
- Different performance options
- Easy disk resizing
- Snapshots and backup support
- Integration with Azure Virtual Machines
- Encryption at rest

---

## Types of VM Disks

Azure VMs can use different types of disks.

### 1. OS Disk

The **OS Disk** contains the operating system of the virtual machine.

Examples:

```text
Linux → Ubuntu / RHEL / Debian
Windows → Windows Server
```

The VM boots from the OS disk.

---

### 2. Data Disk

A **Data Disk** is used to store application data, databases, files, and other workloads.

Example:

```text
VM
 ├── OS Disk
 ├── Data Disk 1
 └── Data Disk 2
```

Data disks can be attached or detached from a VM when required.

---

### 3. Temporary Disk

The **Temporary Disk** provides temporary storage for the VM.

It can be used for:

- Temporary files
- Cache
- Paging or swap files

Important:

> Data stored on the temporary disk should not be considered persistent because the disk can be lost during certain VM operations or maintenance events.

---

# Managed vs Unmanaged Disks

Azure previously supported **unmanaged disks**, where VM disks were stored as VHD files inside a storage account.

Managed disks remove the need to manage those underlying storage accounts.

| Managed Disks | Unmanaged Disks |
|---|---|
| Managed by Azure | Customer manages storage account |
| Easier to manage | More management required |
| Recommended approach | Legacy approach |
| Azure handles underlying storage | VHDs stored in storage accounts |

---

# Managed Disk Types

Azure provides different managed disk types for different workloads.

## Standard HDD

**Standard HDD** provides economical disk storage for workloads that do not require high performance.

Suitable for:

- Development
- Testing
- Infrequently accessed workloads
- Cost-sensitive applications

---

## Standard SSD

**Standard SSD** provides better performance and lower latency than Standard HDD.

Suitable for:

- Web servers
- Lightweight applications
- Development and testing
- General-purpose workloads

---

## Premium SSD

**Premium SSD** provides higher performance and lower latency.

Suitable for:

- Production applications
- Business-critical workloads
- High-performance applications

---

## Premium SSD v2

**Premium SSD v2** provides configurable performance and is designed for workloads that require flexible IOPS and throughput.

Suitable for:

- High-performance applications
- Databases
- Workloads requiring independently configurable performance

---

## Ultra Disk

**Ultra Disk** provides very high IOPS and throughput with extremely low latency.

Suitable for:

- High-performance databases
- Transaction-intensive workloads
- Mission-critical applications

---

# Managed Disk Comparison

| Disk Type | Performance | Typical Use |
|---|---|---|
| Standard HDD | Low | Development, testing, backup |
| Standard SSD | Moderate | General workloads |
| Premium SSD | High | Production workloads |
| Premium SSD v2 | High and configurable | Performance-sensitive workloads |
| Ultra Disk | Very High | Mission-critical workloads |

---

# Disk Performance

Two important disk performance metrics are:

## IOPS

**IOPS (Input/Output Operations Per Second)** represents how many read/write operations a disk can perform per second.

```text
Higher IOPS
    ↓
More I/O operations per second
```

Workloads such as databases can require high IOPS.

---

## Throughput

**Throughput** represents the amount of data that can be transferred per second.

It is commonly measured in:

```text
MB/s
GB/s
```

Example:

```text
Higher Throughput
        ↓
More data transferred per second
```

---

# IOPS vs Throughput

| IOPS | Throughput |
|---|---|
| Number of I/O operations | Amount of data transferred |
| Important for many small operations | Important for large data transfers |
| Commonly important for databases | Commonly important for large sequential workloads |

---

# Choosing a Managed Disk

Choose the disk type based on:

- Workload requirements
- IOPS requirements
- Throughput requirements
- Latency requirements
- Capacity
- Cost

Example:

```text
Development VM
      ↓
Standard SSD

Production Application
      ↓
Premium SSD

High-performance Database
      ↓
Ultra Disk / Premium SSD v2
```

---

# Managed Disk Architecture

```text
                    Azure VM
                       │
        ┌──────────────┼──────────────┐
        │              │              │
     OS Disk       Data Disk     Temporary Disk
        │              │              │
        └──────────────┼──────────────┘
                       │
                Managed by Azure
```

---

# Key Points

- Azure Managed Disks are block-level storage used with Azure VMs.
- Azure manages the underlying storage infrastructure.
- OS disks contain the operating system.
- Data disks store application and user data.
- Temporary disks provide temporary storage.
- Azure supports Standard HDD, Standard SSD, Premium SSD, Premium SSD v2, and Ultra Disk.
- IOPS measures the number of I/O operations per second.
- Throughput measures the amount of data transferred per second.
- Disk type should be selected based on workload, performance, and cost requirements.
- Managed Disks are the recommended approach for Azure VM disk management.
