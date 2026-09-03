# 17.2 ARM Templates

**Azure Resource Manager (ARM) templates** are JSON files that define Azure infrastructure and resources using a **declarative infrastructure-as-code approach**.

Instead of creating resources manually through the Azure Portal, you can define the required infrastructure in a template and deploy it through Azure Resource Manager.

---

## What is an ARM Template?

An ARM template is a JSON file that describes:

- Resources to deploy
- Resource configuration
- Parameters
- Variables
- Dependencies
- Outputs

Basic workflow:

```text
ARM Template
     ↓
Azure Resource Manager
     ↓
Resource Providers
     ↓
Azure Resources
```

---

# Declarative Infrastructure

ARM templates describe the **desired state** of your infrastructure.

For example:

```text
Desired Infrastructure

Resource Group
     │
     ├── Storage Account
     ├── Virtual Network
     └── Virtual Machine
```

You define this infrastructure in the template:

```text
ARM Template
     ↓
Azure Resource Manager
     ↓
Infrastructure Created
```

You don't need to manually execute every individual resource creation command.

---

# ARM Template Structure

A typical ARM template contains these main sections:

```json
{
  "$schema": "...",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {}
}
```

The important sections are:

| Section | Purpose |
|---|---|
| `$schema` | Defines the template schema |
| `contentVersion` | Version of the template |
| `parameters` | Values provided during deployment |
| `variables` | Reusable values inside the template |
| `resources` | Azure resources to deploy |
| `outputs` | Values returned after deployment |

---

# 1. `$schema`

The `$schema` property identifies the ARM template schema.

Example:

```json
"$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#"
```

It helps tools understand the structure and syntax of the template.

---

# 2. contentVersion

`contentVersion` identifies the version of the template.

Example:

```json
"contentVersion": "1.0.0.0"
```

You can update this value when the template structure changes.

---

# 3. Parameters

**Parameters** allow values to be provided when the template is deployed.

Example:

```json
"parameters": {
  "storageAccountName": {
    "type": "string"
  }
}
```

During deployment:

```text
ARM Template
     │
     ├── storageAccountName
     │
     ▼
User provides value
     │
     ▼
Resource Created
```

Parameters make templates reusable.

For example, the same template can be used for:

```text
Development
     ↓
devstorage001

Testing
     ↓
teststorage001

Production
     ↓
prodstorage001
```

---

# 4. Variables

**Variables** store reusable values inside the template.

Example:

```json
"variables": {
  "storageSku": "Standard_LRS"
}
```

A variable can be referenced elsewhere in the template.

```text
Variable
   ↓
storageSku
   ↓
Standard_LRS
```

Variables help avoid repeating values.

---

# 5. Resources

The `resources` section defines the Azure resources that should be created or configured.

Example:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2023-01-01",
    "name": "[parameters('storageAccountName')]",
    "location": "[resourceGroup().location]",
    "sku": {
      "name": "[variables('storageSku')]"
    },
    "kind": "StorageV2"
  }
]
```

Important properties include:

- `type`
- `apiVersion`
- `name`
- `location`
- Resource-specific properties

---

# Resource Type

The `type` property identifies which Azure resource is being deployed.

Example:

```json
"type": "Microsoft.Storage/storageAccounts"
```

Another example:

```json
"type": "Microsoft.Network/virtualNetworks"
```

The format is:

```text
Resource Provider / Resource Type
```

---

# API Version

The `apiVersion` specifies the Azure resource API version used by the template.

Example:

```json
"apiVersion": "2023-01-01"
```

Different resource types support different API versions.

---

# Resource Dependencies

Some Azure resources depend on other resources.

For example:

```text
VNet
 ↓
Subnet
 ↓
NIC
 ↓
VM
```

ARM can determine dependencies from resource references.

You can also explicitly specify a dependency using `dependsOn`.

Example:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/virtualNetworks', 'myVNet')]"
]
```

This tells ARM that the resource should be deployed after the specified resource.

---

# ARM Template Functions

ARM templates provide functions for dynamically generating values.

Common functions include:

| Function | Purpose |
|---|---|
| `parameters()` | Access parameter values |
| `variables()` | Access variables |
| `resourceGroup()` | Get resource group information |
| `subscription()` | Get subscription information |
| `resourceId()` | Generate a resource ID |
| `concat()` | Combine strings |
| `format()` | Format strings |
| `uniqueString()` | Generate a deterministic unique string |

Example:

```json
"name": "[parameters('storageAccountName')]"
```

---

# Outputs

The `outputs` section returns values after deployment.

Example:

```json
"outputs": {
  "storageAccountName": {
    "type": "string",
    "value": "[parameters('storageAccountName')]"
  }
}
```

After deployment:

```text
ARM Deployment
      ↓
Outputs
      ↓
Storage Account Name
```

Outputs can be useful when another deployment or user needs information about the deployed resources.

---

# Complete ARM Template Example

The following template creates a basic storage account:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",

  "parameters": {
    "storageAccountName": {
      "type": "string"
    }
  },

  "variables": {
    "storageSku": "Standard_LRS"
  },

  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-01-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[variables('storageSku')]"
      },
      "kind": "StorageV2"
    }
  ],

  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[parameters('storageAccountName')]"
    }
  }
}
```

---

# Deploying an ARM Template

ARM templates can be deployed using:

- Azure Portal
- Azure CLI
- Azure PowerShell
- Azure REST APIs
- CI/CD pipelines

Example using Azure CLI:

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file template.json
```

