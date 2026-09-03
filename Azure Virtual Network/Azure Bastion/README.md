# Azure Bastion

**Azure Bastion** is a fully managed Azure service that provides secure **RDP and SSH access to Azure Virtual Machines** without requiring a public IP address on the VM.

Instead of exposing ports such as:

```text
TCP 3389 → RDP
TCP 22   → SSH
```

directly to the Internet, you connect to the VM through Azure Bastion using the Azure Portal.

---

## What is Azure Bastion?

Azure Bastion is deployed inside an Azure VNet and acts as a secure jump point for administrative access to VMs.

```text
                Azure Portal
                     │
                     │ HTTPS
                     ▼
              Azure Bastion
                     │
                     │ Private connection
                     ▼
              Azure Virtual Machine
```

The VM does not need a public IP address for Bastion access.

---

# Why Use Azure Bastion?

Without Bastion:

```text
Internet
    │
    │ RDP 3389 / SSH 22
    ▼
Public IP
    │
    ▼
VM
```

This exposes management ports to the Internet.

With Bastion:

```text
User
 │
 │ HTTPS
 ▼
Azure Portal
 │
 ▼
Azure Bastion
 │
 │ Private IP
 ▼
VM
```

This reduces the need to expose RDP or SSH ports publicly.

---

# Azure Bastion Architecture

```text
                    Azure Portal
                         │
                         │ HTTPS
                         ▼
              ┌─────────────────────┐
              │   Azure Bastion     │
              │                     │
              │   Public IP         │
              └──────────┬──────────┘
                         │
                         │ Private connectivity
                         ▼
              ┌─────────────────────┐
              │      Azure VNet     │
              │                     │
              │   ┌─────────────┐   │
              │   │     VM      │   │
              │   │ Private IP  │   │
              │   └─────────────┘   │
              └─────────────────────┘
```

---

# Bastion Subnet

Azure Bastion requires a dedicated subnet named:

```text
AzureBastionSubnet
```

Example:

```text
VNet: 10.0.0.0/16

Subnets:

├── WebSubnet
│   10.0.1.0/24
│
├── AppSubnet
│   10.0.2.0/24
│
└── AzureBastionSubnet
    10.0.3.0/26
```

The Bastion service is deployed into the `AzureBastionSubnet`.

> The subnet must use the exact name `AzureBastionSubnet`. Microsoft recommends a subnet of `/26` or larger for Bastion.

---

# Bastion and Public IP

Azure Bastion requires a public IP address for its service.

However, the **VM itself does not need a public IP**.

```text
                 Internet
                    │
                    ▼
              Bastion Public IP
                    │
                    ▼
             Azure Bastion
                    │
                    │ Private
                    ▼
                   VM
```

The public IP belongs to Bastion, not the VM.

---

# RDP Access Through Bastion

For Windows VMs:

```text
User
 │
 ▼
Azure Portal
 │
 ▼
Bastion
 │
 │ RDP
 ▼
Windows VM
```

You can connect to the VM through the Azure Portal without exposing port `3389` to the Internet.

---

# SSH Access Through Bastion

For Linux VMs:

```text
User
 │
 ▼
Azure Portal
 │
 ▼
Bastion
 │
 │ SSH
 ▼
Linux VM
```

This avoids exposing port `22` directly to the Internet.

---

# Bastion vs Direct RDP/SSH

| Direct RDP/SSH | Azure Bastion |
|---|---|
| VM requires public IP for direct Internet access | VM does not need public IP |
| RDP/SSH ports may be exposed | RDP/SSH ports need not be exposed publicly |
| Higher management exposure | More secure administrative access |
| Connect using public IP | Connect through Azure Portal |
| You manage the access path | Azure manages the Bastion service |

---

# Bastion and NSG

Bastion and NSGs work together.

```text
User
 │
 ▼
Bastion
 │
 ▼
NSG
 │
 ▼
VM
```

The NSG must allow the required traffic between the Bastion subnet and the VM subnet according to the Bastion configuration requirements.

When troubleshooting Bastion connectivity, check:

- `AzureBastionSubnet`
- Bastion deployment status
- Bastion public IP
- VM private IP
- NSG rules
- VM operating system firewall
- VM running state

---

# Bastion vs Jump Box

A traditional jump box is a VM used as an intermediate administrative server.

### Jump Box

```text
User
 │
 ▼
Internet
 │
 ▼
Jump Box VM
 │
 ▼
Target VM
```

### Azure Bastion

```text
User
 │
 ▼
Azure Portal
 │
 ▼
Azure Bastion
 │
 ▼
Target VM
```

Azure Bastion is a **managed service**, so you do not have to maintain a jump-box VM.

---

# Practical Lab — Configure Azure Bastion

## Objective

Deploy Azure Bastion and use it to connect to a VM without assigning a public IP to the VM.

### Architecture

```text
                 Azure Portal
                      │
                      ▼
              Azure Bastion
                      │
                      │ Private
                      ▼
              ┌──────────────┐
              │      VM      │
              │ Private IP   │
              └──────────────┘
```

---

## Step 1: Create the Bastion Subnet

Open:

```text
Virtual Network
    ↓
Subnets
    ↓
+ Subnet
```

Create:

```text
Name:
AzureBastionSubnet
```

Use an appropriate subnet range, for example:

```text
10.0.3.0/26
```

---

## Step 2: Create Azure Bastion

Open:

```text
Azure Portal
    ↓
Bastions
    ↓
Create
```

Select:

```text
Virtual network:
ZeroToHero-VNet

Subnet:
AzureBastionSubnet
```

Create or select a **Standard Public IP** for Bastion as required by the selected Bastion configuration.

---

## Step 3: Connect to the VM

Open the VM:

```text
Virtual Machine
    ↓
Connect
    ↓
Bastion
```

Select:

```text
Authentication Type:
VM password / SSH private key
```

Enter the required credentials.

Select:

```text
Connect
```

Azure opens the VM session through Bastion.

---

# Final Architecture

```text
                    User
                     │
                     │ HTTPS
                     ▼
              Azure Portal
                     │
                     ▼
             Azure Bastion
                     │
                     │ Private IP
                     ▼
              ┌──────────────┐
              │ Azure VNet   │
              │              │
              │     VM       │
              │ Private IP   │
              └──────────────┘
```

---

# Key Points

- **Azure Bastion** provides secure RDP and SSH access to Azure VMs.
- Bastion is a **managed Azure service**.
- The VM does **not need a public IP** for Bastion access.
- Bastion requires a dedicated subnet named **`AzureBastionSubnet`**.
- Bastion uses a public IP for the Bastion service itself.
- Windows VMs can be accessed using **RDP**.
- Linux VMs can be accessed using **SSH**.
- Bastion reduces the need to expose ports **3389** and **22** directly to the Internet.
- Bastion is an alternative to maintaining a traditional **jump box**.
- NSGs and the VM's operating system firewall can still affect connectivity.
