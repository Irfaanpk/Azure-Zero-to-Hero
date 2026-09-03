# 7.3 Network Interfaces and IP Addressing

A **Network Interface (NIC)** is an Azure networking resource that allows a Virtual Machine to communicate with other resources, networks, and the internet.

A NIC contains one or more **IP configurations**, which can include private and public IP addresses.

---

## What is a Network Interface (NIC)?

A **Network Interface Card (NIC)** is a virtual network interface attached to an Azure resource such as a Virtual Machine.

It provides network connectivity between the VM and the VNet.

```text
Azure VNet
    │
    ▼
  Subnet
    │
    ▼
   NIC
    │
    ▼
   VM
```

A NIC is associated with:

- A VNet subnet
- A private IP address
- Optional public IP address
- Network Security Group (NSG)
- IP configurations

---

## Why Use a NIC?

A NIC provides the networking connection for an Azure VM.

It allows the VM to:

- Communicate with other Azure resources
- Communicate with resources in the same VNet
- Communicate with other connected VNets
- Access external networks
- Receive network traffic
- Send network traffic
- Use private and public IP addresses

---

# NIC and Virtual Machine

When you create an Azure VM, a network interface is normally created and attached to the VM.

```text
Virtual Machine
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

The VM uses the NIC for network communication.

---

# IP Addressing in Azure

Azure resources can use different types of IP addresses.

### Private IP Address

A **private IP address** is used for communication inside private networks.

Example:

```text
VM-01
Private IP:
10.0.1.4
```

Private IPs are commonly used for:

- VM-to-VM communication
- VNet communication
- Internal applications
- Private services
- Communication between subnets

---

### Public IP Address

A **public IP address** provides internet-facing connectivity when associated with an Azure resource.

Example:

```text
Public IP:
20.50.100.10
```

Public IPs can be used for scenarios such as:

- Internet-facing applications
- Public-facing VMs
- Load balancers
- VPN gateways
- Azure Bastion

---

# Private IP Configuration

A NIC can have a private IP configuration.

Example:

```text
VNet:
10.0.0.0/16

Subnet:
10.0.1.0/24

NIC:
10.0.1.4
```

Architecture:

```text
VNet
10.0.0.0/16
    │
    ▼
Subnet
10.0.1.0/24
    │
    ▼
NIC
10.0.1.4
    │
    ▼
VM
```

---

# Dynamic Private IP

A private IP can be assigned dynamically.

With dynamic allocation, Azure assigns an available IP address from the subnet.

Example:

```text
Subnet:
10.0.1.0/24

NIC:
10.0.1.4
```

If the IP configuration is changed or the resource is removed and recreated, the address can change depending on the configuration and lifecycle.

---

# Static Private IP

A private IP can also be configured as **static**.

Example:

```text
Subnet:
10.0.1.0/24

NIC:
10.0.1.10
```

The IP remains assigned to that IP configuration until it is changed or removed.

Static private IPs are useful when a resource needs a predictable internal address.

Examples:

- Internal application servers
- Network appliances
- Domain controllers
- Internal services

---

# Public IP Address Configuration

A public IP resource can be associated with an IP configuration on a NIC.

```text
Internet
    │
    ▼
Public IP
20.50.100.10
    │
    ▼
NIC
10.0.1.4
    │
    ▼
VM
```

The public IP and private IP are separate addresses.

---

# Static and Dynamic Public IP

Azure Public IP addresses can be configured with different assignment methods depending on the resource and configuration.

For modern Azure deployments, **Standard Public IP** is the recommended public IP SKU.

Public IP configuration can include:

- IP address
- IP version
- SKU
- Assignment method
- DNS name label, when supported

---

# Public IP vs Private IP

| Private IP | Public IP |
|---|---|
| Used for private network communication | Used for internet-facing connectivity |
| Assigned from a VNet subnet | Provided through an Azure Public IP resource |
| Commonly used by internal resources | Commonly used by public-facing resources |
| Not directly internet-routable | Internet-routable |
| Example: `10.0.1.4` | Example: `20.50.100.10` |

---

# IP Configuration

An IP configuration is a configuration on a NIC that defines how the NIC communicates on the network.

An IP configuration can contain:

- Private IP address
- Public IP association
- Subnet association
- Primary/secondary designation

Example:

```text
NIC
 │
 ├── IP Configuration
 │      ├── Private IP: 10.0.1.4
 │      └── Public IP: 20.50.100.10
 │
 └── Subnet
        10.0.1.0/24
```

---

# Primary IP Configuration

A NIC has a **primary IP configuration**.

It is used as the primary network identity of the NIC.

Example:

```text
NIC
 │
 └── Primary IP Configuration
        │
        ├── Private IP: 10.0.1.4
        └── Public IP: 20.50.100.10
```

---

# Secondary IP Configurations

A NIC can have additional IP configurations.

These can provide additional private IP addresses and, where supported, public IP associations.

Example:

```text
NIC
 │
 ├── Primary IP Configuration
 │      └── 10.0.1.4
 │
 ├── Secondary IP Configuration
 │      └── 10.0.1.5
 │
 └── Secondary IP Configuration
        └── 10.0.1.6
```

This can be useful when a workload needs multiple IP addresses.

---

# Multiple NICs

An Azure VM can have multiple NICs depending on the VM size and configuration.

Example:

```text
                VM
             ┌───────┐
             │       │
             └───┬───┘
                 │
          ┌──────┴──────┐
          │             │
        NIC 1         NIC 2
          │             │
          ▼             ▼
      Subnet 1       Subnet 2
