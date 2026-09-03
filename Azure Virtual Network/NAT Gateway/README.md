# NAT Gateway

**Azure NAT Gateway** provides reliable and scalable **outbound Internet connectivity** for resources inside an Azure Virtual Network.

It allows resources in a subnet to access the Internet using a predictable public IP address without assigning a public IP directly to each resource.

---

## What is NAT Gateway?

**NAT** stands for **Network Address Translation**.

Azure NAT Gateway performs **Source Network Address Translation (SNAT)** for outbound connections.

```text
Azure VNet
│
├── Web Subnet
│      │
│      ├── VM-1
│      └── VM-2
│
│      NAT Gateway
│          │
│          ▼
│       Internet
```

The private IP of the resource is translated to the public IP associated with the NAT Gateway when it connects to the Internet.

---

# Why Use NAT Gateway?

Without NAT Gateway, outbound Internet connectivity can depend on other Azure networking configurations.

NAT Gateway provides:

- Predictable outbound public IP addresses
- Scalable outbound connectivity
- SNAT for outbound connections
- Centralized outbound Internet access for a subnet
- No need to assign public IP addresses directly to individual VMs

Example:

```text
VM-1 ──┐
       │
VM-2 ──┼── NAT Gateway ──→ Internet
       │
VM-3 ──┘
```

All resources can use the NAT Gateway's public IP for outbound connections.

---

# How NAT Gateway Works

Suppose a VM has:

```text
Private IP:
10.0.1.10
```

The VM wants to connect to:

```text
Internet
```

The NAT Gateway translates the source address:

```text
Before NAT:

10.0.1.10
     │
     ▼
Internet
```

After NAT:

```text
NAT Gateway Public IP
     │
     ▼
Internet
```

The Internet sees the NAT Gateway's public IP instead of the VM's private IP.

---

# SNAT

**SNAT (Source Network Address Translation)** changes the source IP address of outbound traffic.

Example:

```text
VM
10.0.1.10
   │
   │ Outbound request
   ▼
NAT Gateway
20.x.x.x
   │
   ▼
Internet
```

The destination sees:

```text
Source IP = NAT Gateway Public IP
```

instead of:

```text
10.0.1.10
```

---

# NAT Gateway Architecture

```text
                     Internet
                         ▲
                         │
                  Public IP
                         │
                  ┌──────┴──────┐
                  │ NAT Gateway │
                  └──────┬──────┘
                         │
                    Azure VNet
                         │
                  ┌──────┴──────┐
                  │    Subnet   │
                  │             │
                  │ VM-1  VM-2 │
                  └─────────────┘
```

NAT Gateway is associated with a **subnet**.

Resources in that subnet can use the NAT Gateway for outbound Internet connectivity.

---

# NAT Gateway and Subnets

NAT Gateway is attached to one or more subnets.

Example:

```text
VNet
│
├── WebSubnet
│      │
│      └── NAT Gateway
│
├── AppSubnet
│      │
│      └── NAT Gateway
│
└── DatabaseSubnet
```

Multiple subnets can use the same NAT Gateway.

---

# Public IP with NAT Gateway

A NAT Gateway requires a public IP address or public IP prefix for Internet connectivity.

Example:

```text
NAT Gateway
     │
     ├── Public IP 1
     ├── Public IP 2
     └── Public IP 3
```

Using multiple public IPs provides additional SNAT ports for outbound connections.

---

# NAT Gateway and Public IP on VM

A VM does not need its own public IP to use NAT Gateway for outbound Internet connectivity.

### Without NAT Gateway

```text
VM
 │
 └── Public IP
        │
        ▼
     Internet
```

### With NAT Gateway

```text
VM
 │
 │ Private IP
 ▼
NAT Gateway
 │
 │ Public IP
 ▼
Internet
```

This keeps the VM from needing a directly assigned public IP for outbound access.

---

# NAT Gateway vs Public IP

| NAT Gateway | Public IP on VM |
|---|---|
| Provides outbound Internet connectivity | Gives the VM a public IP |
| Attached to subnet | Attached to a resource/NIC |
| Centralized outbound access | Individual resource access |
| Predictable outbound IP | Resource-specific public IP |
| Designed for outbound connections | Can support broader public connectivity |
| Does not make the VM directly reachable from Internet | Can allow inbound connectivity when permitted |

---

# NAT Gateway and Inbound Traffic

NAT Gateway is primarily designed for **outbound connectivity**.

