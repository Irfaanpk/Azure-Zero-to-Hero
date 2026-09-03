# 8.3 VM Sizes and Pricing Options

## What is a VM Size?

An Azure VM size defines the compute resources available to a virtual machine.

The selected VM size determines:

- Number of vCPUs
- Amount of RAM
- Temporary storage
- Maximum number of data disks
- Network bandwidth and performance
- Overall VM performance

Example:

```text
VM Size
   │
   ├── vCPUs
   ├── RAM
   ├── Temporary Storage
   ├── Data Disk Support
   └── Network Performance
```

---

## Why VM Size Matters

Choosing the correct VM size is important because different workloads require different amounts of CPU, memory, storage, and network performance.

For example:

```text
Small Web Server
      ↓
Low CPU + Low Memory
      ↓
Smaller VM Size
```

Whereas:

```text
Memory-Intensive Application
      ↓
High RAM Requirement
      ↓
Memory Optimized VM
```

Choosing a VM that is too small can cause poor performance.

Choosing a VM that is unnecessarily large can increase costs.

---

# VM Size Families

Azure provides different VM size families for different workloads.

| VM Family | Main Purpose |
|---|---|
| General Purpose | Balanced CPU, memory, and networking |
| Compute Optimized | CPU-intensive workloads |
| Memory Optimized | Memory-intensive workloads |
| Storage Optimized | High disk throughput and IOPS |
| GPU | Graphics, AI, and compute-intensive workloads |

---

## General Purpose

General-purpose VMs provide a balanced combination of:

- CPU
- Memory
- Network performance

They are commonly used for:

- Web servers
- Application servers
- Development environments
- Testing environments
- Small databases

Example:

```text
General Purpose
      ↓
Balanced CPU + RAM
      ↓
Web / Application Workloads
```

---

## Compute Optimized

Compute-optimized VMs provide a higher CPU-to-memory ratio.

They are suitable for workloads that require more CPU processing power.

Examples:

- Batch processing
- Application servers
- High-performance web servers
- CPU-intensive applications

```text
High CPU Requirement
        ↓
Compute Optimized VM
```

---

## Memory Optimized

Memory-optimized VMs provide a higher amount of RAM compared with CPU resources.

They are suitable for:

- Large databases
- In-memory applications
- Data processing
- Caching workloads

```text
High RAM Requirement
        ↓
Memory Optimized VM
```

---

## Storage Optimized

Storage-optimized VMs are designed for workloads requiring high disk performance.

They are suitable for workloads that require:

- High IOPS
- High disk throughput
- Large amounts of local storage

Examples:

- Data processing
- Large-scale transactional workloads
- Storage-intensive applications

---

## GPU VMs

GPU-enabled VMs provide GPU resources in addition to normal CPU and memory resources.

They can be used for:

- Machine learning
- AI workloads
- Graphics processing
- Video rendering
- High-performance computing

---

# VM Size Naming

Azure VM sizes use naming conventions to indicate characteristics of the VM.

Example:

```text
D4s_v5
```

The exact meaning of each part depends on the VM series and generation.

When selecting a VM size in the Azure Portal, Azure displays the available specifications, so you should compare the actual resources rather than memorizing every naming convention.

---

# Important VM Size Specifications

When comparing VM sizes, look at:

### vCPUs

Number of virtual CPUs available to the VM.

```text
2 vCPUs
4 vCPUs
8 vCPUs
...
```

More vCPUs generally provide greater CPU processing capacity.

---

### RAM

Amount of memory available to applications.

```text
4 GB
8 GB
16 GB
32 GB
...
```

Memory requirements depend on the workload.

---

### Temporary Storage

Some VM sizes provide local temporary storage.

Temporary storage is intended for temporary data and should not be treated as persistent storage.

Persistent VM disks are covered separately in the **Managed Disks** section.

---

### Network Performance

VM sizes can have different network performance capabilities.

Higher-end VM sizes generally provide greater network throughput.

---

### Maximum Data Disks

Different VM sizes support different numbers of attached data disks.

This is important when applications require multiple disks.

---

# Choosing the Right VM Size

Consider the following before selecting a VM size:

```text
Workload
   ↓
CPU Requirement
   ↓
Memory Requirement
   ↓
Storage Requirement
   ↓
Network Requirement
   ↓
Cost
   ↓
Select VM Size
```

### Example 1 — Web Server

```text
Requirement:
Low to moderate CPU
Low to moderate RAM
Normal network traffic

        ↓

General Purpose VM
```

### Example 2 — CPU-Intensive Application

```text
Requirement:
High CPU usage

        ↓

Compute Optimized VM
```

### Example 3 — Database

```text
Requirement:
High memory
High disk performance

        ↓

Memory / Storage Optimized VM
```

---

# Resizing a VM

You can change the VM size when the current size does not meet the workload requirements.

Example:

