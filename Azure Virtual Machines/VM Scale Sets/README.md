# 8.11 VM Scale Sets

## What are Virtual Machine Scale Sets?

**Azure Virtual Machine Scale Sets (VMSS)** allow you to create and manage a group of Azure virtual machines as a single resource.

Instead of creating and managing each VM separately, VMSS provides a common configuration for multiple VM instances.

```text
                    VM Scale Set
                         │
             ┌───────────┼───────────┐
             │           │           │
           VM-1        VM-2        VM-3
```

VM Scale Sets are useful for applications that require:

- Multiple VM instances
- High availability
- Scalability
- Consistent VM configuration
- Centralized VM management

---

# Why Use VM Scale Sets?

Without VMSS, each VM needs to be managed separately.

```text
VM-1 → Manage separately
VM-2 → Manage separately
VM-3 → Manage separately
```

With VMSS:

```text
                VM Scale Set
                     │
        ┌────────────┼────────────┐
        │            │            │
      VM-1         VM-2         VM-3
```

VMSS allows you to manage multiple VM instances as a group.

---

# VMSS Architecture

A basic VMSS architecture looks like:

```text
                    VM Scale Set
                         │
             ┌───────────┼───────────┐
             │           │           │
           VM-1        VM-2        VM-3
             │           │           │
             └───────────┼───────────┘
                         │
                    Application
```

The VM instances are created from the VMSS configuration.

---

# VMSS Instances

Each virtual machine inside a VMSS is called an **instance**.

Example:

```text
VM Scale Set
    │
    ├── Instance 1
    ├── Instance 2
    ├── Instance 3
    └── Instance 4
```

You can increase or decrease the number of instances depending on application requirements.

---

# VMSS Orchestration Modes

Azure VM Scale Sets support two orchestration modes:

- **Uniform orchestration**
- **Flexible orchestration**

---

## Uniform Orchestration

Uniform orchestration is designed for groups of identical VM instances.

The instances are based on a common VMSS model.

```text
                VMSS Model
                    │
          ┌─────────┼─────────┐
          │         │         │
        VM-1      VM-2      VM-3
```

It is suitable when multiple instances need a consistent configuration.

---

## Flexible Orchestration

Flexible orchestration provides VMSS capabilities while allowing more flexibility in managing VM instances.

```text
                VM Scale Set
                     │
          ┌──────────┼──────────┐
          │          │          │
        VM-1       VM-2       VM-3
```

It is useful when you need VMSS features while retaining more flexibility over individual VM instances.

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

VMSS allows you to manage individual instances within the scale set.

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

# Manual Scaling

Manual scaling allows you to change the number of VMSS instances manually.

Example:

```text
Current Capacity: 2

        ↓

New Capacity: 4
```

Azure creates additional VM instances.

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

---

# Horizontal Scaling

**Horizontal scaling** means increasing or decreasing the number of VM instances.

### Scale Out

Add more VM instances.

```text
2 VM Instances
       ↓
4 VM Instances
```

### Scale In

Remove VM instances.

```text
4 VM Instances
       ↓
2 VM Instances
```

VMSS is commonly used for horizontal scaling.

---

# VMSS and Availability Zones

VM Scale Sets can be deployed across **Availability Zones** in supported Azure regions.

This helps improve resilience against zone-level failures.

```text
                 VM Scale Set
                      │
          ┌───────────┼───────────┐
          │           │           │
       Zone 1       Zone 2       Zone 3
          │           │           │
        VM-1        VM-2        VM-3
```

If one zone becomes unavailable, instances in other zones can continue serving the application.

---

# VMSS and Azure Load Balancer

VM Scale Sets can integrate with **Azure Load Balancer** to distribute traffic across VMSS instances.

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

The Load Balancer can distribute traffic across healthy VMSS instances.

Detailed Load Balancer configuration is covered in **Section 8.9**.

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

- Layer 7 load balancing
- HTTP/HTTPS routing
- URL-based routing
- Host-based routing

Application Gateway is covered in **Section 8.10**.

---

# VMSS Autoscaling

VMSS can automatically change the number of instances based on workload conditions.

```text
                  VM Scale Set
                       │
                  Autoscaling
                 /           \
                ↓             ↓
           Scale Out       Scale In
```

### Scale Out

When workload increases:

```text
High CPU / High Workload
          ↓
       Scale Out
          ↓
   Add VM Instances
```

### Scale In

