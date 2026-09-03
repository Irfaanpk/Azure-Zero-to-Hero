# 17.3 Bicep

**Bicep** is a declarative infrastructure-as-code language developed by Microsoft for deploying Azure resources.

It provides a simpler and more readable alternative to writing ARM templates directly in JSON.

---

## What is Bicep?

Bicep allows you to define Azure infrastructure using `.bicep` files.

Basic workflow:

```text
Bicep File
    ↓
Bicep Compiler
    ↓
ARM Template
    ↓
Azure Resource Manager
    ↓
Azure Resources
```

Bicep does **not** replace Azure Resource Manager. It provides a simpler way to define infrastructure that is deployed through ARM.

---

# Why Use Bicep?

ARM templates use JSON, which can become difficult to read when infrastructure becomes large.

### ARM Template

```json
{
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2023-01-01",
  "name": "mystorageaccount",
  "location": "eastus"
}
```

### Bicep

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: 'mystorageaccount'
  location: 'eastus'
}
```

Bicep is generally:

- Easier to read
- Less verbose
- Easier to maintain
- Easier to reuse
- Designed specifically for Azure

---

# Bicep File

Bicep files use the:

```text
.bicep
```

extension.

Example:

```text
main.bicep
```

A Bicep file describes the desired Azure infrastructure.

```text
main.bicep
    ↓
Azure Resource Manager
    ↓
Azure Resources
```

---

# Bicep Resource Declaration

A resource is declared using the `resource` keyword.

Example:

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: 'mystorageaccount123'
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

The structure is:

```text
resource
    ↓
Resource Name
    ↓
Resource Type
    ↓
API Version
    ↓
Resource Configuration
```

---

# Resource Type

Example:

```bicep
'Microsoft.Storage/storageAccounts@2023-01-01'
```

Here:

```text
Microsoft.Storage
        ↓
Resource Provider

storageAccounts
        ↓
Resource Type

2023-01-01
        ↓
API Version
```

---

# Parameters

**Parameters** allow values to be supplied during deployment.

Example:

```bicep
param location string = 'eastus'

param storageAccountName string
```

The same Bicep file can then be used with different values.

```text
Development
    ↓
devstorage001

Production
    ↓
prodstorage001
```

This makes Bicep templates reusable.

---

# Variables

**Variables** store reusable values inside a Bicep file.

Example:

```bicep
var storageSku = 'Standard_LRS'
```

The variable can then be used by resources:

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: storageSku
  }
  kind: 'StorageV2'
}
```

---

# Resource Properties

Resources contain properties that define their configuration.

Example:

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: location

  sku: {
    name: 'Standard_LRS'
  }

  kind: 'StorageV2'
}
```

Different Azure resource types have different properties.

---

# Dependencies

Bicep can automatically determine dependencies when one resource references another.

Example:

```text
VNet
 ↓
Subnet
 ↓
NIC
 ↓
VM
```

If a resource directly references another resource, Bicep can infer the deployment dependency.

You can also explicitly define dependencies using:

```bicep
dependsOn: [
  resourceName
]
```

Explicit dependencies should generally be used only when Bicep cannot infer the dependency automatically.

---

# Outputs

**Outputs** return values from a deployment.

Example:

```bicep
output storageAccountName string = storageAccount.name
```

After deployment:

```text
Bicep Deployment
       ↓
     Outputs
       ↓
Storage Account Name
```

Outputs can be useful when you need to expose information about deployed resources.

---

# Modules

**Modules** allow large Bicep deployments to be divided into smaller reusable files.

Example:

```text
main.bicep
    │
    ├── network.bicep
    ├── storage.bicep
    └── virtual-machine.bicep
```

Workflow:

```text
main.bicep
    ↓
Modules
    ↓
Azure Resources
```

Modules help organize larger infrastructure projects.

---

# Bicep Expressions and Functions

Bicep provides expressions and functions for dynamically generating values.

Common examples include:

| Function | Purpose |
|---|---|
| `resourceGroup()` | Access resource group information |
| `subscription()` | Access subscription information |
| `resourceId()` | Generate a resource ID |
| `uniqueString()` | Generate a deterministic unique string |
| `format()` | Format strings |
| `concat()` | Combine strings |

Example:

```bicep
var storageName = 'storage${uniqueString(resourceGroup().id)}'
```

---

# Existing Resources

Bicep can reference resources that already exist without deploying them.

Example:

```bicep
resource existingVnet 'Microsoft.Network/virtualNetworks@2024-01-01' existing = {
  name: 'myVNet'
}
```

The `existing` keyword tells Bicep that the resource already exists.

```text
Existing Azure Resource
        ↓
      Bicep
        ↓
