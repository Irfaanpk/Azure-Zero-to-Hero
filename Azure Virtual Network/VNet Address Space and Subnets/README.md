# 7.2 VNet Address Space and Subnets

An **Azure Virtual Network (VNet)** uses IP address ranges to define how resources communicate within the network.

The VNet address space is divided into smaller networks called **subnets**. Subnets help organize resources and apply networking controls such as Network Security Groups, route tables, and service endpoints.

---

## VNet Address Space

The **address space** defines the range of IP addresses available inside a VNet.

Example:

```text
VNet Address Space: 10.0.0.0/16
```

This provides a large private IP range that can be divided into multiple subnets.

Example:

```text
VNet: 10.0.0.0/16
│
├── Frontend Subnet: 10.0.1.0/24
├── Backend Subnet:  10.0.2.0/24
└── Database Subnet: 10.0.3.0/24
```

---

## CIDR Notation

Azure uses **CIDR (Classless Inter-Domain Routing)** notation to define network ranges.

Example:

```text
10.0.0.0/16
```

The `/16` represents the number of bits used for the network portion.

Common examples:

```text
10.0.0.0/16
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

### CIDR Size

| CIDR | Total IPv4 Addresses |
|---|---:|
| `/16` | 65,536 |
| `/20` | 4,096 |
| `/24` | 256 |
| `/25` | 128 |
| `/26` | 64 |
| `/27` | 32 |
| `/28` | 16 |

The smaller the prefix number, the larger the address range.

```text
/16  → Large network
/20  → Medium network
/24  → Smaller network
/28  → Very small network
```

---

## IPv4 Address Space

Azure VNets commonly use private IPv4 address ranges.

The RFC 1918 private ranges are:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

Example:

```text
VNet:
10.0.0.0/16

Subnet:
10.0.1.0/24
```

---

## IPv6 Address Space

Azure VNets can also support **IPv6 address spaces**.

Example:

```text
IPv6 VNet Address Space:
2001:db8:1234::/48
```

An IPv6 address space can be associated with a VNet and used for IPv6 networking.

---

# What is a Subnet?

A **subnet** is a smaller network segment inside a VNet.

For example:

```text
VNet
10.0.0.0/16
│
├── Frontend
│   10.0.1.0/24
│
├── Application
│   10.0.2.0/24
│
└── Database
    10.0.3.0/24
```

Each subnet uses a portion of the VNet address space.

---

## Why Use Subnets?

Subnets help you:

- Organize resources
- Separate application components
- Apply NSGs
- Apply route tables
- Configure service endpoints
- Configure private endpoints
- Control network traffic
- Create different network segments

Example:

```text
VNet
│
├── Web Subnet
│     └── Web VM
│
├── Application Subnet
│     └── App VM
│
└── Database Subnet
      └── Database
```

---

# VNet Address Space vs Subnet

| VNet Address Space | Subnet |
|---|---|
| Defines the overall network range | Defines a smaller network inside the VNet |
| Can contain multiple subnets | Belongs to a VNet |
| Example: `10.0.0.0/16` | Example: `10.0.1.0/24` |
| Provides the available IP range for the VNet | Uses a portion of that range |

---

# Subnet Address Range

A subnet must use an address range that is contained within the VNet address space.

Example:

```text
VNet:
10.0.0.0/16

Valid Subnets:

10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

Invalid example:

```text
VNet:
10.0.0.0/16

Subnet:
192.168.1.0/24
```

The subnet is outside the VNet address space.

---

# Subnets Cannot Overlap

Subnet address ranges inside the same VNet cannot overlap.

Valid:

```text
VNet: 10.0.0.0/16

Subnet A:
10.0.1.0/24

Subnet B:
10.0.2.0/24
```

Invalid:

```text
Subnet A:
10.0.1.0/24

Subnet B:
10.0.1.128/25
```

The second subnet overlaps with the first subnet.

---

# Subnet Design

A VNet can be divided according to application requirements.

Example:

```text
VNet: 10.0.0.0/16
│
├── Web Subnet
│   10.0.1.0/24
│
├── App Subnet
│   10.0.2.0/24
│
├── Database Subnet
│   10.0.3.0/24
│
└── Management Subnet
    10.0.4.0/24
```

This makes it easier to apply different networking configurations to different workloads.

---

# Azure Reserved IP Addresses

Azure reserves **five IP addresses in every subnet**.

For a subnet such as:

```text
10.0.1.0/24
```

Azure reserves:

```text
10.0.1.0
10.0.1.1
10.0.1.2
10.0.1.3
10.0.1.255
```

Therefore:

```text
Total addresses:
256

Azure-reserved:
5

Usable addresses:
251
```

The five reserved addresses are used for Azure networking functions such as:

- Network address
- Default gateway
- Azure DNS mapping
- Future use
- Broadcast-style addressing

Azure does not support IPv4 broadcast traffic in the same way as a traditional network.

---

# Subnet Size Planning

Subnet size should be planned based on the number of resources that need IP addresses.

Example:

```text
Expected resources:
100

Subnet:
10.0.1.0/24

Total addresses:
256

Azure reserved:
5

Usable:
251
```

A `/24` subnet would provide enough addresses for this example.

For a smaller workload:

```text
10.0.2.0/27
```

provides:

```text
32 total addresses
27 usable addresses
```

Because Azure reserves five addresses.

---

# Address Space Planning

When designing a VNet, plan the address space before deploying resources.

Example:

```text
VNet:
10.0.0.0/16

Frontend:
10.0.1.0/24

Application:
10.0.2.0/24

Database:
10.0.3.0/24

Management:
10.0.4.0/24
```

Leave additional address space available for future subnets.

```text
10.0.0.0/16
│
├── 10.0.1.0/24  → Frontend
├── 10.0.2.0/24  → Application
├── 10.0.3.0/24  → Database
├── 10.0.4.0/24  → Management
│
└── Remaining space → Future use
```

---

# VNet Address Space and Peering

When connecting VNets using **VNet Peering**, their address spaces should not overlap.

Example:

```text
VNet A
10.0.0.0/16
       │
       │ Peering
       ▼
VNet B
10.1.0.0/16
```

This is valid because the address spaces are different.

Avoid:

```text
VNet A
10.0.0.0/16
       │
       │ Peering
       ▼
VNet B
10.0.0.0/16
```

Overlapping address spaces can prevent proper communication between the networks.

---

# Subnet and Network Security

Subnets can be associated with networking controls.

For example:

```text
VNet
│
├── Web Subnet
│     └── NSG
│
├── App Subnet
│     └── NSG
│
└── Database Subnet
      └── NSG
```

This allows different traffic rules to be applied to different parts of the VNet.

NSGs are covered in:

**7.4 Network Security Groups and Application Security Groups**

---

# Special Azure Subnets

Some Azure services require a dedicated subnet.

Examples include:

- Azure Bastion
- Azure Firewall
- Azure VPN Gateway
- Azure Application Gateway
- Private Endpoints
- Delegated Azure services

For example, Azure Bastion requires a dedicated subnet named:

```text
AzureBastionSubnet
```

These requirements should be considered when designing the VNet.

---

# Example Network Architecture

```text
                    Azure VNet
                   10.0.0.0/16
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
   Web Subnet       App Subnet      Database Subnet
   10.0.1.0/24      10.0.2.0/24      10.0.3.0/24
        │               │                │
        ▼               ▼                ▼
     Web VM          App VM          Database
```

Traffic can then be controlled using:

```text
NSG
 │
 ▼
Subnet / NIC
 │
 ▼
Resource
```

---

# Important Points

- A VNet has one or more address spaces.
- An address space defines the IP range available to the VNet.
- Subnets divide the VNet into smaller network segments.
- CIDR notation is used to define address ranges.
- Subnets must be contained within the VNet address space.
- Subnet address ranges cannot overlap.
- Azure reserves five IPv4 addresses in each subnet.
- Proper address planning is important before creating resources.
- VNets that need to communicate through peering should use non-overlapping address spaces.
- Some Azure services require dedicated subnets.
- Subnets can be associated with NSGs, route tables, and service endpoints.

---

# Lab: Create a VNet with Multiple Subnets

## Objective

Create an Azure VNet with multiple subnets and understand how the VNet address space is divided.

## Architecture

```text
VNet
10.0.0.0/16
│
├── WebSubnet
│   10.0.1.0/24
│
├── AppSubnet
│   10.0.2.0/24
│
└── DatabaseSubnet
    10.0.3.0/24
```

## Steps

### 1. Open Azure Portal

Open the **Azure Portal** and search for:

```text
Virtual networks
```

### 2. Create a Virtual Network

Select:

```text
Create
```

Choose your:

- Subscription
- Resource Group
- Region

Example:

```text
VNet Name:
ZeroToHero-VNet
```

### 3. Configure Address Space

Set the IPv4 address space:

```text
10.0.0.0/16
```

### 4. Create the Web Subnet

```text
Subnet Name:
WebSubnet

Subnet Address Range:
10.0.1.0/24
```

### 5. Create the Application Subnet

```text
Subnet Name:
AppSubnet

Subnet Address Range:
10.0.2.0/24
```

### 6. Create the Database Subnet

```text
Subnet Name:
DatabaseSubnet

Subnet Address Range:
10.0.3.0/24
```

### 7. Review and Create

Review the configuration and select:

```text
Create
```

### 8. Verify the VNet

Open the VNet and check:

```text
Address space
```

You should see:

```text
10.0.0.0/16
```

Then open:

```text
Subnets
```

You should see:

```text
WebSubnet
10.0.1.0/24

AppSubnet
10.0.2.0/24

DatabaseSubnet
10.0.3.0/24
```

## Lab Result

You have created a VNet and divided its address space into multiple non-overlapping subnets.

```text
ZeroToHero-VNet
       │
       ├── WebSubnet
       │    10.0.1.0/24
       │
       ├── AppSubnet
       │    10.0.2.0/24
       │
       └── DatabaseSubnet
            10.0.3.0/24
```
