# VM States and Actions

Azure Virtual Machines have different **power states** that describe the current condition of a VM.

Understanding VM states is important because some states continue to consume compute resources and incur compute charges, while others release the compute resources.

---

## VM Power States

The main VM states are:

| State | Description |
|---|---|
| **Running** | VM is running and available for workloads |
| **Stopped** | VM has been stopped from inside the operating system |
| **Stopped (Deallocated)** | VM is stopped and compute resources are released |
| **Starting** | VM is being started |
| **Stopping** | VM is being stopped |
| **Deallocating** | Azure is releasing the VM's compute resources |
| **Unknown** | Azure cannot determine the current state |

---

# 1. Running

When a VM is **Running**, the VM is actively using compute resources.

```text
VM
 │
 ├── CPU
 ├── Memory
 └── Applications
```

Example:

```text
Power State: Running
```

The VM is available for applications and user connections.

---

# 2. Stopped

A VM can be stopped from inside the operating system.

For example, from a Linux VM:

```bash
sudo shutdown now
```

or from a Windows VM:

```text
Start
  ↓
Shut down
```

The VM is no longer running, but the underlying compute resources may still be allocated.

Therefore, **Stopped** and **Stopped (Deallocated)** are different.

---

# 3. Stopped (Deallocated)

When a VM is **deallocated**, Azure releases the compute resources allocated to that VM.

```text
Running
   │
   │ Deallocate
   ▼
Stopped (Deallocated)
```

This is useful when a VM does not need to run for some period.

For example:

```text
Learning VM
     │
     ▼
Use during class
     │
     ▼
Deallocate after class
```

Deallocating a VM can reduce compute costs.

> Storage resources such as managed disks can continue to incur charges even when the VM is deallocated.

---

# Stopped vs Stopped (Deallocated)

This distinction is important.

| Stopped | Stopped (Deallocated) |
|---|---|
| VM is powered off | VM is powered off |
| Compute resources remain allocated | Compute resources are released |
| Compute billing can continue | Compute billing for the released compute resource stops |
| Can be stopped from inside OS | Azure deallocation releases compute resources |

The Azure Portal may show:

```text
Stopped
```

or:

```text
Stopped (Deallocated)
```

Always check the actual power state.

---

# VM Lifecycle

A basic VM lifecycle can be represented as:

```text
                 Create
                   │
                   ▼
                Running
                /     \
               /       \
            Stop       Restart
             │           │
             ▼           ▼
          Stopped      Running
             │
             │ Deallocate
             ▼
    Stopped (Deallocated)
             │
             │ Start
             ▼
          Running
```

---

# VM Actions

Azure provides several common VM management actions.

| Action | Purpose |
|---|---|
| **Start** | Starts a stopped/deallocated VM |
| **Stop** | Stops the VM |
| **Restart** | Stops and starts the VM again |
| **Deallocate** | Stops the VM and releases compute resources |
| **Redeploy** | Moves the VM to another Azure host |
| **Delete** | Removes the VM resource |

---

# 1. Start

The **Start** action starts a stopped or deallocated VM.

```text
Stopped (Deallocated)
          │
          │ Start
          ▼
       Running
```

After starting, the VM becomes available for workloads.

---

# 2. Stop

The **Stop** action shuts down the VM.

From the Azure Portal:

```text
VM
 ↓
Overview
 ↓
Stop
```

Depending on how the VM is stopped, verify whether it reaches:

```text
Stopped
```

or:

```text
Stopped (Deallocated)
```

---

# 3. Restart

**Restart** reboots the VM.

```text
Running
   │
   │ Restart
   ▼
Running
```

A restart is commonly used after:

- OS updates
- Software installation
- Configuration changes
- Troubleshooting

---

# 4. Deallocate

**Deallocate** stops the VM and releases its compute resources.

```text
Running
   │
   │ Deallocate
   ▼
Stopped (Deallocated)
```

This is particularly useful for development and test VMs that do not need to run continuously.

---