```text
Current VM

2 vCPUs
8 GB RAM

        ↓
      Resize

New VM

4 vCPUs
16 GB RAM
```

Resizing can be used to:

- Increase CPU capacity
- Increase memory
- Improve network performance
- Support a larger workload
- Reduce resources when a VM is oversized

> The new VM size must be available in the VM's region and may require the VM to be stopped or deallocated.

---

# VM Pricing

Azure VM pricing depends on several factors, including:

- VM size
- Region
- Operating system
- Pricing model
- Usage duration

Azure provides different pricing options for virtual machines.

---

# Pay-as-you-go

**Pay-as-you-go** allows you to use Azure resources without a long-term commitment.

You pay based on usage.

Suitable for:

- Learning
- Development
- Testing
- Temporary workloads
- Unpredictable workloads

Example:

```text
Create VM
   ↓
Use VM
   ↓
Pay for usage
```

---

# Azure Reservations

Azure Reservations allow you to commit to eligible Azure resources for a fixed term in exchange for discounted pricing.

Suitable for:

- Predictable workloads
- Long-running VMs
- Production workloads with stable requirements

Example:

```text
Predictable VM Usage
        ↓
Long-term Commitment
        ↓
Potential Cost Savings
```

---

# Azure Savings Plan for Compute

Azure Savings Plan for Compute provides discounted rates when you commit to a certain amount of eligible compute spending over a fixed term.

Unlike a reservation tied to a specific VM resource, a savings plan provides more flexibility across eligible compute usage.

Suitable for:

- Consistent compute usage
- Workloads where VM requirements may change
- Organizations wanting commitment-based savings with flexibility

---

# Spot VMs

Azure Spot VMs use unused Azure capacity at significantly reduced prices.

However, Azure can evict a Spot VM when Azure needs the capacity.

Therefore, Spot VMs should be used only for workloads that can tolerate interruption.

Suitable examples:

- Batch processing
- Development and testing
- Non-critical workloads
- Fault-tolerant applications

```text
Unused Azure Capacity
        ↓
     Spot VM
        ↓
Low Cost
        ↓
Possible Eviction
```

---

# Pricing Options Comparison

| Pricing Option | Commitment | Cost | Suitable For |
|---|---|---|---|
| Pay-as-you-go | None | Standard | Flexible workloads |
| Reservations | Fixed-term | Discounted | Predictable workloads |
| Savings Plan for Compute | Compute spending commitment | Discounted | Flexible predictable compute |
| Spot VM | No traditional long-term commitment | Highly discounted | Interruptible workloads |

---

# VM Cost Components

The VM compute price is not the only cost associated with a VM.

A VM deployment can also use other Azure resources that may incur charges.

Examples:

- Managed Disks
- Public IP addresses
- Network services
- Load Balancers
- Other attached Azure resources

```text
Azure VM
   │
   ├── Compute Cost
   ├── Managed Disk Cost
   ├── Public IP Cost
   └── Other Resource Costs
```

> Managed Disk concepts are covered separately in the next section.

---

# VM Size vs Pricing

VM size and pricing are closely related.

Generally:

```text
More Resources
      ↓
Higher VM Cost
```

Therefore, avoid selecting a larger VM size than required.

A good approach is:

```text
Start with suitable size
        ↓
Monitor workload
        ↓
Evaluate performance
        ↓
Resize if required
```

---

# Practical Lab

## Lab: Explore VM Sizes and Pricing

### Objective

Explore different Azure VM sizes and understand how VM specifications affect pricing.

### Steps

1. Sign in to the **Azure Portal**.
2. Open **Virtual Machines**.
3. Select **Create → Azure virtual machine**.
4. Select your subscription and resource group.
5. Enter a VM name.
6. Select the required region.
7. Choose a Linux or Windows image.
8. Open the **Size** section.
9. Explore the available VM sizes.
10. Compare:
    - vCPUs
    - RAM
    - Temporary storage
    - Maximum data disks
    - Network performance
    - Estimated pricing
11. Select a suitable VM size.
12. Review the pricing information.
13. You can cancel the deployment if you only want to explore the available sizes and pricing.

---

# Key Points

- VM size determines the resources available to an Azure VM.
- VM sizes include vCPUs, RAM, storage capabilities, and network performance.
- Azure provides different VM families for different workloads.
- General Purpose VMs provide balanced resources.
- Compute Optimized VMs are designed for CPU-intensive workloads.
- Memory Optimized VMs are designed for memory-intensive workloads.
- Storage Optimized VMs are designed for storage-intensive workloads.
- GPU VMs provide GPU resources for specialized workloads.
- VMs can be resized when more or fewer resources are required.
- Pay-as-you-go provides flexibility without long-term commitment.
- Reservations provide discounted pricing through a fixed-term commitment.
- Savings Plan for Compute provides discounted compute through a spending commitment.
- Spot VMs provide low-cost compute but can be evicted.
- VM compute cost is separate from costs for resources such as managed disks and networking resources.
