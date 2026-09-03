# Introduction to Azure Virtual Machines

Azure Virtual Machines (VMs) are **Infrastructure as a Service (IaaS)** resources that provide virtualized computers running inside Microsoft Azure.

A VM gives you control over the operating system, installed software, applications, and configuration while Azure manages the underlying physical infrastructure.

---

## What is an Azure Virtual Machine?

An Azure VM is a virtual computer running in Azure.

You can choose:

- Operating system
- VM size
- CPU and memory
- Authentication method
- Region
- Networking configuration
- Applications and software

Example:

```text
              Azure
┌─────────────────────────────────┐
│                                 │
│       Virtual Machine           │
│                                 │
│  ┌───────────────────────────┐  │
│  │        Operating System   │  │
│  │                           │  │
│  │   Applications            │  │
│  │   Web Server              │  │
│  │   Runtime / Software      │  │
│  └───────────────────────────┘  │
│                                 │
└─────────────────────────────────┘
```

---

# Why Use Azure Virtual Machines?

Azure VMs are useful when you need control over the operating system and server environment.

Common use cases include:

- Hosting web servers
- Running application servers
- Hosting custom applications
- Running development and testing environments
- Migrating on-premises servers to Azure
- Running workloads that require OS-level control
- Running Windows or Linux workloads

---

# Azure VM Architecture

An Azure VM is not an isolated resource. It works together with several Azure resources.

```text
                    Azure VM
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
        NIC          OS Disk     Data Disks
          │
          ▼
        Subnet
          │
          ▼
       VNet
```

Other resources can also be associated with the VM:

```text
                    Virtual Machine
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
       NIC              OS Disk           Data Disk
        │
        ▼
      Subnet
        │
        ▼
       VNet
        │
        ├── NSG
        └── Public IP (optional)
```

The networking components are covered in detail in the **Azure Virtual Network** section.

---

# Main VM Components

## 1. VM Image

A VM image provides the operating system and initial software configuration used to create the VM.

Examples:

- Windows Server
- Ubuntu
- Debian
- Red Hat Enterprise Linux
- SUSE Linux Enterprise

```text
VM Image
    │
    ▼
New Azure VM
    │
    ▼
Operating System
```

---

## 2. VM Size

The VM size determines the compute resources available to the VM.

It defines resources such as:

- vCPUs
- Memory
- Network performance
- Temporary storage
- Maximum data disks
- Other hardware capabilities

Example:

```text
VM Size
   │
   ├── vCPUs
   ├── Memory
   ├── Network performance
   └── Disk capabilities
```

VM sizes are covered in detail in **8.3 VM Sizes and Pricing Options**.

---

## 3. Operating System

Azure supports both Windows and Linux operating systems.

```text
Azure VM
   │
   ├── Windows Server
   │
   └── Linux
        ├── Ubuntu
        ├── Debian
        ├── RHEL
        └── SUSE
```

The operating system determines how you manage and access the VM.

For example:

```text
Windows VM → RDP
Linux VM   → SSH
```

---

## 4. Network Interface

A VM uses a **Network Interface (NIC)** to communicate with the Azure network.

```text
VM
 │
 ▼
NIC
 │
 ▼
Subnet
 │
 ▼
VNet
```

The NIC handles network connectivity for the VM.

Networking concepts such as NICs, IP addresses, subnets, and NSGs are covered in **Section 7 — Azure Virtual Network**.

---

## 5. OS Disk

Every Azure VM has an operating system disk.

It contains:

- Operating system
- System files
- Installed applications
- OS configuration

```text
VM
 │
 └── OS Disk
       │
       └── Operating System
```

Detailed disk management is covered separately in the **Azure Managed Disks** section.

---

# Azure VM Lifecycle

A VM can move through different states during its lifecycle.

```text
Create
  │
  ▼
Running
  │
  ├── Stop
  │
  ▼
Stopped / Deallocated
  │
  └── Start
       │
       ▼
     Running
```

Other actions include:

- Restart
- Redeploy
- Delete

These are covered in detail in **8.2 VM States and Actions**.

---

# Windows VM vs Linux VM

| Windows VM | Linux VM |
|---|---|
| Windows Server operating system | Linux operating system |
| Commonly accessed using RDP | Commonly accessed using SSH |
| Windows-specific applications | Linux-specific applications |
| IIS can be used as a web server | Apache/Nginx can be used as web servers |
| Windows administration tools | Linux command-line tools |

---

# Azure VM Deployment Options

Azure VMs can be created using different management methods.

### Azure Portal

Graphical interface:

```text
Azure Portal
     ↓
Virtual Machines
     ↓
Create
     ↓
Configure VM
```

### Azure CLI

VMs can also be created and managed using Azure CLI.

Example:

```bash
az vm create \
  --resource-group MyResourceGroup \
  --name MyVM \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys
```

### Infrastructure as Code

VMs can also be deployed using tools such as:

- ARM templates
- Bicep
- Terraform

For this repository, the primary hands-on approach will be **Azure Portal**, with CLI used where useful.

---

# VM Creation Flow

When creating a VM, the major decisions are:

```text
Region
   ↓
VM Image
   ↓
VM Size
   ↓
Authentication
   ↓
Networking
   ↓
Disk Configuration
   ↓
Management Options
   ↓
Review + Create
```

Example:

```text
              Create VM
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Image     Size    Authentication
        │         │         │
        └─────────┼─────────┘
                  ▼
              Networking
                  │
                  ▼
                Disks
                  │
                  ▼
              Create VM
```

