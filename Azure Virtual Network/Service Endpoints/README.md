# Service Endpoints

**Azure Service Endpoints** allow a subnet in an Azure Virtual Network (VNet) to access supported Azure services using the Azure backbone network.

Service endpoints extend the identity of a VNet subnet to an Azure service, allowing you to restrict that service so that only selected VNets or subnets can access it.

---

## What is a Service Endpoint?

Normally, Azure services such as Storage Accounts have public endpoints.

```text
VM
 |
 v
VNet
 |
 v
Azure Storage
Public Endpoint
```

With a service endpoint:

```text
VM
 |
 v
VNet Subnet
 |
 | Service Endpoint
 |
 v
Azure Storage
```

The traffic remains on the Azure backbone rather than going through the public Internet.

---

# Why Use Service Endpoints?

Service endpoints are mainly used to restrict access to Azure services based on VNet subnets.

For example:

```text
VNet
│
├── WebSubnet
│
└── AppSubnet
       │
       │ Service Endpoint
       ▼
   Storage Account
```

The Storage Account can be configured to allow access from:

```text
AppSubnet
```

while denying access from other networks.

---

# How Service Endpoints Work

Service endpoints are configured on the **subnet**.

```text
VNet
 │
 └── Subnet
       │
       │ Service Endpoint
       ▼
 Azure Service
```

The Azure service then uses its network access configuration to allow or deny traffic from that subnet.

---

# Service Endpoint Architecture

Example with Azure Storage:

```text
┌──────────────────────────────┐
│          Azure VNet          │
│                              │
│  ┌────────────────────────┐  │
│  │      AppSubnet         │  │
│  │                        │  │
│  │         VM             │  │
│  └───────────┬────────────┘  │
│              │               │
│       Service Endpoint       │
└──────────────┼───────────────┘
               │
               │ Azure Backbone
               ▼
       ┌──────────────────┐
       │  Storage Account │
       └──────────────────┘
```

---

# Supported Azure Services

Service endpoints are available for selected Azure services.

Common examples include:

- Azure Storage
- Azure SQL Database
- Azure Key Vault
- Azure Service Bus
- Azure Event Hubs
- Azure Cosmos DB

The exact supported services can change over time.

---

# Service Endpoint for Azure Storage

A common use case is restricting a Storage Account to a specific subnet.

Example:

```text
VNet
│
├── WebSubnet
│
└── AppSubnet
       │
       │ Microsoft.Storage
       ▼
   Storage Account
```

The Storage Account network settings can allow:

```text
Selected networks
        ↓
VNet
        ↓
AppSubnet
```

Resources outside the allowed network are denied according to the storage account's network access configuration.

---

# Service Endpoint Configuration

A service endpoint has two important sides:

### 1. VNet Subnet

Enable the service endpoint on the subnet.

Example:

```text
Subnet
   │
   └── Service endpoints
          │
          └── Microsoft.Storage
```

### 2. Azure Service

Configure the Azure service's network access rules.

Example:

```text
Storage Account
      │
      └── Networking
             │
             └── Allow selected networks
                    │
                    └── VNet / Subnet
```

Both sides must be configured correctly.

---

# Service Endpoint vs Private Endpoint

These are commonly confused.

| Service Endpoint | Private Endpoint |
|---|---|
| Configured on a subnet | Creates a network interface in a subnet |
| Azure service still uses its public endpoint | Azure service is accessed through a private IP |
| No private IP is created for the service | Private IP is assigned |
| Traffic uses Azure backbone | Traffic uses private connectivity |
| Service remains publicly addressable | Can provide private access |
| Simpler configuration | More advanced private networking |
| Uses service firewall/network rules | Uses Private Link |

### Service Endpoint

```text
VM
 │
 ▼
Subnet
 │
 │ Service Endpoint
 ▼
Storage Public Endpoint
```

### Private Endpoint

```text
VM
 │
 ▼
Subnet
 │
 ▼
Private Endpoint
 │
 │ Private IP
 ▼
Azure Storage
```

---

# Service Endpoint vs Private Endpoint — Important Difference

The key difference is:

```text
Service Endpoint
    ↓
Subnet identity is extended to the Azure service
```

Whereas:

```text
Private Endpoint
    ↓
Private IP is created inside your VNet
```

For example:

```text
Service Endpoint:

VM → Subnet → Azure Storage Public Endpoint


Private Endpoint:

VM → Private IP → Private Endpoint → Azure Storage
```

---

# Service Endpoint vs AWS Gateway Endpoint

For AWS users, **Azure Service Endpoint** is conceptually closest to an **AWS Gateway VPC Endpoint**, especially for services such as S3.

However, they are **not identical**.

| Azure | AWS |
|---|---|
| Service Endpoint | Gateway VPC Endpoint |
| Configured on subnet | Configured through route tables |
| Commonly used with Storage | Commonly used with S3/DynamoDB |
| Service remains publicly addressable | Gateway endpoint provides private access to supported AWS services |

Azure also has **Private Endpoint**, which is conceptually closer to an AWS **Interface VPC Endpoint**.

---

# Service Endpoint and Public Access

A service endpoint does **not** make the Azure service private in the same way as a private endpoint.

For example:

```text
Storage Account
      │
      ├── Public Endpoint
      │
      └── Network Rules
             │
             └── Allow selected VNet/Subnet
```

The service still has its normal service endpoint, but network access can be restricted to selected VNets and subnets.

---

# Service Endpoint and NSG

Service endpoints and NSGs solve different problems.

| Service Endpoint | NSG |
|---|---|
| Provides VNet-to-Azure-service integration | Filters network traffic |
| Used to restrict supported Azure services | Allows/denies traffic |
| Configured on subnet | Applied to subnet/NIC |
| Works with service network rules | Uses security rules |

Example:

```text
VM
 │
 ▼
NSG
 │
 │ Allow/Deny traffic
 ▼
Subnet
 │
 │ Service Endpoint
 ▼
Azure Storage
```

Both can work together.

---

# Practical Lab — Configure Storage Service Endpoint

## Objective

Configure a service endpoint on a subnet and allow that subnet to access an Azure Storage Account.

### Architecture

```text
┌─────────────────────────────┐
│          VNet               │
│                             │
│  ┌───────────────────────┐  │
│  │      AppSubnet        │  │
│  │                       │  │
│  │         VM            │  │
│  └──────────┬────────────┘  │
│             │               │
│      Microsoft.Storage      │
│      Service Endpoint       │
└─────────────┼───────────────┘
              │
              ▼
       Storage Account
```

---

## Step 1: Open the VNet

Open:

```text
Azure Portal
    ↓
Virtual networks
    ↓
ZeroToHero-VNet
```

---

## Step 2: Open the Subnet

Go to:

```text
Subnets
    ↓
AppSubnet
```

---

## Step 3: Enable Service Endpoint

Find:

```text
Service endpoints
```

Select:

```text
Microsoft.Storage
```

Save the subnet configuration.

---

## Step 4: Configure Storage Account Networking

Open the Storage Account:

```text
Storage Account
    ↓
Networking
```

Configure:

```text
Public network access:
Enabled from selected virtual networks and IP addresses
```

Add:

```text
Virtual network:
ZeroToHero-VNet

Subnet:
AppSubnet
```

Save the configuration.

---

## Step 5: Verify

The final configuration should be:

```text
AppSubnet
    │
    │ Microsoft.Storage
    ▼
Storage Account
    │
    └── Network Rules
          │
          └── AppSubnet allowed
```

Test access from a resource in the allowed subnet.

---

# Key Points

- **Service Endpoints** provide secure connectivity from a VNet subnet to supported Azure services.
- Service endpoints are configured on the **subnet**.
- They use the **Azure backbone network**.
- The Azure service can restrict access to selected VNets and subnets.
- Service endpoints do **not** create a private IP for the Azure service.
- The service still uses its normal service endpoint.
- **Private Endpoint** creates a private IP inside the VNet.
- Service endpoints and NSGs solve different networking problems.
- For AWS users, Service Endpoint is conceptually closest to a **Gateway VPC Endpoint**.
- For private IP-based access, use **Private Endpoint**.
