# 8.11 VM Scale Sets

## What are Virtual Machine Scale Sets?

**Azure Virtual Machine Scale Sets (VMSS)** allow you to create and manage a group of load-balanced virtual machines.

Instead of managing each VM individually, VMSS allows you to manage multiple VM instances as a single resource.

```text
                    VM Scale Set
                         │
             ┌───────────┼───────────┐
             │           │           │
           VM-1        VM-2        VM-3
```

VM Scale Sets are useful for applications that need:

- Multiple VM instances
- High availability
- Automatic scaling
- Consistent VM configuration
- Centralized VM management

---

# Why Use VM Scale Sets?

Without VMSS:

```text
VM-1
VM-2
VM-3
VM-4
```

Each VM may need to be managed separately.

With VMSS:

```text
                VM Scale Set
                     │
        ┌────────────┼────────────┐
        │            │            │
      VM-1         VM-2         VM-3
```

The instances are managed as part of the same scale set.

---

# VMSS Architecture

A basic VMSS architecture looks like:

```text
                       VM Scale Set
                            │
              ┌─────────────┼─────────────┐
              │             │             │
            VM-1          VM-2          VM-3
              │             │             │
              └─────────────┼─────────────┘
                            │
                       Application
```

VMSS instances are created from a common configuration.

---

# VMSS Instances

Each VM inside a scale set is called an **instance**.

Example:

```text
VM Scale Set
    │
    ├── Instance 1
    ├── Instance 2
    ├── Instance 3
    └── Instance 4
```

You can increase or decrease the number of instances depending on workload requirements.

---

# VMSS Orchestration Modes

Azure VM Scale Sets support different orchestration approaches.

For AZ-104, the important concept is **Flexible orchestration mode**.

### Flexible Orchestration

Flexible orchestration provides VMSS capabilities while allowing more flexibility in managing VM instances.

```text
VM Scale Set
      │
      ├── VM-1
      ├── VM-2
      └── VM-3
```

The scale set manages the group while individual VM instances can be managed more flexibly.

---

# VMSS Scaling

VMSS can increase or decrease the number of VM instances.

### Scale Out

Increase the number of VM instances.

```text
Before:

VMSS
 ├── VM-1
 └── VM-2

      ↓ Scale Out

VMSS
 ├── VM-1
 ├── VM-2
 ├── VM-3
 └── VM-4
```

Scale out is useful when workload increases.

---

### Scale In

Decrease the number of VM instances.

```text
Before:

VMSS
 ├── VM-1
 ├── VM-2
 ├── VM-3
 └── VM-4

      ↓ Scale In

VMSS
 ├── VM-1
 └── VM-2
```

Scale in is useful when workload decreases.

---

# Manual Scaling

You can manually change the number of instances in a VM Scale Set.

Example:

```text
Current Capacity: 2

        ↓

New Capacity: 4
```

Azure creates additional VM instances.

---

# Autoscaling

VMSS can also automatically change the number of instances based on defined conditions.

For example:

```text
CPU > 70%
    ↓
Scale Out
    ↓
Add VM Instances
```

And:

```text
CPU < 30%
    ↓
Scale In
    ↓
Remove VM Instances
```

Autoscaling is covered in detail in:

**Section 8.12 — VMSS Load Balancing & Autoscaling**

---

# VMSS and Load Balancer

VM Scale Sets can work with **Azure Load Balancer** to distribute traffic across VM instances.

```text
                     Internet
                        │
                        ▼
                Azure Load Balancer
                        │
                        ▼
                   VM Scale Set
                        │
             ┌──────────┼──────────┐
             │          │          │
           VM-1       VM-2       VM-3
```

The Load Balancer sends traffic to healthy VM instances.

---

# VMSS and Application Gateway

VM Scale Sets can also be used as backend targets for **Azure Application Gateway**.

```text
                     Internet
                        │
                        ▼
                Application Gateway
                        │
                        ▼
                   VM Scale Set
                        │
             ┌──────────┼──────────┐
             │          │          │
           VM-1       VM-2       VM-3
```

Application Gateway can provide:

- Layer 7 routing
- HTTP/HTTPS traffic management
- Health probes
- URL-based routing
- Host-based routing

---

# VMSS and Availability

VMSS can provide multiple VM instances for applications that require high availability.

Instead of:

```text
Application
    │
    ▼
  VM-1
```

You can use:

