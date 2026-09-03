# Private Endpoints and Private Link

**Azure Private Endpoint** provides private connectivity to Azure services by assigning a **private IP address from your VNet** to the service.

**Azure Private Link** is the underlying Azure technology that enables private access to supported Azure services and privately accessible services.

---

## What is Private Endpoint?

A **Private Endpoint** is a network interface that gets a private IP address from a subnet in your VNet.

It connects that private IP to a supported Azure service.

```text
┌──────────────────────────────┐
│          Azure VNet          │
│                              │
│  ┌───────────┐               │
│  │    VM     │               │
│  └─────┬─────┘               │
│        │                      │
│        ▼                      │
│  ┌────────────────┐          │
│  │ Private        │          │
│  │ Endpoint       │          │
│  │ 10.0.1.10      │          │
│  └───────┬────────┘          │
└──────────┼───────────────────┘
           │
           │ Private Link
           ▼
    Azure Storage
```

The Azure service can therefore be accessed through a private IP address in your VNet.

---

# What is Azure Private Link?

**Azure Private Link** is the technology that provides private connectivity between your VNet and supported Azure services.

Think of it as:

```text
Private Link
     │
     └── Provides private connectivity
             │
             ▼
      Private Endpoint
             │
             ▼
       Azure Service
```

### Simple Difference

| Private Endpoint | Private Link |
|---|---|
| Network interface with a private IP | Azure technology providing private connectivity |
| Created inside your VNet | Provides the underlying private connection |
| Entry point to the service | Connectivity mechanism |
| Visible as a resource in your VNet | Azure networking technology |

---

# Why Use Private Endpoints?

Private Endpoints are commonly used when you want Azure services to be accessed without exposing application traffic to the public Internet.

Common use cases:

- Azure Storage
- Azure SQL Database
- Azure Key Vault
- Azure Cosmos DB
- Azure App Service
- Other supported Azure services

Example:

```text
VM
 │
 ▼
Private IP
 │
 ▼
Private Endpoint
 │
 ▼
Azure Storage
```

---

# Private Endpoint Architecture

Example using Azure Storage:

```text
                    Azure VNet
┌────────────────────────────────────────┐
│                                        │
│  Application Subnet                    │
│                                        │
│  ┌─────────────┐                       │
│  │     VM      │                       │
│  └──────┬──────┘                       │
│         │                              │
│         │ Private IP                   │
│         ▼                              │
│  ┌─────────────────┐                   │
│  │ Private Endpoint│                   │
│  │ 10.0.2.10       │                   │
│  └────────┬────────┘                   │
│           │                            │
└───────────┼────────────────────────────┘
            │
            │ Private Link
            ▼
     ┌──────────────────┐
     │  Azure Storage   │
     └──────────────────┘
```

---

# Private Endpoint and Private IP

When you create a Private Endpoint, Azure creates a network interface in your VNet.

Example:

```text
Private Endpoint NIC
        │
        ▼
Private IP: 10.0.2.10
```

The IP address comes from the subnet selected during Private Endpoint creation.

This allows resources inside the VNet to communicate with the service using a private IP.

---

# Public Endpoint vs Private Endpoint

### Public Endpoint

```text
VM
 │
 ▼
Internet / Public Network
 │
 ▼
Azure Storage Public Endpoint
```

### Private Endpoint

```text
VM
 │
 ▼
VNet
 │
 ▼
Private IP
 │
 ▼
Private Endpoint
 │
 ▼
Azure Storage
```

The Private Endpoint provides a private access path from the VNet to the service.

---

# Private Endpoint and DNS

DNS is an important part of Private Endpoint configuration.

Normally, an Azure service might resolve to a public IP.

With a Private Endpoint, the service name should resolve to the private IP associated with the Private Endpoint.

Example:

```text
Storage Account Name
        │
        ▼
Private DNS
        │
        ▼
10.0.2.10
        │
        ▼
Private Endpoint
```

Azure commonly uses **Private DNS zones** for this purpose.

For Azure Storage, an example is:

```text
privatelink.blob.core.windows.net
```

The exact Private DNS zone depends on the Azure service being connected.

---

# Private DNS Architecture

```text
VM
 │
 │ DNS Query
 ▼
Private DNS Zone
 │
 │
 ▼
Private IP
 │
 ▼
Private Endpoint
 │
 ▼
Azure Service
```

Without correct DNS configuration, the application may resolve the service name to a public endpoint instead of the Private Endpoint.

---

# Private Endpoint Connection States

A Private Endpoint connection can have different states.

Common states include:

| State | Meaning |
|---|---|
| Pending | Connection is waiting for approval |
| Approved | Private connection is approved |
| Rejected | Connection request was rejected |
| Disconnected | Connection is no longer active |

For services requiring approval, the service owner can approve or reject the Private Endpoint connection.

---

# Public Network Access

Private Endpoint does not automatically mean that the public endpoint is disabled.

For example:

```text
Azure Storage
   │
   ├── Public Endpoint
   │
   └── Private Endpoint
            │
            ▼
           VNet
```

