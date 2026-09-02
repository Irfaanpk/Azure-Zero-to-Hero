# 5.8 Managed Identities

**Managed Identities** in Microsoft Entra ID provide an identity for Azure resources so that they can authenticate to other Azure services without storing usernames, passwords, or credentials in code.

---

## What is a Managed Identity?

A **managed identity** is an identity automatically managed by Azure and associated with an Azure resource.

It allows an Azure resource to authenticate to services that support Microsoft Entra authentication.

Example:

```text
Azure Virtual Machine
        │
        ▼
Managed Identity
        │
        ▼
Microsoft Entra ID
        │
        ▼
Azure Storage
```

The VM can authenticate to the storage service without storing a storage account key or password inside the VM.

---

## Why Use Managed Identities?

Managed identities help:

- Avoid storing credentials in application code
- Avoid manually managing passwords and secrets
- Authenticate Azure resources to other Azure services
- Use Microsoft Entra ID for service authentication
- Simplify credential management

Example without managed identity:

```text
Application
    │
    ├── Username
    ├── Password
    └── Secret
          │
          ▼
      Azure Storage
```

Example with managed identity:

```text
Application
    │
    ▼
Managed Identity
    │
    ▼
Microsoft Entra ID
    │
    ▼
Azure Storage
```

---

# Types of Managed Identities

Azure provides two types of managed identities:

1. **System-assigned managed identity**
2. **User-assigned managed identity**

---

# System-Assigned Managed Identity

A **system-assigned managed identity** is created directly on an Azure resource.

The identity has the same lifecycle as the resource.

Example:

```text
Virtual Machine
      │
      ▼
System-Assigned Identity
```

If the Azure resource is deleted, its system-assigned managed identity is also deleted.

---

## System-Assigned Identity Example

```text
VM
 │
 └── System-Assigned Managed Identity
              │
              ▼
        Microsoft Entra ID
              │
              ▼
        Storage Account
```

The managed identity can be assigned an appropriate Azure RBAC role on the target resource.

---

# User-Assigned Managed Identity

A **user-assigned managed identity** is created as a separate Azure resource.

It can then be assigned to one or more Azure resources.

Example:

```text
User-Assigned Managed Identity
          │
          ├── VM 1
          ├── VM 2
          └── App Service
```

The identity exists independently of the resources using it.

If one resource is deleted, the user-assigned managed identity remains available.

---

# System-Assigned vs User-Assigned

| Feature | System-Assigned | User-Assigned |
|---|---|---|
| Created with Azure resource | Yes | No |
| Separate Azure resource | No | Yes |
| Can be assigned to multiple resources | No | Yes |
| Lifecycle | Tied to resource | Independent |
| Deleted with resource | Yes | No |
| Reusable | No | Yes |

---

# Managed Identity and Azure RBAC

A managed identity can be assigned an Azure RBAC role on an Azure resource.

Example:

```text
Virtual Machine
      │
      ▼
Managed Identity
      │
      ▼
Storage Blob Data Reader
      │
      ▼
Storage Account
```

The managed identity can access the target resource according to the permissions provided by the assigned role.

---

# Example: Virtual Machine Accessing Storage

Suppose an application running on a VM needs to read files from Azure Blob Storage.

Without managed identity:

```text
Application
     │
     ├── Storage Account Key
     │
     ▼
Azure Storage
```

The application must store and manage a credential.

With managed identity:

```text
Virtual Machine
       │
       ▼
Managed Identity
       │
       ▼
Microsoft Entra ID
       │
       ▼
Azure RBAC
       │
       ▼
Blob Storage
```

The application does not need to store a storage account key.

---

# Managed Identity Authentication Flow

A simplified authentication flow looks like this:

```text
Azure Resource
      │
      ▼
Managed Identity
      │
      ▼
Microsoft Entra ID
      │
      ▼
Authentication Token
      │
      ▼
Target Azure Service
      │
      ▼
Access Based on RBAC
```

The managed identity obtains a Microsoft Entra token and uses it to authenticate to the target service.