Reference Resource
```

---

# Bicep Deployment

Bicep files can be deployed using:

- Azure Portal
- Azure CLI
- Azure PowerShell
- CI/CD pipelines

Example using Azure CLI:

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file main.bicep
```

With parameters:

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file main.bicep \
  --parameters storageAccountName=mystorageaccount123
```

---

# Validate a Bicep File

You can validate a deployment before creating resources.

```bash
az deployment group validate \
  --resource-group myResourceGroup \
  --template-file main.bicep
```

This helps identify configuration and template problems before deployment.

---

# What Happens During Deployment?

```text
main.bicep
    ↓
Bicep Processing
    ↓
ARM Template Representation
    ↓
Azure Resource Manager
    ↓
Resource Providers
    ↓
Azure Resources
```

Bicep is therefore integrated directly with the Azure Resource Manager deployment model.

---

# Bicep Example

The following Bicep file creates a storage account:

```bicep
param storageAccountName string
param location string = resourceGroup().location

var storageSku = 'Standard_LRS'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: storageSku
  }
  kind: 'StorageV2'
}

output storageAccountId string = storageAccount.id
```

---

# Practical Lab

## Lab — Deploy Azure Resources Using Bicep

### Objective

Create a Bicep file that deploys an Azure Storage Account and deploy it using Azure CLI.

### Steps

1. Create a resource group.

```bash
az group create \
  --name bicep-lab-rg \
  --location eastus
```

2. Create a file:

```text
main.bicep
```

3. Add the Bicep storage account configuration.

4. Validate the Bicep deployment:

```bash
az deployment group validate \
  --resource-group bicep-lab-rg \
  --template-file main.bicep \
  --parameters storageAccountName=mystorageaccount123
```

5. Deploy the Bicep file:

```bash
az deployment group create \
  --resource-group bicep-lab-rg \
  --template-file main.bicep \
  --parameters storageAccountName=mystorageaccount123
```

6. Verify the deployed resource:

```bash
az resource list \
  --resource-group bicep-lab-rg \
  --output table
```

7. Check the deployment in the Azure Portal:

```text
Resource Group
    ↓
Deployments
    ↓
Bicep Deployment
```

8. Verify the storage account.

9. Delete the resource group after completing the lab:

```bash
az group delete \
  --name bicep-lab-rg \
  --yes
```

---

# Bicep vs ARM Templates

| Bicep | ARM Templates |
|---|---|
| Bicep language | JSON |
| `.bicep` file | `.json` file |
| Less verbose | More verbose |
| Easier to read | More difficult to read |
| Supports modules | Supports nested/linked templates |
| Compiles to ARM template representation | Native ARM deployment format |
| Designed specifically for Azure | Native Azure deployment format |

---

# Bicep vs Azure CLI

| Bicep | Azure CLI |
|---|---|
| Declarative | Primarily command-based |
| Defines desired infrastructure | Executes commands |
| Good for infrastructure as code | Good for individual operations and scripting |
| Reusable templates | Command/script based |
| Supports resource dependencies | Commands execute in sequence |

Example:

```text
Bicep
  ↓
Define desired state
  ↓
Deploy infrastructure
```

Whereas:

```text
Azure CLI
  ↓
Run commands
  ↓
Create / modify resources
```

---

# Key Points

- **Bicep** is Microsoft's declarative infrastructure-as-code language for Azure.
- Bicep uses `.bicep` files.
- Bicep is simpler and less verbose than ARM template JSON.
- Bicep deployments are processed through **Azure Resource Manager**.
- `resource` defines Azure resources.
- `param` defines deployment parameters.
- `var` defines reusable variables.
- `output` returns deployment values.
- Bicep supports modules for reusable infrastructure components.
- Bicep can automatically determine many resource dependencies.
- The `existing` keyword allows Bicep to reference existing resources.
- Bicep can be deployed using Azure CLI, PowerShell, Portal, and CI/CD pipelines.
- Bicep is compiled into an ARM-compatible deployment representation.

---

## Next

➡️ **17.4 — ARM Templates vs Bicep**
