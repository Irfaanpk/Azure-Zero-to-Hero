# 17.4 ARM Templates vs Bicep

**ARM Templates** and **Bicep** are both used to deploy Azure infrastructure using Infrastructure as Code (IaC).

Bicep provides a simpler and more readable way to define Azure resources, while ARM templates use JSON directly.

---

## Basic Difference

```text
ARM Template
     │
     └── JSON-based

Bicep
     │
     └── Bicep language
             ↓
        ARM deployment
```

Both ultimately use **Azure Resource Manager** to deploy Azure resources.

---

# ARM Templates

ARM templates are JSON files that define Azure infrastructure.

Example:

```json
{
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2023-01-01",
  "name": "mystorageaccount",
  "location": "eastus",
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "StorageV2"
}
```

ARM templates are native to the Azure Resource Manager deployment model.

---

# Bicep

Bicep is a dedicated infrastructure-as-code language for Azure.

Example:

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: 'mystorageaccount'
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

The same resource definition is generally much shorter and easier to read in Bicep.

---

# Main Differences

| Feature | ARM Templates | Bicep |
|---|---|---|
| Format | JSON | Bicep |
| File extension | `.json` | `.bicep` |
| Readability | More verbose | More readable |
| Syntax | JSON syntax | Bicep syntax |
| Azure integration | Native | Native |
| Parameters | Supported | Supported |
| Variables | Supported | Supported |
| Outputs | Supported | Supported |
| Dependencies | Supported | Supported |
| Modules | Linked/nested templates | Native modules |
| Infrastructure as Code | Yes | Yes |
| Deployment through ARM | Yes | Yes |

---

# Readability

One of the biggest advantages of Bicep is readability.

### ARM Template

```json
{
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2023-01-01",
  "name": "[parameters('storageAccountName')]",
  "location": "[resourceGroup().location]",
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "StorageV2"
}
```

### Bicep

```bicep
param storageAccountName string

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

Bicep removes much of the JSON structure and makes the infrastructure definition easier to understand.

---

# Parameters

Both technologies support parameters.

### ARM

```json
"parameters": {
  "location": {
    "type": "string"
  }
}
```

### Bicep

```bicep
param location string
```

Bicep uses simpler syntax for defining parameters.

---

# Variables

Both support variables.

### ARM

```json
"variables": {
  "storageSku": "Standard_LRS"
}
```

### Bicep

```bicep
var storageSku = 'Standard_LRS'
```

---

# Outputs

Both support deployment outputs.

### ARM

```json
"outputs": {
  "storageAccountName": {
    "type": "string",
    "value": "[parameters('storageAccountName')]"
  }
}
```

### Bicep

```bicep
output storageAccountName string = storageAccount.name
```

---

# Dependencies

Both ARM templates and Bicep support resource dependencies.

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

Bicep can automatically determine dependencies when resources reference each other.

```bicep
resource vnet 'Microsoft.Network/virtualNetworks@2024-01-01' = {
  ...
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2024-01-01' = {
  parent: vnet
  ...
}
```

This makes resource relationships easier to express.

---

# Modules

Bicep provides a built-in **module** system for breaking infrastructure into reusable components.

Example:

```text
main.bicep
    │
    ├── network.bicep
    ├── storage.bicep
    └── compute.bicep
```

This makes larger infrastructure projects easier to organize.

ARM templates can also use linked or nested templates, but Bicep modules provide a cleaner development experience.

---

# Deployment

Both can be deployed using Azure CLI.

### ARM Template

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file template.json
```

### Bicep

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file main.bicep
```

The deployment is handled by Azure Resource Manager.

```text
ARM Template / Bicep
        ↓
Azure Resource Manager
        ↓
Resource Providers
        ↓
Azure Resources
```

---

# Bicep and ARM Relationship

Bicep is **not a separate deployment engine**.

The relationship is:

```text
Bicep
  ↓
ARM-compatible deployment representation
  ↓
Azure Resource Manager
  ↓
Azure Resources
```

ARM templates are directly expressed in JSON.

---

# When to Use ARM Templates

ARM templates can be useful when:

- Working with existing ARM template JSON
- Maintaining legacy infrastructure
- A project already uses ARM templates
- You specifically need JSON-based ARM templates

---

# When to Use Bicep

Bicep is generally preferred for new Azure Infrastructure-as-Code projects because it provides:

- Cleaner syntax
- Better readability
- Less repetitive code
- Native Azure resource support
- Modules
- Easier maintenance

Example:

```text
New Azure IaC Project
        ↓
      Bicep
        ↓
Azure Resource Manager
```

---

# ARM Templates vs Bicep vs Azure CLI

These tools have different purposes.

| ARM Templates | Bicep | Azure CLI |
|---|---|---|
| Declarative | Declarative | Command-based |
| JSON | Bicep language | Shell commands |
| Defines desired state | Defines desired state | Executes operations |
| Infrastructure as Code | Infrastructure as Code | Automation / management |
| More verbose | More concise | Command-oriented |

Example:

```text
Bicep
  ↓
Define infrastructure
  ↓
Deploy


Azure CLI
  ↓
Execute commands
  ↓
Create / modify resources
```

---

# Which Should You Learn for AZ-104?

For AZ-104, understand **both ARM templates and Bicep**, but focus more on Bicep for practical Infrastructure-as-Code work.

### ARM Templates

Understand:

- JSON structure
- Parameters
- Variables
- Resources
- Outputs
- Dependencies
- Deployment

### Bicep

Understand:

- Resource declarations
- Parameters
- Variables
- Outputs
- Dependencies
- Modules
- Existing resources
- Deployment

---

# Practical Lab

No separate lab is required for this comparison topic.

The practical deployment work is already covered in:

- **17.2 — ARM Templates**
- **17.3 — Bicep**

---

# Key Points

- **ARM Templates** use JSON to define Azure infrastructure.
- **Bicep** is a simpler declarative language designed specifically for Azure.
- Both use **Azure Resource Manager** for deployment.
- Bicep is generally easier to read and maintain.
- Both support parameters, variables, resources, outputs, and dependencies.
- Bicep provides native modules for reusable infrastructure components.
- Bicep does not replace ARM; it provides a simpler authoring experience for Azure deployments.
- For new Azure Infrastructure-as-Code projects, **Bicep is generally the preferred choice**.