---

# 🧪 Lab: Enable System-Assigned Managed Identity

## Objective

Enable a system-assigned managed identity on an Azure Virtual Machine.

## Prerequisites

- Azure subscription
- Azure Virtual Machine
- Permission to modify the VM
- Permission to create role assignments

---

## Step 1: Open the Virtual Machine

1. Sign in to the **Azure Portal**.
2. Select **Virtual machines**.
3. Open your test virtual machine.

---

## Step 2: Open Identity

1. In the VM menu, select **Identity**.
2. Open the **System assigned** tab.
3. Set the status to **On**.

---

## Step 3: Save

1. Select **Save**.
2. Confirm the operation if prompted.

Azure creates a system-assigned managed identity for the VM.

---

## Step 4: Verify the Identity

After the identity is created, Azure displays an **Object (principal) ID**.

Example:

```text
System assigned:
On

Principal ID:
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

This identity can now be used for Azure resource access.

---

# 🧪 Lab: Create a User-Assigned Managed Identity

## Objective

Create a user-assigned managed identity and use it with an Azure resource.

---

## Step 1: Open Managed Identities

1. Sign in to the **Azure Portal**.
2. Search for **Managed Identities**.
3. Open **Managed Identities**.
4. Select **Create**.

---

## Step 2: Configure the Identity

Select your subscription and resource group.

Example:

```text
Subscription:
Azure Subscription

Resource Group:
Identity-Lab-RG

Region:
Your Azure Region

Name:
my-user-identity
```

---

## Step 3: Create the Identity

1. Select **Review + create**.
2. Review the configuration.
3. Select **Create**.

The user-assigned managed identity is now created as an Azure resource.

---

# 🧪 Lab: Assign a User-Assigned Identity to a VM

## Step 1: Open the VM

1. Open **Virtual machines**.
2. Select your test VM.
3. Select **Identity**.

---

## Step 2: Open User Assigned

1. Select the **User assigned** tab.
2. Select **Add**.
3. Select the user-assigned managed identity.
4. Select **Add**.

The managed identity is now associated with the VM.

---

# 🧪 Lab: Assign RBAC Role to a Managed Identity

## Objective

Give a managed identity permission to access an Azure Storage Account.

---

## Step 1: Open the Storage Account

1. Open **Storage accounts**.
2. Select your test storage account.
3. Select **Access control (IAM)**.

---

## Step 2: Add Role Assignment

1. Select **Add**.
2. Select **Add role assignment**.
3. Select an appropriate role.

For example:

```text
Storage Blob Data Reader
```

4. Select **Next**.

---

## Step 3: Select Managed Identity

1. Under **Assign access to**, select **Managed identity**.
2. Select **Select members**.
3. Select the appropriate managed identity.
4. Select **Next**.

---

## Step 4: Assign the Role

1. Select **Review + assign**.
2. Select **Review + assign** again.

The managed identity now has the selected permissions on the storage account.

---

# Managed Identity vs Service Principal

Both managed identities and service principals can represent applications or services, but managed identities are designed to have Azure manage their credentials.

| Feature | Managed Identity | Service Principal |
|---|---|---|
| Identity managed by Azure | Yes | No |
| Credential management | Azure-managed | Application/admin-managed |
| Password/secret storage | Not required | Often required |
| Azure resource integration | Built-in | Requires configuration |
| Can use Azure RBAC | Yes | Yes |

For Azure resources, managed identities can simplify authentication because credentials do not need to be stored and rotated manually.

---

# Key Points

- Managed identities provide identities for Azure resources.
- They use Microsoft Entra ID for authentication.
- They eliminate the need to store credentials in application code.
- There are two types:
  - **System-assigned**
  - **User-assigned**
- System-assigned identities are tied to the lifecycle of an Azure resource.
- User-assigned identities are independent resources and can be associated with multiple resources.
- Managed identities can receive Azure RBAC roles.
- Managed identities can be used to access supported Azure services securely.
- Managed identities are useful for applications running on Azure resources that need to access other Azure resources.