When workload decreases:

```text
Low CPU / Low Workload
          ↓
        Scale In
          ↓
  Remove VM Instances
```

---

# Autoscaling Limits

Autoscaling can be configured with:

- Minimum instance count
- Default instance count
- Maximum instance count

Example:

```text
Minimum: 2
Default: 2
Maximum: 5
```

This means the VMSS can scale between:

```text
2 ←──── VM Instances ────→ 5
```

---

# Metric-Based Autoscaling

Autoscaling can use metrics to determine when to add or remove instances.

A common metric is:

```text
Percentage CPU
```

Example:

```text
Average CPU > 70%
        ↓
     Scale Out
```

```text
Average CPU < 30%
        ↓
      Scale In
```

Azure Monitor provides the metrics used by autoscale.

---

# Scheduled Autoscaling

VMSS can also scale based on a predefined schedule.

This is useful when workload patterns are predictable.

Example:

```text
Business Hours
     ↓
Increase VM Instances

Night Time
     ↓
Decrease VM Instances
```

Example:

```text
9:00 AM
   ↓
Scale to 4 instances

6:00 PM
   ↓
Scale to 2 instances
```

---

# Predictive Autoscaling

**Predictive autoscale** uses historical workload patterns to predict future demand and scale VM instances ahead of expected demand.

Example:

```text
Historical Workload Data
          ↓
   Predict Future Demand
          ↓
    Scale Before Demand
          ↓
    VMSS Ready for Load
```

This can help applications prepare for predictable increases in workload.

> Predictive autoscaling is covered conceptually here. The complete autoscaling implementation is demonstrated in **Section 8.12**.

---

# VMSS Use Cases

VM Scale Sets are commonly used for:

- Web applications
- Application servers
- APIs
- Microservices
- High-traffic applications
- Distributed applications
- Scalable workloads
- Highly available applications

---

# VMSS vs Azure VM

| Azure VM | VM Scale Set |
|---|---|
| Individual virtual machine | Group of virtual machines |
| Managed individually | Managed as a group |
| Suitable for single-instance workloads | Suitable for multiple-instance workloads |
| Manual scaling | Supports manual and automatic scaling |
| One VM instance | Multiple VM instances |

---

# VMSS vs Availability Set

These concepts solve different problems.

| VM Scale Set | Availability Set |
|---|---|
| Manages multiple VM instances | Controls VM placement for availability |
| Supports scaling | Does not provide autoscaling |
| Designed for scalable workloads | Designed to reduce failure impact |
| Can integrate with Load Balancer | Can integrate with Load Balancer |
| Creates and manages VM instances as a set | Groups VMs for availability |

---

# Practical Lab

## Lab: Create and Manage a VM Scale Set

### Objective

Create a VM Scale Set with multiple VM instances and practice basic instance management and manual scaling.

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

Wait for the deployment to complete.

---

### Step 9: Verify Instances

Open the VM Scale Set.

Select **Instances**.

Verify:

```text
Instance 1 → Running
Instance 2 → Running
```

---

### Step 10: Test Manual Scale Out

Increase the instance count:

```text
2 → 3
```

Verify that a new instance is created.

```text
VMSS
 ├── VM-1
 ├── VM-2
 └── VM-3
```

---

### Step 11: Test Manual Scale In

Reduce the instance count:

```text
3 → 2
```

Verify that the VMSS instance count decreases.

---

### Step 12: Explore Instance Management

Open the VMSS instance list and explore available operations such as:

- Start
- Stop
- Restart
- Reimage
- Delete
- View instance details

Do not delete an instance unless you want to test the operation.

---

# Key Points

- VM Scale Sets manage multiple Azure VMs as a group.
- Each VM inside a VMSS is called an instance.
- VMSS supports Uniform and Flexible orchestration.
- VMSS provides centralized management of VM instances.
- Manual scaling changes the number of VMSS instances.
- Horizontal scaling adds or removes VM instances.
- VMSS can be deployed across Availability Zones.
- VMSS can integrate with Azure Load Balancer.
- VMSS can be used as a backend for Application Gateway.
- VMSS supports autoscaling based on workload conditions.
- Autoscaling can use metric-based, scheduled, and predictive scaling.
- Minimum, default, and maximum instance counts control autoscaling boundaries.
- Detailed VMSS load balancing, autoscaling configuration, monitoring, alerts, Action Groups, and email notification are covered in **Section 8.12**.