---

# VM Region

When creating a VM, you select an Azure region.

Example:

```text
Azure
 │
 ├── East US
 ├── West Europe
 ├── Central India
 └── Southeast Asia
```

The VM is deployed into the selected region.

The region affects:

- Availability
- Latency
- Pricing
- Supported VM sizes
- Compliance requirements

---

# VM Image

The image determines the initial operating system.

Example:

```text
Ubuntu Image
      │
      ▼
Azure VM
      │
      ▼
Ubuntu Operating System
```

You can select images from the Azure Marketplace or use other supported image sources.

---

# VM Authentication

When creating a VM, you configure how administrators will access it.

For Linux:

```text
SSH Key
   or
Password
```

For Windows:

```text
Username + Password
```

SSH keys and VM access are covered in detail in **8.4 SSH Keys and VM Access**.

---

# VM Pricing

Azure VM cost depends on several factors, including:

- VM size
- Region
- Operating system
- Usage duration
- Pricing model
- Attached resources

Common pricing options include:

```text
Pay-as-you-go
Reservations
Savings Plan
Spot VMs
```

These are covered in detail in **8.3 VM Sizes and Pricing Options**.

---

# VM Use Case Example

Suppose you want to host a website.

```text
Internet
    │
    ▼
Azure Load Balancer
    │
    ▼
Azure VM
    │
    ▼
Nginx
    │
    ▼
Website
```

The VM runs the operating system and web server, while Azure networking services provide connectivity.

---

# VM High Availability

For production workloads, a single VM can become a single point of failure.

Instead, multiple VMs can be deployed using Azure availability features.

```text
             Application
                  │
          ┌───────┴───────┐
          ▼               ▼
        VM-1             VM-2
          │               │
          └───────┬───────┘
                  │
              Availability
```

Azure provides:

- Availability Sets
- Availability Zones

These concepts are covered in **8.5 VM Availability and Placement**.

---

# VM Scaling

A single VM has a fixed amount of compute capacity.

If the application needs more instances, **Virtual Machine Scale Sets (VMSS)** can be used.

```text
              VM Scale Set
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
      VM-1       VM-2       VM-3
```

VM Scale Sets are covered in **8.11 VM Scale Sets**.

---

# Azure VM vs On-Premises Server

| Azure VM | On-Premises Server |
|---|---|
| Runs in Azure data center | Runs in organization's data center |
| Azure manages physical infrastructure | Organization manages physical hardware |
| Flexible VM sizing | Hardware capacity is usually fixed |
| Can scale quickly | Scaling may require new hardware |
| Pay for Azure resources | Hardware and infrastructure costs |
| Azure handles physical maintenance | Organization handles physical maintenance |

---

# Azure VM vs Azure App Service

Both can host applications, but they provide different levels of control.

| Azure VM | Azure App Service |
|---|---|
| IaaS | PaaS |
| Full OS control | OS managed by Azure |
| Install your own software | Platform manages much of the infrastructure |
| More administration | Less infrastructure management |
| Suitable for custom OS-level requirements | Suitable for supported web applications |

---

# Practical Lab — Create Your First Azure VM

## Objective

Create a basic Linux VM using the Azure Portal and access it after deployment.

### Architecture

```text
                    Azure
                      │
                      ▼
              ┌──────────────┐
              │     VNet     │
              │              │
              │   Subnet     │
              │      │       │
              │      ▼       │
              │     VM       │
              │   Ubuntu     │
              └──────────────┘
```

---

## Step 1: Open Virtual Machines

Open:

```text
Azure Portal
    ↓
Virtual Machines
    ↓
Create
    ↓
Azure Virtual Machine
```

---

## Step 2: Configure Basics

Configure:

```text
Subscription:
Your subscription

Resource Group:
Create/select a resource group

Virtual Machine Name:
ZeroToHero-VM

Region:
Choose your region

Image:
Ubuntu Server

Authentication:
SSH public key
```

---

## Step 3: Select VM Size

Choose an appropriate size for the learning lab.

For example:

```text
General-purpose
```

Use the smallest suitable size available for your subscription and region to minimize cost.

---

## Step 4: Configure Networking

Select:

```text
Virtual Network
Subnet
NIC
```

Use the networking concepts already covered in **Section 7**.

---

## Step 5: Review and Create

Select:

```text
Review + create
```

After validation succeeds:

```text
Create
```

Azure deploys the VM.

---

## Step 6: Verify the VM

Open:

```text
Virtual Machines
    ↓
ZeroToHero-VM
```

Check:

- VM status
- Region
- VM size
- Operating system
- Private IP
- Public IP, if configured
- Network interface

---

## Step 7: Access the VM

For a Linux VM, connect using SSH.

Example:

```bash
ssh azureuser@PUBLIC_IP
```

For Windows VMs, use RDP or Azure Bastion.

---

# Key Points

- **Azure Virtual Machines** provide IaaS compute resources.
- You have control over the operating system and installed software.
- Azure manages the underlying physical infrastructure.
- Azure supports both **Windows and Linux VMs**.
- A VM commonly works with a **NIC, VNet, subnet, and OS disk**.
- The VM **image** determines the initial operating system.
- The **VM size** determines available compute resources.
- VM states and lifecycle actions affect management and billing.
- VM networking is provided through Azure networking resources covered in Section 7.
- **Availability Sets and Availability Zones** can improve VM availability.
- **VM Scale Sets** allow multiple VM instances to be managed and scaled together.
- VM disks, monitoring, backup, and other specialized topics are covered separately.