```

Multiple NICs can be useful for workloads that require separate network interfaces.

For example:

```text
NIC 1 → Frontend Network
NIC 2 → Backend Network
```

---

# NIC and NSG

A Network Security Group can be associated with a NIC.

```text
Internet
    │
    ▼
   NSG
    │
    ▼
   NIC
    │
    ▼
   VM
```

The NSG controls network traffic to and from the NIC.

An NSG can also be associated with a subnet.

```text
NSG
 │
 ├── Subnet
 │
 └── NIC
```

NSGs are covered in detail in:

**7.4 Network Security Groups and Application Security Groups**

---

# NIC and Subnet

A NIC is connected to a subnet.

```text
VNet
 │
 └── Subnet
      │
      └── NIC
           │
           └── VM
```

The NIC's private IP address comes from the address range of the subnet.

Example:

```text
VNet:
10.0.0.0/16

Subnet:
10.0.1.0/24

NIC:
10.0.1.4
```

---

# NIC and Public IP

A public IP is not the same as the NIC.

Instead, a Public IP resource can be associated with the NIC's IP configuration.

```text
Public IP
    │
    ▼
IP Configuration
    │
    ▼
NIC
    │
    ▼
VM
```

---

# Example: VM with Private and Public IP

```text
                  Internet
                     │
                     ▼
              Public IP
             20.50.100.10
                     │
                     ▼
                  NIC
          ┌──────────┴──────────┐
          │                     │
     Private IP            NSG Rules
     10.0.1.4
          │
          ▼
         VM
```

The VM uses:

```text
Private IP:
10.0.1.4
```

for internal communication and:

```text
Public IP:
20.50.100.10
```

for internet-facing connectivity when configured.

---

# Private IP Communication

Two VMs in the same VNet can communicate using their private IP addresses.

```text
VNet
 │
 ├── Subnet 1
 │     └── VM-01
 │          10.0.1.4
 │
 └── Subnet 2
       └── VM-02
            10.0.2.4

VM-01
10.0.1.4
   │
   │ Private network
   ▼
VM-02
10.0.2.4
```

The actual connectivity also depends on routing and security rules.

---

# Public IP Communication

A resource with an appropriate public IP configuration can communicate with the internet.

Example:

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

Network Security Groups and other network controls can determine whether traffic is allowed.

---

# IP Address Lifecycle

A typical IP configuration lifecycle is:

```text
Create IP
    │
    ▼
Associate with NIC
    │
    ▼
Use for communication
    │
    ▼
Change / Disassociate
    │
    ▼
Delete
```

---

# Common NIC Operations

You can manage NICs by:

- Creating a NIC
- Attaching a NIC to a VM
- Detaching a NIC
- Changing IP configuration
- Changing private IP assignment
- Associating a public IP
- Removing a public IP association
- Associating an NSG
- Changing subnet association
- Adding secondary IP configurations

---

# Important Points

- A **NIC provides network connectivity** to an Azure VM.
- A NIC is associated with a **subnet**.
- A NIC contains one or more **IP configurations**.
- Private IP addresses are used for internal network communication.
- Public IP addresses provide internet-facing connectivity when required.
- Private IP assignment can be dynamic or static.
- A NIC has a primary IP configuration and can have secondary configurations.
- A VM can have multiple NICs depending on its size and configuration.
- NSGs can be associated with NICs or subnets.
- A Public IP resource is associated with an IP configuration rather than directly being the same object as the NIC.

---

# Lab: Configure NIC and IP Addressing

## Objective

Create a VM and explore its NIC, private IP, public IP, and IP configuration.

## Architecture

```text
VNet
10.0.0.0/16
    │
    ▼
Subnet
10.0.1.0/24
    │
    ▼
NIC
    │
    ├── Private IP
    │   10.0.1.4
    │
    └── Public IP
        XX.XX.XX.XX
    │
    ▼
VM
```

## Steps

### 1. Create a VNet

Create a VNet with:

```text
VNet Name:
ZeroToHero-VNet

Address Space:
10.0.0.0/16
```

Create a subnet:

```text
Subnet Name:
VMSubnet

Address Range:
10.0.1.0/24
```

### 2. Create a Virtual Machine

Create a Windows or Linux VM.

During networking configuration:

- Select `ZeroToHero-VNet`
- Select `VMSubnet`
- Create a NIC
- Create or select a Public IP

### 3. Open the VM Networking Settings

After deployment:

```text
Virtual Machine
    ↓
Networking
```

Identify the attached network interface.

### 4. Open the NIC

Open:

```text
VM
 ↓
Networking
 ↓
Network Interface
```

Check:

- IP configurations
- Private IP address
- Public IP association
- Subnet
- NSG

### 5. Change Private IP Assignment

Open the NIC:

```text
IP configurations
```

Select the primary IP configuration.

Change:

```text
Assignment:
Dynamic
```

to:

```text
Assignment:
Static
```

Choose an available IP address from the subnet.

Example:

```text
10.0.1.10
```

Save the configuration.

### 6. Verify

Check the NIC again and verify:

```text
Private IP:
10.0.1.10

Subnet:
VMSubnet

VNet:
ZeroToHero-VNet
```

Also verify whether a Public IP is associated.

## Lab Result

You have learned how an Azure VM connects to a VNet through a NIC and how private and public IP configurations are associated with the NIC.

```text
VNet
 │
 └── Subnet
      │
      └── NIC
           │
           ├── Private IP
           └── Public IP
                │
                ▼
                VM
```
