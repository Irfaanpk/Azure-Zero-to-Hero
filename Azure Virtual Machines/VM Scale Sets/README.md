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

VMSS allows you to manage the instances as part of one scale set.

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

All instances are created and managed through the VM Scale Set.

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

You can increase or decrease the number of instances depending on the workload.

---

# VMSS Orchestration Modes

Azure VM Scale Sets support different orchestration modes.

The main modes are:

- Flexible orchestration
- Uniform orchestration

---

## Flexible Orchestration

**Flexible orchestration** provides VMSS capabilities while allowing more flexibility in managing VM instances.

It is useful when you need:

- VMSS management
- Scaling
- Load balancing
- Availability
- More flexibility in VM configuration

```text
VM Scale Set
      │
      ├── VM-1
      ├── VM-2
      └── VM-3
```

---

## Uniform Orchestration

**Uniform orchestration** is designed for a group of identical VM instances.

The instances are based on a common VMSS model.

```text
VMSS Model
    │
    ├── VM-1
    ├── VM-2
    ├── VM-3
    └── VM-4
```

Uniform orchestration is useful when instances need a consistent configuration.

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

# VMSS Model

The VMSS maintains a configuration model that defines the configuration used by its instances.

Example:

```text
VMSS Model
    │
    ├── VM Image
    ├── VM Size
    ├── OS Configuration
    ├── Authentication
    └── Network Configuration
             │
             ▼
       VM Instances
```

The model helps maintain consistent configuration across instances.

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

# Manual Scaling

Manual scaling allows you to change the number of VMSS instances yourself.

Example:

```text
Current Capacity: 2

        ↓

Change Capacity: 4
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

Manual scaling is useful when you know that more or fewer instances are required.

---

# Horizontal Scaling

**Horizontal scaling** means increasing or decreasing the number of VM instances.

### Scale Out

Add more VM instances.

```text
2 VMs
 ↓
4 VMs
```

### Scale In

Remove VM instances.

```text
4 VMs
 ↓
2 VMs
```

VMSS is primarily designed to support horizontal scaling.

---

# Vertical Scaling

**Vertical scaling** means changing the resources of an individual VM by changing its VM size.

Example:

```text
Before:

2 vCPUs
8 GB RAM

      ↓ Resize

After:

4 vCPUs
16 GB RAM
```

Vertical scaling increases the capacity of individual VM instances.

```text
Horizontal Scaling
       ↓
Add more VMs

Vertical Scaling
       ↓
Increase VM resources
```

VM sizing and resizing are covered in more detail in **Section 8.3 — VM Sizes and Pricing Options**.

---

# VMSS and Availability Zones

VM Scale Sets can be deployed across **Availability Zones** in supported Azure regions.

This can improve resilience against zone-level failures.

Example:

```text
                 VM Scale Set
                      │
          ┌───────────┼───────────┐
          │           │           │
       Zone 1       Zone 2       Zone 3
          │           │           │
        VM-1        VM-2        VM-3
```

If one availability zone becomes unavailable, instances in other zones can continue serving the workload.

---

# VMSS and Fault Domains

VMSS instances can also be distributed across fault domains depending on the orchestration and deployment configuration.

Fault domains help reduce the impact of infrastructure failures affecting shared physical resources.

```text
VM Scale Set

Fault Domain 0       Fault Domain 1
     │                    │
   VM-1                 VM-2
```

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

The Load Balancer can use health probes to determine which VMSS instances are healthy.

Detailed load balancing and autoscaling are covered in **Section 8.12 — VMSS Load Balancing & Autoscaling**.

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
- Health probes

Application Gateway is covered in **Section 8.10**.

---

# VMSS Availability with Load Balancer

A common highly available architecture is:

```text
                         Internet
                            │
                            ▼
                  Azure Load Balancer
                            │
                            ▼
                       VM Scale Set
                 ┌──────────┼──────────┐
                 │          │          │
               VM-1       VM-2       VM-3