With a parameter:

```bash
az deployment group create \
  --resource-group myResourceGroup \
  --template-file template.json \
  --parameters storageAccountName=mystorageaccount123
```

---

# Template Deployment Workflow

```text
Create ARM Template
        ↓
Define Parameters
        ↓
Define Variables
        ↓
Define Resources
        ↓
Validate Template
        ↓
Deploy Template
        ↓
Azure Resource Manager
        ↓
Resources Created / Updated
        ↓
Review Outputs
```

---

# Incremental Deployment

ARM templates can be deployed using **incremental deployment mode**.

In incremental mode:

```text
Existing Resources
       +
Resources in Template
       ↓
Resources Created / Updated
```

Resources that are not defined in the template are not automatically removed simply because they are absent from the template.

Example:

```text
Resource Group

Existing:
├── VM
├── Storage Account
└── VNet

Template:
├── VM
└── VNet

Incremental Deployment
        ↓

VM       → Updated if required
VNet     → Updated if required
Storage  → Remains
```

---

# Complete Deployment Mode

ARM also supports **complete deployment mode** in applicable deployment scenarios.

Historically, complete mode could remove resources from a resource group that were not included in the template.

Because behavior and availability can vary by deployment scope and current Azure tooling, verify the current Microsoft documentation before using complete mode in production.

For normal learning and day-to-day ARM deployments, **incremental mode** is the important concept to understand.

---

# ARM Template Validation

Before deploying infrastructure, you can validate the template.

Example:

```bash
az deployment group validate \
  --resource-group myResourceGroup \
  --template-file template.json
```

Validation can help identify template or configuration problems before creating resources.

---

# ARM Template Advantages

### Repeatable Deployments

The same template can be deployed multiple times.

```text
One Template
     │
     ├── Development
     ├── Testing
     └── Production
```

### Consistency

Resources can be deployed using the same configuration.

### Infrastructure as Code

Infrastructure is stored as code and can be managed using source control.

### Automation

Templates can be deployed through scripts and CI/CD pipelines.

### Dependency Management

ARM can manage relationships and dependencies between resources.

---

# ARM Templates vs Azure Portal

| Azure Portal | ARM Template |
|---|---|
| Manual deployment | Automated deployment |
| GUI-based | Code-based |
| More repetitive | Repeatable |
| Harder to standardize | Easy to standardize |
| Configuration entered manually | Configuration defined in code |

---

# ARM Templates vs Azure CLI

| Azure CLI | ARM Template |
|---|---|
| Imperative commands | Declarative configuration |
| Executes commands | Defines desired infrastructure |
| Good for individual operations | Good for repeatable deployments |
| Script-based | Template-based |

Example CLI approach:

```bash
az storage account create ...
az network vnet create ...
az vm create ...
```

ARM approach:

```text
ARM Template
     ↓
Define all required resources
     ↓
Single deployment
```

---

# ARM Templates vs Bicep

| ARM Templates | Bicep |
|---|---|
| JSON-based | Bicep language |
| More verbose | More concise |
| Native ARM template format | Compiles to ARM templates |
| More difficult to read | Easier to read |
| Azure-native | Azure-native |

Example:

```text
Bicep
  ↓
Compilation
  ↓
ARM Template
  ↓
Azure Resource Manager
  ↓
Azure Resources
```

Bicep is covered in detail in **17.3 — Bicep**.

---

# Practical Lab

## Lab — Deploy an Azure Storage Account Using an ARM Template

### Objective

Create an ARM template that deploys a storage account and deploy it using Azure CLI.

### Steps

1. Create a resource group.

```bash
az group create \
  --name arm-lab-rg \
  --location eastus
```

2. Create a file named:

```text
template.json
```

3. Add an ARM template that defines a storage account.

4. Validate the template:

```bash
az deployment group validate \
  --resource-group arm-lab-rg \
  --template-file template.json
```

5. Deploy the template:

```bash
az deployment group create \
  --resource-group arm-lab-rg \
  --template-file template.json
```

6. Verify the deployment:

```bash
az resource list \
  --resource-group arm-lab-rg \
  --output table
```

7. Verify the storage account in the Azure Portal.

8. Review the deployment under:

```text
Resource Group
    ↓
Deployments
    ↓
ARM Deployment
```

9. Delete the resource group after completing the lab:

```bash
az group delete \
  --name arm-lab-rg \
  --yes
```

---

# Key Points

- **ARM templates** define Azure infrastructure using JSON.
- ARM templates use a **declarative** approach.
- Main sections include `parameters`, `variables`, `resources`, and `outputs`.
- Parameters make templates reusable.
- Variables store reusable values.
- Resources define what Azure resources should be deployed.
- `dependsOn` can define explicit resource dependencies.
- ARM template functions dynamically generate values.
- ARM templates can be deployed through Portal, CLI, PowerShell, APIs, and CI/CD pipelines.
- **Incremental deployment** is the commonly used deployment mode.
- ARM templates provide repeatable and consistent infrastructure deployment.
- **Bicep** provides a simpler way to define Azure infrastructure and compiles to ARM template JSON.

---

## Next

➡️ **17.3 — Bicep**