You can configure the service's networking settings according to your security requirements.

A common architecture is:

```text
Internet
   X
   │
   │ Public access disabled/restricted
   ▼
Azure Storage
      ▲
      │
      │ Private Link
      │
Private Endpoint
      ▲
      │
     VNet
```

---

# Service Endpoint vs Private Endpoint

These two concepts are very important for AZ-104.

| Service Endpoint | Private Endpoint |
|---|---|
| Configured on subnet | Creates a private endpoint/NIC |
| No private IP for the service in your VNet | Private IP is assigned in your VNet |
| Service uses its service endpoint | Service is accessed through private connectivity |
| Service remains publicly addressable | Can be used with restricted/disabled public access |
| Uses Azure backbone | Uses Private Link |
| Simpler | More advanced |
| Uses service network rules | Uses private endpoint connection |

### Service Endpoint

```text
VM
 │
 ▼
Subnet
 │
 │ Service Endpoint
 ▼
Azure Storage
```

### Private Endpoint

```text
VM
 │
 ▼
Private Endpoint
 │
 │ Private IP
 ▼
Azure Storage
```

---

# Private Endpoint vs AWS Interface Endpoint

For AWS users, an Azure Private Endpoint is conceptually closest to an **AWS Interface VPC Endpoint**.

| Azure | AWS |
|---|---|
| Private Endpoint | Interface VPC Endpoint |
| Private IP in subnet | Private IP in subnet |
| Private Link | AWS PrivateLink |
| Connects to supported services | Connects to supported AWS services |

The concepts are similar, but the implementations are not identical.

---

# Private Endpoint vs VNet Peering

These solve different problems.

| Private Endpoint | VNet Peering |
|---|---|
| Connects a VNet to an Azure service | Connects two VNets |
| Uses Private Link | Uses VNet Peering |
| Provides private IP access to service | Provides private network connectivity |
| Common for PaaS services | Common for VNet-to-VNet communication |

Example:

### Private Endpoint

```text
VNet
 │
 ▼
Private Endpoint
 │
 ▼
Azure Storage
```

### VNet Peering

```text
VNet-A
   │
   │ Peering
   ▼
VNet-B
```

---

# Practical Lab — Create a Private Endpoint for Storage

## Objective

Create a Private Endpoint for an Azure Storage Account and access the storage service privately from a VNet.

### Architecture

```text
                 Azure VNet
┌───────────────────────────────────┐
│                                   │
│  VM                               │
│   │                               │
│   ▼                               │
│ Private Endpoint                  │
│ 10.0.2.10                         │
│   │                               │
└───┼───────────────────────────────┘
    │
    │ Private Link
    ▼
Azure Storage Account
```

---

## Step 1: Create/Open a Storage Account

Open the Azure Portal and create or select a Storage Account.

---

## Step 2: Create Private Endpoint

Inside the Storage Account:

```text
Networking
    ↓
Private endpoint connections
    ↓
+ Private endpoint
```

Configure:

```text
Subscription:
Your subscription

Resource group:
Your resource group

Name:
Storage-PrivateEndpoint

Region:
Same region as VNet
```

---

## Step 3: Select the Target Resource

Select:

```text
Connection method:
Connect to an Azure resource in my directory

Resource type:
Microsoft.Storage/storageAccounts
```

Select your Storage Account.

Choose the required sub-resource, such as:

```text
blob
```

---

## Step 4: Select the VNet and Subnet

Select:

```text
Virtual network:
ZeroToHero-VNet

Subnet:
AppSubnet
```

Azure assigns a private IP from the selected subnet.

Example:

```text
AppSubnet
10.0.2.0/24

Private Endpoint:
10.0.2.10
```

---

## Step 5: Configure Private DNS

Enable the option to integrate with a **Private DNS Zone**.

Azure can create or use the appropriate Private DNS zone for the selected service.

For Blob Storage, this commonly involves:

```text
privatelink.blob.core.windows.net
```

---

## Step 6: Verify

Check:

```text
Storage Account
    ↓
Networking
    ↓
Private endpoint connections
```

The connection should show:

```text
Approved
```

Then verify the Private Endpoint network interface and private IP.

---

# Key Points

- **Private Endpoint** provides private access to supported Azure services.
- A Private Endpoint gets a **private IP address from your VNet subnet**.
- **Private Link** is the Azure technology that provides the private connectivity.
- Private Endpoints create a network interface in your VNet.
- **Private DNS** is important for resolving service names to the private IP.
- A Private Endpoint does not automatically disable the service's public endpoint.
- Public network access can be restricted or disabled according to the service's capabilities and configuration.
- **Service Endpoint** does not create a private IP in your VNet.
- **Private Endpoint** provides private IP-based access.
- For AWS users, Private Endpoint is conceptually similar to an **AWS Interface VPC Endpoint**.
- Use **VNet Peering** for VNet-to-VNet connectivity.
- Use **Private Endpoint** when you need private connectivity to supported Azure services.