# 5. Redeploy

**Redeploy** moves a VM to a new Azure host.

It can be useful when troubleshooting certain underlying host or connectivity problems.

Conceptually:

```text
Azure Host A
     │
     │ Redeploy
     ▼
Azure Host B
     │
     ▼
Same VM
```

Redeploy is different from recreating the VM.

The VM configuration and associated resources are retained while Azure moves the VM to another host.

---

# 6. Delete

The **Delete** action removes the VM resource.

```text
VM
 │
 │ Delete
 ▼
VM Removed
```

Before deleting a VM, check its associated resources.

Depending on the deletion configuration, resources such as:

- OS disk
- Data disks
- Network interfaces
- Public IP addresses

may be retained or deleted separately.

Always review the deletion options before confirming.

---

# VM Stop vs Deallocate

This is one of the most important concepts in Azure VM management.

### Stop

```text
VM
 │
 ▼
Stopped
 │
 └── Compute resources may remain allocated
```

### Deallocate

```text
VM
 │
 ▼
Stopped (Deallocated)
 │
 └── Compute resources released
```

For a VM used only occasionally:

```text
Work
 ↓
Deallocate
 ↓
Start when required
```

is commonly more cost-efficient than leaving the VM running continuously.

---

# VM State and Billing

The general concept is:

```text
Running
   │
   └── Compute charges apply

Stopped
   │
   └── Compute resources may remain allocated

Stopped (Deallocated)
   │
   └── Compute resources released
```

However, deallocation does **not** mean every Azure resource associated with the VM becomes free.

For example:

```text
VM
├── Compute
├── OS Disk
├── Data Disk
├── Public IP
└── Other resources
```

Some associated resources can continue to incur charges.

Always check the pricing for the specific resource type.

---

# VM State from Azure Portal

Open:

```text
Azure Portal
    ↓
Virtual Machines
    ↓
Select VM
    ↓
Overview
```

You can see the current:

```text
Power state
```

For example:

```text
Power state:
Running
```

or:

```text
Power state:
Stopped (Deallocated)
```

---

# VM State Using Azure CLI

You can check the VM power state using:

```bash
az vm get-instance-view \
  --resource-group MyResourceGroup \
  --name MyVM \
  --query instanceView.statuses
```

A simpler status query is:

```bash
az vm get-instance-view \
  --resource-group MyResourceGroup \
  --name MyVM \
  --query "instanceView.statuses[1].displayStatus" \
  --output tsv
```

Example:

```text
VM running
```

or:

```text
VM deallocated
```

---

# Practical Lab — Manage VM States

## Objective

Create or use an existing VM and practice the different VM management actions.

### Step 1: Check Current State

Open:

```text
Azure Portal
    ↓
Virtual Machines
    ↓
ZeroToHero-VM
```

Check the current power state.

---

### Step 2: Stop the VM

Select:

```text
Stop
```

Observe the resulting power state.

---

### Step 3: Start the VM

Select:

```text
Start
```

Verify:

```text
Power state: Running
```

---

### Step 4: Restart the VM

Select:

```text
Restart
```

Wait for the VM to return to:

```text
Running
```

---

### Step 5: Deallocate the VM

Select:

```text
Stop
```

and ensure the VM reaches:

```text
Stopped (Deallocated)
```

---

### Step 6: Start the VM Again

Select:

```text
Start
```

Verify that the VM returns to:

```text
Running
```

---

# Key Points

- **Running** means the VM is actively using compute resources.
- **Stopped** and **Stopped (Deallocated)** are different states.
- **Deallocated** means Azure has released the VM's compute resources.
- Deallocation can reduce VM compute costs.
- Managed disks and other associated resources can still incur charges.
- **Restart** reboots the VM.
- **Redeploy** moves the VM to another Azure host.
- **Delete** removes the VM resource.
- Always check the VM's **Power State** in the Azure Portal.
- For development and learning environments, deallocate VMs when they are not needed to avoid unnecessary compute charges.