```text
Application
    │
    ▼
 VM Scale Set
    │
 ┌──┼──┐
VM-1 VM-2 VM-3
```

If one instance becomes unavailable, other instances can continue serving the application.

---

# VMSS Configuration

When creating a VM Scale Set, common configuration options include:

- Subscription
- Resource group
- VMSS name
- Region
- Orchestration mode
- VM image
- VM size
- Authentication
- Instance count
- Networking

Example:

```text
VMSS
 │
 ├── Image
 ├── VM Size
 ├── Instance Count
 ├── Authentication
 └── Networking
```

---

# VMSS Instance Management

You can manage individual VMSS instances.

Common operations include:

- Start
- Stop
- Restart
- Delete
- Reimage
- Redeploy
- View instance details

Example:

```text
VM Scale Set
    │
    ├── Instance 1 → Running
    ├── Instance 2 → Running
    └── Instance 3 → Stopped
```

---

# VMSS Model and Instances

A VM Scale Set maintains a common configuration model.

```text
VMSS Model
    │
    ├── Image
    ├── VM Size
    ├── OS Configuration
    └── Network Configuration
          │
          ▼
    VM Instances
```

When configuration changes are made, you need to understand how those changes are applied to existing instances and future instances.

---

# VMSS Use Cases

VM Scale Sets are commonly used for:

- Web applications
- Application servers
- APIs
- Microservices
- Distributed applications
- High-traffic applications
- Workloads requiring automatic scaling

---

# VM vs VMSS

| Azure VM | VM Scale Set |
|---|---|
| Individual virtual machine | Group of virtual machines |
| Managed individually | Managed as a group |
| Manual scaling | Supports manual and automatic scaling |
| Suitable for single-instance workloads | Suitable for scalable workloads |
| One VM instance | Multiple VM instances |

---

# VMSS vs Availability Set

These concepts are different.

| VM Scale Set | Availability Set |
|---|---|
| Manages a group of VMs | Provides VM placement for availability |
| Supports scaling | Does not provide autoscaling |
| Designed for scalable workloads | Designed to reduce failure impact |
| Can integrate with Load Balancer | Can integrate with Load Balancer |
| Supports multiple VM instances | Groups existing VMs |

---

# Practical Lab

## Lab: Create a Virtual Machine Scale Set

### Objective

Create a VM Scale Set with multiple VM instances and explore instance management.

### Step 1: Open Azure Portal

1. Sign in to the **Azure Portal**.
2. Search for **Virtual Machine Scale Sets**.
3. Select **Create**.

---

### Step 2: Configure Basics

Configure:

```text
Subscription
Resource Group
VMSS Name
Region
Orchestration Mode
```

Select the appropriate orchestration mode for the deployment.

---

### Step 3: Select VM Image

Choose an operating system image.

Example:

```text
Ubuntu
```

---

### Step 4: Select VM Size

Choose a suitable VM size.

Example:

```text
General Purpose VM
```

---

### Step 5: Configure Authentication

For Linux:

```text
SSH Public Key
```

For Windows:

```text
Username + Password
```

---

### Step 6: Configure Instance Count

Set the initial number of instances.

Example:

```text
Instance Count: 2
```

---

### Step 7: Configure Networking

Select or create:

- Virtual Network
- Subnet
- Network configuration

---

### Step 8: Create the VMSS

Review the configuration and select **Create**.

Azure creates the VM Scale Set and its VM instances.

---

### Step 9: Verify Instances

Open the VM Scale Set and check:

```text
Instances

VMSS-Instance-1
VMSS-Instance-2
```

---

### Step 10: Change Instance Count

Increase the instance count:

```text
2 → 3
```

Verify that another VM instance is created.

Then reduce:

```text
3 → 2
```

Verify that the instance count decreases.

---

# Key Points

- VM Scale Sets manage a group of Azure VMs.
- VMSS is useful for scalable and highly available applications.
- Individual VMs inside a VMSS are called instances.
- VMSS supports manual scaling and autoscaling.
- **Scale out** increases the number of instances.
- **Scale in** decreases the number of instances.
- VMSS can work with Azure Load Balancer.
- VMSS can work with Azure Application Gateway.
- VMSS provides centralized management of multiple VM instances.
- Flexible orchestration provides greater flexibility for VM instance management.
- Autoscaling, Load Balancer integration, alerts, and Action Groups are covered in **Section 8.12**.