```text
VM
 │
 ▼
NAT Gateway
 │
 ▼
Internet
```

It does not provide inbound Internet access to the VM.

If you need inbound connectivity, use services such as:

- Azure Load Balancer
- Azure Application Gateway
- Azure Firewall
- Public IP with appropriate configuration

---

# NAT Gateway and NSG

NAT Gateway and NSG have different responsibilities.

| NAT Gateway | NSG |
|---|---|
| Provides outbound address translation | Filters network traffic |
| Handles SNAT | Allows or denies traffic |
| Provides outbound public IP | Uses security rules |
| Attached to subnet | Attached to subnet or NIC |

Example:

```text
VM
 │
 ▼
NSG
 │
 │ Allow/Deny
 ▼
NAT Gateway
 │
 │ SNAT
 ▼
Internet
```

---

# NAT Gateway and Route Tables

NAT Gateway works with Azure routing.

For example:

```text
VM
 │
 ▼
Subnet
 │
 ├── Route Table
 │
 └── NAT Gateway
        │
        ▼
     Internet
```

If a custom route sends Internet traffic to another next hop, such as a firewall or network virtual appliance, that routing configuration can affect how outbound traffic is handled.

Therefore, when troubleshooting NAT Gateway, check:

- Subnet association
- NAT Gateway configuration
- Public IP configuration
- Route tables
- NSGs
- Firewall rules
- Effective routes

---

# NAT Gateway Use Cases

### 1. Predictable Outbound IP

Applications may need a fixed public IP for external allowlists.

```text
Application
     │
     ▼
NAT Gateway
     │
     │ Public IP: 20.x.x.x
     ▼
External Service
```

The external service can allow:

```text
20.x.x.x
```

---

### 2. Multiple VMs

Instead of assigning public IPs to every VM:

```text
VM-1 ──┐
VM-2 ──┼── NAT Gateway ──→ Internet
VM-3 ──┘
```

---

### 3. Private Application Servers

Application servers can remain private while still accessing external services.

```text
Private VM
10.0.2.10
    │
    ▼
NAT Gateway
    │
    ▼
Internet
```

---

# Practical Lab — Configure NAT Gateway

## Objective

Create a NAT Gateway, associate it with a subnet, and provide outbound Internet connectivity to resources in that subnet.

### Architecture

```text
                    Internet
                        ▲
                        │
                  Public IP
                        │
                 ┌──────┴──────┐
                 │ NAT Gateway │
                 └──────┬──────┘
                        │
                   AppSubnet
                        │
                  ┌─────┴─────┐
                  │           │
                 VM-1       VM-2
```

---

## Step 1: Create a Public IP

Create a **Standard Public IP** in the same region as the NAT Gateway.

Example:

```text
Name:
ZeroToHero-NAT-PIP

SKU:
Standard
```

---

## Step 2: Create NAT Gateway

Open:

```text
Azure Portal
    ↓
NAT gateways
    ↓
Create
```

Configure:

```text
Name:
ZeroToHero-NAT

Region:
Same region as VNet

Public IP:
ZeroToHero-NAT-PIP
```

Create the NAT Gateway.

---

## Step 3: Associate NAT Gateway with Subnet

Open the NAT Gateway:

```text
Subnets
    ↓
Associate
```

Select:

```text
Virtual network:
ZeroToHero-VNet

Subnet:
AppSubnet
```

Save the configuration.

---

## Step 4: Verify Outbound IP

From a VM inside the associated subnet, access an external IP-checking service.

The observed public IP should correspond to the NAT Gateway's public IP.

Conceptually:

```text
VM Private IP
10.0.2.10
      │
      ▼
NAT Gateway
20.x.x.x
      │
      ▼
Internet
```

---

# Key Points

- **NAT Gateway** provides outbound Internet connectivity.
- NAT Gateway performs **SNAT** for outbound connections.
- NAT Gateway is associated with a **subnet**.
- A VM does not need its own public IP to use NAT Gateway for outbound access.
- NAT Gateway provides predictable outbound public IP addresses.
- Multiple subnets can use the same NAT Gateway.
- NAT Gateway is primarily for **outbound** connectivity.
- NAT Gateway does not provide inbound Internet access to VMs.
- **NSGs control traffic; NAT Gateway performs address translation.**
- **Route tables control traffic paths; NAT Gateway provides outbound SNAT.**
- Use NAT Gateway when private resources need reliable and predictable outbound Internet connectivity.
