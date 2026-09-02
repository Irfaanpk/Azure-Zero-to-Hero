# 6.12 Storage Account Firewall and Network Access

Azure Storage provides network security controls that allow you to control **where storage accounts can be accessed from**.

By default, storage accounts can be accessed through public endpoints. You can restrict access using:

- Public network access settings
- Storage account firewall
- IP network rules
- Virtual network rules
- Private endpoints

---

## Public Network Access

**Public network access** determines whether the storage account can be accessed through its public endpoint.

The main options are:

### Enabled from All Networks

The storage account can be accessed through its public endpoint from any network, subject to authentication and authorization.

```text
Internet
   ↓
Public Endpoint
   ↓
Storage Account
```

---

### Enabled from Selected Virtual Networks and IP Addresses

Access through the public endpoint is restricted to configured:

- IP addresses
- IP ranges
- Virtual networks

```text
Allowed Network
      ↓
Public Endpoint
      ↓
Storage Account

Other Networks
      ↓
❌ Access Denied
```

---

### Disabled

Public network access is disabled.

Access can be provided through private connectivity such as:

```text
Private Endpoint
      ↓
Storage Account
```

---

# Storage Account Firewall

The **Storage Account firewall** allows you to restrict access to specific networks and IP addresses.

It can help prevent unauthorized network access to storage data.

---

## IP Network Rules

You can allow specific public IP addresses or IP ranges.

Example:

```text
Allowed IP:
203.0.113.10
```

Only requests originating from allowed IP addresses can access the storage account through the public endpoint.

---

## Virtual Network Rules

You can restrict access to selected Azure virtual networks.

Example:

```text
Virtual Network
      ↓
Subnet
      ↓
Storage Account
```

Only traffic from configured network locations is allowed according to the storage account network rules.

---

# Trusted Microsoft Services

Azure Storage can provide an option to allow **trusted Microsoft services** to access the storage account even when network restrictions are enabled.

This can be useful when certain Azure services need to access the storage account while firewall restrictions are configured.

---

# Private Endpoint

A **Private Endpoint** provides a private IP address from an Azure Virtual Network for accessing the storage account.

```text
Virtual Network
      │
      ▼
Private Endpoint
      │
      ▼
Azure Storage Account
```

The storage service can then be accessed through a private IP address from the VNet.

---

## Why Use Private Endpoint?

Private endpoints are useful when you want storage traffic to remain on private network connectivity rather than using the public endpoint.

Common use cases:

- Internal applications
- Private enterprise workloads
- Restricted storage accounts
- Applications running inside Azure VNets

---

# Public Endpoint vs Private Endpoint

| Feature | Public Endpoint | Private Endpoint |
|---|---|---|
| Accessible through public network | ✅ | ❌ |
| Uses public IP connectivity | ✅ | ❌ |
| Uses private IP in VNet | ❌ | ✅ |
| Can restrict using firewall rules | ✅ | Not the primary mechanism |
| Suitable for private workloads | Limited | ✅ |

---

# Firewall vs Private Endpoint

These are different approaches.

### Firewall

Controls access to the **public endpoint** based on network rules.

```text
Client
   ↓
Public Endpoint
   ↓
Firewall Rules
   ↓
Storage Account
```

### Private Endpoint

Provides private connectivity to the storage service.

```text
Client
   ↓
VNet
   ↓
Private Endpoint
   ↓
Storage Account
```

---

# Network Access Evaluation

When configuring network access, consider:

```text
Client
   ↓
Network Location
   ↓
Public or Private Endpoint
   ↓
Network Rules
   ↓
Authentication / Authorization
   ↓
Storage Data
```

Network access controls and identity-based access controls are separate security layers.

For example:

```text
Network Access
       +
Authentication
       +
Authorization
       ↓
Storage Access
```

A user may have an appropriate RBAC role but still be unable to connect if the storage account's network configuration blocks the request.

---

# 🧪 Lab: Configure Storage Account Firewall

### Step 1: Open Storage Account

Go to:

```text
https://portal.azure.com
```

Open:

```text
Storage accounts
```

Select your storage account.

---

### Step 2: Open Networking

Go to:

```text
Security + networking → Networking
```

Under:

```text
Public network access
```

select:

```text Enabled from selected virtual networks and IP addresses
```

---

### Step 3: Add Your Public IP

Under the firewall settings, add your current public IP address.

Example:

```text
203.0.113.10
```

Save the configuration.

---

### Step 4: Test Access

From the allowed network, access the storage account.

Verify that the storage service is accessible.

---

### Step 5: Test from Another Network

If possible, try accessing the storage account from a network that is not included in the allowed rules.

The request should be blocked by the network configuration.

---

### Step 6: Restore Configuration

For a learning environment, you can return the setting to:

```text
Enabled from all networks
```

or configure the required network rules for your environment.

---

# 🧪 Lab: Create a Private Endpoint

### Step 1: Open Storage Account

Open your storage account in the Azure Portal.

Go to:

```text
Security + networking → Networking
```

---

### Step 2: Open Private Endpoint Connections

Select:

```
text Private endpoint connections
```

Click:

```
text + Private endpoint
```

---

### Step 3: Configure Basics

Select:

```text
Subscription
Resource Group
Region
```

Enter a private endpoint name.

Example:

```text
storage-private-endpoint
```

---

### Step 4: Select Resource

For resource type, select:

```
text Microsoft.Storage/storageAccounts
```

Select your storage account.

Choose the appropriate storage sub-resource, such as:

```
text blob
```

---

### Step 5: Configure Virtual Network

Select:

```
text Virtual Network
Subnet
```

The private endpoint receives a private IP address from the selected subnet.

---

### Step 6: Review and Create

Click:

```
text Review + create
```

Then:

```
text Create
```

After deployment, verify that the private endpoint appears under:

```
text Private endpoint connections
```

---

## Key Points

- Storage network security controls **where** a storage account can be accessed from.
- Public network access can be enabled for all networks or restricted to selected networks.
- Storage firewalls can restrict access using IP addresses and virtual network rules.
- Virtual network rules restrict access based on configured Azure networking.
- A **Private Endpoint** provides private connectivity to a storage account.
- Firewall rules primarily control access through the public endpoint.
- Private endpoints use a private IP address from an Azure VNet.
- Network access and Azure RBAC are separate security controls.
- A user can have the correct RBAC permissions but still be blocked by network rules.
