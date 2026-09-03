# 8.5 VM Availability and Placement

## Why VM Availability Matters

A single Azure Virtual Machine can become unavailable because of:

- Hardware failures
- Planned Azure maintenance
- Power failures
- Network issues
- Data center problems

For production workloads, running only one VM creates a **single point of failure**.

Azure provides several options to improve VM availability and control where VMs are placed.

```text
Single VM
   │
   ▼
Single Point of Failure
```

To improve availability:

```text
Multiple VMs
      │
      ├── Availability Set
      │
      └── Availability Zones
```

---

# Availability Options in Azure

The main VM availability and placement options are:

| Option | Purpose |
|---|---|
| Availability Set | Protect VMs from host and maintenance failures within a data center |
| Availability Zone | Protect workloads from data center failures within a region |
| Azure Dedicated Host | Provides dedicated physical servers for your organization |

---

# Availability Set

An **Availability Set** is a logical grouping of VMs that distributes them across different Azure infrastructure components.

It helps protect against:

- Hardware failures
- Planned maintenance

An Availability Set uses:

- Fault Domains
- Update Domains

```text
Availability Set
       │
 ┌─────┴─────┐
 │           │
VM-1       VM-2
 │           │
Different infrastructure placement
```

---

# Fault Domains

A **Fault Domain** represents a group of Azure infrastructure that shares common hardware components such as:

- Power source
- Network switch
- Physical server rack

VMs in an Availability Set are distributed across multiple fault domains.

Example:

```text
Availability Set

Fault Domain 0        Fault Domain 1

┌──────────────┐      ┌──────────────┐
│    VM-1      │      │    VM-2      │
└──────────────┘      └──────────────┘
```

If one fault domain experiences a hardware failure, VMs in another fault domain remain available.

---

# Update Domains

An **Update Domain** is a group of VMs that Azure updates together during planned maintenance.

Azure does not update all VMs in an Availability Set at the same time.

Example:

```text
Availability Set

Update Domain 0
   VM-1

Update Domain 1
   VM-2

Update Domain 2
   VM-3
```

During planned maintenance:

```text
Update Domain 0
      ↓
VM-1 restarted

Other VMs remain available
```

After that, Azure moves to the next update domain.

This helps reduce downtime during planned maintenance.

---

# Fault Domains vs Update Domains

| Fault Domain | Update Domain |
|---|---|
| Protects against hardware failures | Protects against planned maintenance |
| Physical infrastructure grouping | Maintenance grouping |
| Power/network/rack failures | Azure update events |
| VMs spread across hardware | VMs updated in sequence |

---

# Availability Set Architecture

```text
                  Availability Set
                         │
          ┌──────────────┼──────────────┐
          │              │              │
         VM-1           VM-2           VM-3
          │              │              │
        FD-0           FD-1           FD-0
        UD-0           UD-1           UD-2
```

The VMs are distributed across fault and update domains to reduce the chance that one event affects all VMs.

---

# Availability Zones

An **Availability Zone** is a physically separate Azure data center within the same Azure region.

Each zone has independent infrastructure, including:

- Power
- Cooling
- Networking

Example:

```text
Azure Region

┌────────────┐   ┌────────────┐   ┌────────────┐
│   Zone 1   │   │   Zone 2   │   │   Zone 3   │
│            │   │            │   │            │
│   VM-1     │   │   VM-2     │   │   VM-3     │
└────────────┘   └────────────┘   └────────────┘
```

If one zone becomes unavailable, workloads in other zones can continue running.

---

# Availability Set vs Availability Zone

This is an important AZ-104 comparison.

| Availability Set | Availability Zone |
|---|---|
| Protects against host/rack failures | Protects against data center failures |
| Within a data center infrastructure | Separate physical data centers |
| Uses Fault Domains | Uses Zones |
| Uses Update Domains | Independent zone infrastructure |
| Suitable for many traditional HA workloads | Higher resilience across zones |

---

# When to Use Availability Set

Use an Availability Set when:

- You have multiple VMs.
- The VMs should remain available during host failures.
- The workload is deployed within one region without requiring zone-level separation.

Example:

```text
Web Application

       Load Balancer
            │
      ┌─────┴─────┐
      │           │
    VM-1        VM-2

Availability Set
```

---

# When to Use Availability Zones

Use Availability Zones when:

- The application requires higher availability.
- You want protection against an entire data center failure.
- The selected Azure region supports Availability Zones.

Example:

```text
             Load Balancer
                  │
       ┌──────────┼──────────┐
       │          │          │
     Zone 1     Zone 2     Zone 3
      VM-1       VM-2       VM-3
```

---

# Azure Dedicated Host

An **Azure Dedicated Host** provides a physical server dedicated to your organization.

Unlike standard Azure VMs, where physical infrastructure is shared with other customers, a Dedicated Host reserves the physical host for your workloads.

```text
Azure Dedicated Host

┌─────────────────────────────┐
│ Your Organization's Host    │
│                             │
│  VM-1                       │
│  VM-2                       │
│  VM-3                       │
│                             │
└─────────────────────────────┘
```

---

# Why Use Azure Dedicated Host?

Common reasons include:

- Compliance requirements
- Licensing requirements
- Need for physical server isolation
- Visibility into the underlying host

It is generally used for specialized enterprise workloads rather than typical applications.

---

# Dedicated Host vs Standard VM

| Standard VM | Dedicated Host |
|---|---|
| Shared physical infrastructure | Dedicated physical server |
| Lower cost | Higher cost |
| Suitable for most workloads | Specialized enterprise workloads |
| Azure manages host allocation | Dedicated host capacity |

---

# Availability Set vs Dedicated Host

These solve different problems.

| Availability Set | Dedicated Host |
|---|---|
| Improves VM availability | Provides physical host isolation |
| Distributes VMs across infrastructure | Reserves an entire physical host |
| Protects against failures | Used for compliance/licensing |
| Does not provide dedicated hardware | Dedicated hardware |

---

# High Availability Architecture

A common architecture is:

```text
                  Internet
                      │
                      ▼
               Azure Load Balancer
                      │
          ┌───────────┴───────────┐
          │                       │
        VM-1                    VM-2
          │                       │
      Availability Zone 1    Availability Zone 2
```

The Load Balancer distributes traffic across the available VMs.

If one VM or zone becomes unavailable, traffic can be sent to the remaining healthy VM.

---

# Availability Set with Load Balancer

```text
                Load Balancer
                     │
             ┌───────┴───────┐
             │               │
           VM-1            VM-2
             │               │
       Availability Set
```

This pattern is commonly used for highly available applications.

---

# Practical Lab

## Lab: Create an Availability Set and Deploy VMs

### Objective

Create an Availability Set and deploy two VMs into it.

### Architecture

```text
              Availability Set
                    │
            ┌───────┴───────┐
            │               │
          VM-1            VM-2
```

---

### Step 1: Create an Availability Set

1. Open the **Azure Portal**.
2. Search for **Availability Sets**.
3. Select **Create**.
4. Configure:
   - Subscription
   - Resource Group
   - Name
   - Region
5. Create the Availability Set.

---

### Step 2: Create VM-1

1. Open **Virtual Machines**.
2. Select **Create**.
3. Configure the VM.
4. Under **Availability options**, select the Availability Set.
5. Create the VM.

---

### Step 3: Create VM-2

Create another VM in the same Availability Set.

---

### Step 4: Verify

Open the Availability Set and verify that both VMs are listed.

Review the fault and update domain placement.

---

# Key Points

- A single VM can become a single point of failure.
- **Availability Sets** distribute VMs across fault and update domains.
- **Fault Domains** protect against hardware and infrastructure failures.
- **Update Domains** protect against planned Azure maintenance affecting all VMs simultaneously.
- **Availability Zones** use separate physical data centers within the same Azure region.
- Availability Zones provide higher resilience than Availability Sets against data center failures.
- **Azure Dedicated Host** provides dedicated physical servers for specialized workloads.
- Availability Sets, Availability Zones, and Dedicated Hosts solve different availability and placement requirements.
- Production workloads commonly combine multiple VMs with a Load Balancer to improve availability.
