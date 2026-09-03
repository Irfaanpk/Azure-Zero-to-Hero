# 17.1 Azure Resource Manager

**Azure Resource Manager (ARM)** is the deployment and management service for Azure. It provides a consistent management layer for creating, updating, deleting, and organizing Azure resources.

---

## What is Azure Resource Manager?

Azure Resource Manager provides a management layer between you and Azure resources.

```text
User / Azure Portal / CLI / PowerShell / API
                    ↓
          Azure Resource Manager
                    ↓
        Azure Resources
```

For example:

```text
Azure Portal
     ↓
ARM
     ↓
Storage Account
Virtual Machine
Virtual Network
Key Vault
```

---

# Why Azure Resource Manager?

ARM provides a consistent way to manage Azure resources.

It allows you to:

- Create resources
- Update resources
- Delete resources
- Organize resources
- Apply access control
- Apply tags
- Apply Azure Policy
- Deploy resources using templates
- Manage resources consistently

---

# ARM Resource Hierarchy

Azure resources are organized through a hierarchy:

```text
Microsoft Entra Tenant
        │
        ▼
   Management Group
        │
        ▼
    Subscription
        │
        ▼
   Resource Group
        │
        ▼
     Resource
```

Example:

```text
Subscription
     │
     └── Production-RG
             │
             ├── Virtual Machine
             ├── Storage Account
             ├── Virtual Network
             └── Key Vault
```

---

# Resource Groups and ARM

A **resource group** is a logical container for Azure resources.

ARM manages resources within their resource groups.

For example:

```text
Resource Group: WebApp-RG
        │
        ├── VM
        ├── NIC
        ├── Public IP
        └── Storage Account
```

Resources can be managed individually or as part of a resource group.

---

# Resource Providers

Azure services are exposed through **resource providers**.

A resource provider manages a particular type of Azure resource.

Examples:

| Resource Provider | Example Resource |
|---|---|
| `Microsoft.Compute` | Virtual Machine |
| `Microsoft.Storage` | Storage Account |
| `Microsoft.Network` | VNet |
| `Microsoft.KeyVault` | Key Vault |
| `Microsoft.Web` | App Service |

Example resource type:

```text
Microsoft.Compute/virtualMachines
```

Here:

```text
Microsoft.Compute
        ↓
Resource Provider

virtualMachines
        ↓
Resource Type
```

---

# Resource Types

A resource type identifies the type of resource being managed.

Examples:

```text
Microsoft.Storage/storageAccounts

Microsoft.Network/virtualNetworks

Microsoft.Compute/virtualMachines

Microsoft.Web/sites
```

The general format is:

```text
ResourceProvider/ResourceType
```

---

# ARM Deployments

ARM uses **deployments** to create or update Azure resources.

A deployment can contain one or multiple resources.

Example:

```text
ARM Deployment
      │
      ├── Virtual Network
      ├── Subnet
      ├── Network Interface
      └── Virtual Machine
```

This allows related infrastructure to be deployed together.

---

# Declarative Deployment

ARM templates and Bicep use a **declarative approach**.

Instead of specifying every individual command to execute, you describe the desired Azure infrastructure.

```text
Desired State
     ↓
ARM Template / Bicep
     ↓
Azure Resource Manager
     ↓
Azure Infrastructure
```

Example:

```text
Desired Infrastructure:

VNet
 ├── Subnet
 └── VM

        ↓

Bicep / ARM Template

        ↓

ARM

        ↓

Resources Created
```

---

# ARM Deployment Scope

Azure Resource Manager supports different deployment scopes, including:

- Tenant
- Management group
- Subscription
- Resource group

For example:

```text
Management Group
        ↓
Subscription
        ↓
Resource Group
        ↓
Resources
```

The appropriate scope depends on what you are deploying or managing.

---

# ARM and Azure Portal

When you create an Azure resource through the Azure Portal, the Portal communicates with Azure management services.

Conceptually:

```text
Azure Portal
     ↓
Azure Resource Manager
     ↓
Resource Provider
     ↓
Azure Resource
```

You do not need to manually interact with ARM for every Portal operation.

---

# ARM and Azure CLI

Azure CLI also uses Azure management APIs.

Example:

```bash
az group create \
  --name myResourceGroup \
  --location eastus
```

Conceptually:

```text
Azure CLI
    ↓
ARM
    ↓
Resource Provider
    ↓
Resource Group
```

---

# ARM and Resource Providers

When ARM receives a request to create a resource, the appropriate resource provider handles the resource operation.

Example:

```text
Create Storage Account
        ↓
       ARM
        ↓
Microsoft.Storage
        ↓
Storage Account
```

Another example:

```text
Create Virtual Machine
        ↓
       ARM
        ↓
Microsoft.Compute
        ↓
Virtual Machine
```

---

# ARM and Access Control

Azure Resource Manager works with **Azure RBAC** to control who can manage Azure resources.

Example:

```text
User
 │
 ▼
Azure RBAC
 │
 ▼
ARM
 │
 ▼
Azure Resource
```

For example, a user with the **Contributor** role can manage resources within the assigned scope according to the permissions of that role.

---

# ARM and Azure Policy

Azure Policy can enforce organizational rules when resources are deployed or managed.

Example:

```text
User
  ↓
ARM Deployment
  ↓
Azure Policy
  ↓
Policy Evaluation
  │
  ├── Allowed → Resource Deployment
  │
  └── Denied  → Deployment Blocked
```

This allows organizations to enforce governance requirements.

---

# ARM Templates and Bicep

ARM supports infrastructure-as-code deployments using:

```text
ARM Templates
       or
Bicep
```

Example:

```text
Bicep
  ↓
ARM
  ↓
Resource Providers
  ↓
Azure Resources
```

ARM templates use JSON, while Bicep provides a simpler language for defining Azure infrastructure.

These are covered in detail in:

- **17.2 — ARM Templates**
- **17.3 — Bicep**

---

# ARM Deployment Modes

ARM deployments can use different approaches when deploying resources.

The commonly important concept is:

### Incremental

Adds or updates resources defined in the deployment while leaving other existing resources unchanged.

```text
Existing Resources
       +
Deployment
       ↓
Updated Infrastructure
```

### Complete

Historically, complete mode could remove resources in a resource group that were not defined in the deployment. Its behavior and availability depend on the deployment scope and current Azure tooling, so always verify current Microsoft documentation before relying on it for production deployments.

---

# ARM Locks

Resource locks can help prevent accidental deletion or modification of resources.

Examples:

```text
Resource
   │
   └── Delete Lock
           ↓
     Prevent Deletion
```

Resource locks are a governance feature and can be applied through Azure Resource Manager.

---

# ARM Benefits

### Consistent Management

Azure resources can be managed through:

- Azure Portal
- Azure CLI
- Azure PowerShell
- REST APIs
- ARM Templates
- Bicep

### Infrastructure as Code

Infrastructure can be defined in files and deployed repeatedly.

### Access Control

ARM integrates with Azure RBAC.

### Governance

ARM works with Azure Policy, locks, and other Azure management capabilities.

### Resource Organization

ARM manages resources within subscriptions and resource groups.

---

# Practical Example

Suppose you need to deploy:

```text
Resource Group
      │
      ├── VNet
      ├── Subnet
      ├── NSG
      ├── NIC
      └── VM
```

Instead of manually creating each resource, you can define the infrastructure using Bicep or an ARM template:

```text
Bicep / ARM Template
        ↓
Azure Resource Manager
        ↓
Resource Providers
        ↓
Multiple Azure Resources
```

This makes infrastructure deployment more consistent and repeatable.

---

# Key Points

- **Azure Resource Manager (ARM)** is Azure's management and deployment layer.
- ARM manages Azure resources through resource providers.
- Resources are organized within **resource groups and subscriptions**.
- **Resource providers** manage specific Azure resource types.
- ARM supports deployments through Azure Portal, CLI, PowerShell, APIs, ARM templates, and Bicep.
- ARM works with **Azure RBAC** for access control.
- ARM works with **Azure Policy** for governance.
- ARM supports infrastructure-as-code deployments.
- **ARM templates use JSON**, while **Bicep** provides a simpler language for Azure infrastructure.
- ARM is the foundation for consistent Azure resource management.

---

## Next

➡️ **17.2 — ARM Templates**
