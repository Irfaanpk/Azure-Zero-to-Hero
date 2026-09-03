# 7.1 Introduction to Azure Virtual Network

<div align="center">

</div>

Azure Virtual Network (**VNet**) is a fundamental networking service in Microsoft Azure that allows Azure resources to communicate securely with each other, with the internet, and with on-premises networks.

A VNet provides an isolated network environment where you can define IP address ranges, create subnets, control network traffic, and establish connectivity between Azure resources.

---

## What is Azure Virtual Network?

An **Azure Virtual Network (VNet)** is a logically isolated network in Azure that provides private networking for Azure resources.

It is similar to a traditional network that you would create in an on-premises data center.

A VNet can be used to:

- Connect Azure resources
- Divide networks into subnets
- Assign private IP addresses
- Control network traffic
- Connect Azure to on-premises networks
- Connect multiple VNets
- Provide private access to Azure services

---

## Why Use Azure Virtual Network?

VNets provide the networking foundation for many Azure services.

Common reasons to use a VNet include:

- **Network isolation** — Keep Azure resources within an isolated network
- **Communication** — Allow resources to communicate with each other
- **Security** — Control traffic using NSGs and other networking controls
- **IP addressing** — Define private IP address ranges
- **Connectivity** — Connect Azure networks to on-premises environments
- **Routing** — Control how network traffic moves between networks
- **Private access** — Access Azure services through private connectivity

---

## Basic VNet Architecture

A basic Azure VNet can contain multiple subnets, and resources can be placed inside those subnets.

```text
                    Azure Virtual Network
              ┌─────────────────────────────┐
              │                             │
              │   VNet Address Space        │
              │   10.0.0.0/16               │
              │                             │
              │   ┌─────────────────────┐   │
              │   │ Subnet-Frontend     │   │
              │   │ 10.0.1.0/24         │   │
              │   │                     │   │
              │   │  VM / Application   │   │
              │   └─────────────────────┘   │
              │                             │
              │   ┌─────────────────────┐   │
              │   │ Subnet-Backend      │   │
              │   │ 10.0.2.0/24         │   │
              │   │                     │   │
              │   │ Database / Services │   │
              │   └─────────────────────┘   │
              │                             │
              └─────────────────────────────┘
```

---

## Main Components of a VNet

### 1. VNet Address Space

The address space defines the range of private IP addresses available inside the VNet.

Example:

```text
10.0.0.0/16
```

This address space can be divided into smaller subnet ranges.

---

### 2. Subnets

A **subnet** is a smaller section of the VNet address space.

Example:

```text
VNet:          10.0.0.0/16

Frontend:      10.0.1.0/24
Backend:       10.0.2.0/24
```

Subnets help organize resources and apply networking controls.

---

### 3. Network Interfaces

A **Network Interface (NIC)** provides network connectivity to Azure resources such as virtual machines.

```text
VNet
 │
 └── Subnet
      │
      └── NIC
           │
           └── VM
```

A NIC can have private IP configurations and can be associated with a public IP when required.

---

### 4. Network Security Groups

A **Network Security Group (NSG)** controls inbound and outbound network traffic.

For example:

```text
Internet
   │
   ▼
 NSG
   │
   ▼
 VM
```

NSGs use security rules based on factors such as:

- Source
- Destination
- Protocol
- Port
- Direction
- Priority

NSGs are covered in detail in **7.4 Network Security Groups and Application Security Groups**.

---

### 5. Route Tables

Azure uses routing rules to determine where network traffic should go.

You can use **User-Defined Routes (UDRs)** to create custom routing behavior.

Example:

```text
VM
 │
 ▼
Route Table
 │
 ▼
Firewall / NVA
 │
 ▼
Destination
```

Routing is covered in detail in **7.5 Route Tables and User-Defined Routes**.

---

## VNet Communication

Azure resources inside the same VNet can communicate using their private IP addresses, subject to applicable network security and routing rules.

Example:

```text
VNet
 │
 ├── Subnet 1
 │     └── VM 1
 │
 └── Subnet 2
       └── VM 2

VM 1 ───────────► VM 2
       Private IP
```

Resources in different VNets require connectivity such as **VNet Peering** or other networking solutions.

---

## VNet Connectivity

Azure VNets can connect to different networks and services.

### VNet to VNet

```text
VNet A
   │
   │ VNet Peering
   │
   ▼
VNet B
```

### Azure to On-Premises

```text
On-Premises Network
        │
        │ VPN
        ▼
   VPN Gateway
        │
        ▼
      Azure VNet
```