```

If one instance becomes unavailable:

```text
VM-1 → Unhealthy
VM-2 → Healthy
VM-3 → Healthy
```

The Load Balancer can continue sending traffic to the healthy instances.

---

# VMSS Scaling vs Availability

These concepts solve different problems.

### Scaling

Handles changes in workload.

```text
High Workload
     ↓
More VM Instances
```

### Availability

Protects the application from infrastructure failures.

```text
VM-1 Failure
     ↓
VM-2 + VM-3 Continue
```

VMSS can provide both multiple instances and scaling capabilities.

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
| Individual VM | Group of VMs |
| Managed individually | Managed as a group |
| Suitable for single-instance workloads | Suitable for multiple-instance workloads |
| Manual scaling | Supports scaling |
| One VM instance | Multiple VM instances |

---

# VMSS vs Availability Set

These concepts are different.

| VM Scale Set | Availability Set |
|---|---|
| Manages a group of VMs | Controls VM placement for availability |
| Supports scaling | Does not provide autoscaling |
| Designed for scalable workloads | Designed to reduce failure impact |
| Can integrate with Load Balancer | Can integrate with Load Balancer |
| Creates/manages VM instances as a set | Groups existing VMs |

---

# VMSS vs Availability Zones

| VMSS | Availability Zones |
|---|---|
| VM management and scaling technology | Physical availability boundary |
| Manages multiple VM instances | Provides separate data centers |
| Supports scaling | Provides zone-level resiliency |
| Can deploy instances across zones | Can be used with VMSS |

---

# Practical Lab

## Lab: Create and Manage a VM Scale Set

### Objective

Create a VM Scale Set with multiple VM instances and practice basic instance management and scaling.

---

## Step 1: Open Azure Portal

1. Sign in to the **Azure Portal**.
2. Search for **Virtual Machine Scale Sets**.
3. Select **Create**.

---

## Step 2: Configure Basics

Configure:

```text
Subscription
Resource Group
VMSS Name
Region
Orchestration Mode
```

---

## Step 3: Select VM Image

Choose an operating system image.

Example:

```text
Ubuntu
```

---

## Step 4: Select VM Size

Choose a suitable VM size.

Example:

```text
General Purpose VM
```

---

## Step 5: Configure Authentication

For Linux:

```text
SSH Public Key
```

For Windows:

```text
Username + Password
```

---

## Step 6: Configure Instance Count

Set the initial number of instances.

Example:

```text
Instance Count: 2
```

---

## Step 7: Configure Networking

Select or create:

- Virtual Network
- Subnet
- Network configuration

---

## Step 8: Create the VMSS

Review the configuration and select **Create**.

Wait for the deployment to complete.

---

## Step 9: Verify Instances

Open the VM Scale Set.

Select **Instances**.

Verify:

```text
Instance 1 → Running
Instance 2 → Running
```

---

## Step 10: Test Manual Scale Out

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

## Step 11: Test Manual Scale In

Reduce the instance count:

```text
3 → 2
```

Verify that the VMSS instance count decreases.

---

## Step 12: Explore Instance Management

Open the VMSS instance list and explore available operations such as:

- Restart
- Stop
- Start
- Reimage
- Delete

Do not delete an instance unless you want to test the operation.

---

# Key Points

- VM Scale Sets manage multiple Azure VMs as a group.
- Each VM inside a VMSS is called an instance.
- VMSS provides centralized management of VM instances.
- Azure supports Flexible and Uniform orchestration modes.
- Flexible orchestration provides greater flexibility for VM instances.
- Uniform orchestration is designed for consistent VM instances based on a common model.
- VMSS maintains a model that defines the configuration of its instances.
- Manual scaling changes the number of VMSS instances.
- **Horizontal scaling** adds or removes VM instances.
- **Vertical scaling** increases or decreases the resources of individual VM instances.
- VMSS can use Availability Zones for improved resilience.
- VMSS can integrate with Azure Load Balancer.
- VMSS can be used as a backend for Application Gateway.
- VMSS is suitable for scalable and highly available applications.
- Detailed **Load Balancing, Autoscaling, Azure Monitor alerts, Action Groups, and email notification** are covered in **Section 8.12**.
