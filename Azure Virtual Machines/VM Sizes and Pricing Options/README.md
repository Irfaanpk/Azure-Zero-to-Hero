# 8.3 VM Sizes and Pricing Options

## What is a VM Size?

An Azure VM size defines the amount of compute resources available to a virtual machine.

A VM size determines:

- Number of vCPUs
- Amount of RAM
- Maximum data disks
- Network performance
- Temporary storage
- Overall VM performance

---

## VM Size Families

Azure provides different VM families for different workloads.

| VM Family | Purpose |
|---|---|
| General Purpose | Balanced CPU, memory, and networking |
| Compute Optimized | Higher CPU performance |
| Memory Optimized | Higher RAM for memory-intensive workloads |
| Storage Optimized | High disk throughput and IOPS |
| GPU | Graphics and AI/ML workloads |

For most general workloads, **General Purpose** VMs are commonly used.

---

## Choosing a VM Size

When selecting a VM size, consider:

- CPU requirements
- Memory requirements
- Network requirements
- Storage requirements
- Expected workload
- Cost

Example:

```text
Small web server
    ↓
General Purpose VM
    ↓
Low CPU + Low RAM requirement
    ↓
Choose a smaller VM size