### VNet to Azure Service

```text
Azure VNet
    │
    ▼
Private Endpoint
    │
    ▼
Azure Service
```

---

## VNet and Internet Connectivity

A VNet can provide resources with connectivity to the internet depending on their network configuration.

For example:

```text
Internet
    │
    ▼
 Public IP
    │
    ▼
   NIC
    │
    ▼
    VM
```

For outbound-only internet connectivity, Azure services such as **NAT Gateway** can be used.

---

## VNet Isolation

Each VNet is logically isolated from other VNets.

For example:

```text
VNet A                    VNet B
10.0.0.0/16               10.1.0.0/16

   VM A                       VM B

      X  No direct connectivity  X
```

Connectivity between them must be explicitly configured.

For example:

```text
VNet A
   │
   │ VNet Peering
   ▼
VNet B
```

---

## VNet and Subnet Relationship

The hierarchy is:

```text
VNet
 │
 ├── Subnet 1
 │     ├── NIC
 │     └── VM
 │
 ├── Subnet 2
 │     ├── NIC
 │     └── VM
 │
 └── Subnet 3
       └── Azure Resources
```

A subnet belongs to a VNet, and resources are placed into subnets.

---

## Important VNet Concepts

| Concept | Purpose |
|---|---|
| **VNet** | Provides an isolated virtual network |
| **Address Space** | Defines the IP range of the VNet |
| **Subnet** | Divides the VNet into smaller networks |
| **NIC** | Provides network connectivity to resources |
| **Private IP** | Provides internal network communication |
| **Public IP** | Provides internet-facing connectivity when required |
| **NSG** | Controls inbound and outbound traffic |
| **Route Table** | Controls network traffic routing |
| **VNet Peering** | Connects VNets |
| **VPN Gateway** | Connects Azure VNet with external networks |
| **Private Endpoint** | Provides private connectivity to supported Azure services |

---

## VNet Use Cases

### Application Architecture

```text
VNet
 │
 ├── Frontend Subnet
 │      └── Web Servers
 │
 ├── Application Subnet
 │      └── Application Servers
 │
 └── Database Subnet
        └── Database
```

### Hybrid Connectivity

```text
On-Premises
     │
     ▼
VPN Gateway
     │
     ▼
Azure VNet
     │
     ├── Application
     └── Database
```

### Multi-VNet Architecture

```text
             Hub VNet
                │
        ┌───────┼───────┐
        │       │       │
        ▼       ▼       ▼
     Spoke A  Spoke B  Spoke C
```

---

## VNet vs Subnet

| VNet | Subnet |
|---|---|
| Large virtual network | Smaller network inside a VNet |
| Defines overall address space | Uses part of the VNet address space |
| Can contain multiple subnets | Belongs to one VNet |
| Provides network isolation | Organizes resources within the VNet |

---

## Key Points

- Azure VNet is the foundation of Azure networking.
- A VNet provides logical network isolation.
- A VNet contains one or more subnets.
- Address spaces define the IP range available to the VNet.
- Azure resources can communicate using private IP addresses.
- NSGs control network traffic.
- Route tables control traffic routing.
- VNet Peering connects VNets.
- VPN Gateway provides connectivity with external networks.
- Private Endpoints provide private connectivity to supported Azure services.
- VNet concepts are fundamental to understanding Azure Virtual Machines and other Azure services.

---

## Lab: Create and Explore an Azure VNet

### Objective

Create a basic VNet with a subnet and explore its networking configuration using the Azure Portal.

### Architecture

```text
Azure
 │
 └── VNet
      │
      └── Subnet
```

### Steps

1. Open the **Azure Portal**.
2. Search for **Virtual networks**.
3. Select **Create**.
4. Select your Azure subscription.
5. Create or select a resource group.
6. Enter a VNet name.

Example:

```text
VNet Name: ZeroToHero-VNet
```

7. Select the appropriate region.
8. Configure the IPv4 address space.

Example:

```text
10.0.0.0/16
```

9. Create a subnet.

Example:

```text
Subnet Name: default
Subnet Range: 10.0.1.0/24
```

10. Review the configuration.
11. Select **Create**.

### Verify

After deployment:

1. Open the VNet.
2. Check **Address space**.
3. Open **Subnets**.
4. Verify the subnet address range.
5. Explore **Connected devices** and other available networking settings.

You have now created the basic networking foundation that will be used by Azure resources such as Virtual Machines.
