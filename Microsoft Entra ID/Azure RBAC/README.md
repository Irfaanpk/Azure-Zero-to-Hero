# 5.6 Azure RBAC

**Azure Role-Based Access Control (Azure RBAC)** is the authorization system used to control access to Azure resources.

Azure RBAC determines **who can access an Azure resource, what they can do, and at what scope**.

---

## What is Azure RBAC?

Azure RBAC allows administrators to assign permissions to users, groups, service principals, and managed identities.

Example:

```text
User
 │
 ▼
Azure RBAC Role
 │
 ▼
Permissions
 │
 ▼
Azure Resource
```

Example:

```text
John
 │
 ▼
Virtual Machine Contributor
 │
 ▼
Manage Virtual Machines
 │
 ▼
Production VM
```

---

## Why Use Azure RBAC?

Azure RBAC helps organizations:

- Control access to Azure resources
- Grant only required permissions
- Assign permissions to users and groups
- Separate administrative responsibilities
- Manage access at different scopes
- Follow the principle of least privilege

---

# Azure RBAC Role

An Azure RBAC role is a collection of permissions that defines what actions an identity can perform on Azure resources.

Example:

```text
Role:
Reader

Permissions:
- View resources
- View resource configuration

No permissions to:
- Create resources
- Modify resources
- Delete resources
```

---

# Azure RBAC Role Types

Azure provides several built-in roles.

Common roles include:

| Role | Purpose |
|---|---|
| Owner | Full access to resources, including the ability to assign Azure RBAC roles |
| Contributor | Manage Azure resources but cannot assign Azure RBAC roles |
| Reader | View Azure resources without making changes |
| User Access Administrator | Manage user access to Azure resources |
| Virtual Machine Contributor | Manage virtual machines but not the virtual network or access permissions |
| Storage Account Contributor | Manage storage accounts but not access to the storage account |

---

# Owner

The **Owner** role provides full management access to Azure resources.

An Owner can:

- Create resources
- Modify resources
- Delete resources
- Assign Azure RBAC roles

Example:

```text
Owner
 │
 ├── Create Resource
 ├── Modify Resource
 ├── Delete Resource
 └── Assign RBAC Roles
```

---

# Contributor

The **Contributor** role allows users to manage Azure resources.

A Contributor can:

- Create resources
- Modify resources
- Delete resources

However, a Contributor cannot normally assign Azure RBAC roles.

Example:

```text
Contributor
 │
 ├── Create Resource
 ├── Modify Resource
 └── Delete Resource

Cannot:
└── Assign RBAC Roles
```

---

# Reader

The **Reader** role provides read-only access.

A Reader can:

- View resources
- View resource configuration
- View resource properties

A Reader cannot normally:

- Create resources
- Modify resources
- Delete resources

Example:

```text
Reader
 │
 └── View Resources
```

---

# User Access Administrator

The **User Access Administrator** role is used to manage access to Azure resources.

It can be used to:

- Assign Azure RBAC roles
- Remove Azure RBAC role assignments
- Manage access to Azure resources

Example:

```text
User Access Administrator
          │
          ▼
Manage RBAC Assignments
```

---

# Role Assignment

A role does not automatically give a user access to Azure resources.

The role must be **assigned** to an identity at a specific scope.

A role assignment contains three main components:

```text
Security Principal
        +
Role Definition
        +
Scope
        │
        ▼
Role Assignment
```

### Example

```text
Security Principal:
John

Role:
Reader

Scope:
Resource Group
```

This means John receives Reader permissions for that resource group.

---

# Security Principal

A **security principal** is the identity that receives the Azure RBAC role.

Common security principals include:

- User
- Group
- Service Principal
- Managed Identity

Example:

```text
User
John
  │
  ▼
Reader Role
```

Or:

```text
Group
Developers
  │
  ▼
Contributor Role
```

---

# Role Definition

The **role definition** describes the permissions included in a role.

Example:

```text
Reader
 │
 ├── Read Resources
 └── View Configuration
```

Another example:

```text
Contributor
 │
 ├── Read
 ├── Create
 ├── Update
 └── Delete
```

---

# Scope

The **scope** determines where the role assignment applies.

Azure RBAC supports different scopes:

```text
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

A role assigned at a higher scope can be inherited by resources under that scope.

Detailed scope concepts are covered in the **RBAC Scopes** section.

---

# Azure RBAC Example

Consider a development team:

```text
Group:
Developers

Role:
Contributor

Scope:
Development Resource Group
```

The members of the Developers group can manage resources within the assigned resource group according to the Contributor role.

```text
Developers Group
       │
       ▼
Contributor
       │
       ▼
Development Resource Group
       │
       ├── VM
       ├── Storage Account
       └── VNet
```

---

# Group-Based RBAC

Azure RBAC can be assigned to a group instead of individual users.

Example:

```text
Developers Group
 │
 ├── John
 ├── Sarah
 └── David
       │
       ▼
   Contributor
       │
       ▼
Resource Group
```

This makes access management easier when multiple users require the same permissions.

---

# Built-in Roles

Azure provides many predefined **built-in roles**.

Examples:

```text
Owner
Contributor
Reader
User Access Administrator
Virtual Machine Contributor
Storage Account Contributor
```

Built-in roles can be used directly when their permissions match the required access.

---

# Custom Roles

Azure also allows organizations to create **custom RBAC roles**.

A custom role is useful when the built-in roles do not provide the exact permissions required.

Example:

```text
Required Permissions:

Read Storage Accounts
Read Containers
List Containers
```

Instead of giving a user a broader role, an organization can create a custom role containing only the required permissions.

```text
Custom Role
     │
     ├── Read Storage Account
     ├── Read Containers
     └── List Containers
```

Custom roles can be created using the Azure Portal, Azure CLI, PowerShell, or ARM/Bicep depending on the deployment approach.

---

# Azure RBAC Permission Model

Azure RBAC permissions are based on allowed and excluded actions.

A simplified role definition looks like:

```text
Role
 │
 ├── Actions
 │      ├── Read
 │      ├── Write
 │      └── Delete
 │
 ├── NotActions
 │      └── Excluded Actions
 │
 ├── DataActions
 │      └── Data-level permissions
 │
 └── NotDataActions
        └── Excluded Data-level permissions
```

For AZ-104, understanding the difference between resource management permissions and data-level permissions is important.

---

# Management Plane vs Data Plane

Azure RBAC can control access to the Azure **management plane** and, for supported services, the **data plane**.

### Management Plane

Controls management of Azure resources.

Example:

```text
Create Storage Account
Delete Storage Account
Update Storage Account
```

### Data Plane

Controls access to data inside a resource.

Example:

```text
Read Blob
Write Blob
Delete Blob
```

Example:

```text
Management Plane
        │
        ▼
Storage Account
        │
        ▼
Configure Resource
```

```text
Data Plane
        │
        ▼
Blob Container
        │
        ▼
Access Blob Data
```

---

# 🧪 Lab: Assign Reader Role to a User

## Objective

Assign the **Reader** role to a test user at the resource group scope.

## Prerequisites

- Azure subscription
- Resource group
- Test Microsoft Entra user
- Permission to create role assignments

---

## Step 1: Open the Resource Group

1. Sign in to the **Azure Portal**.
2. Select **Resource groups**.
3. Open your test resource group.

---

## Step 2: Open Access Control

1. Select **Access control (IAM)**.
2. Select **Add**.
3. Select **Add role assignment**.

---

## Step 3: Select the Role

1. Search for **Reader**.
2. Select **Reader**.
3. Select **Next**.

---

## Step 4: Select the User

1. Under **Assign access to**, select **User, group, or service principal**.
2. Select **Select members**.
3. Search for your test user.
4. Select the user.
5. Select **Next**.

---

## Step 5: Assign the Role

1. Select **Review + assign**.
2. Review the configuration.
3. Select **Review + assign** again.

The user now has Reader access to the selected resource group.

---

## Step 6: Verify the Assignment

1. Open **Access control (IAM)**.
2. Select **Role assignments**.
3. Search for the test user.
4. Verify that the **Reader** role is assigned.

---

# 🧪 Lab: Assign Contributor Role to a Group

## Objective

Assign the **Contributor** role to a Microsoft Entra security group.

---

## Step 1: Open Resource Group IAM

1. Open your test resource group.
2. Select **Access control (IAM)**.
3. Select **Add**.
4. Select **Add role assignment**.

---

## Step 2: Select Contributor

1. Search for **Contributor**.
2. Select **Contributor**.
3. Select **Next**.

---

## Step 3: Select the Group

1. Select **User, group, or service principal**.
2. Select **Select members**.
3. Search for the required security group.
4. Select the group.
5. Select **Next**.

---

## Step 4: Assign

1. Select **Review + assign**.
2. Select **Review + assign** again.

The group now has Contributor access at the selected scope.

---

# 🧪 Lab: Remove an RBAC Role Assignment

## Step 1: Open Role Assignments

1. Open the resource group.
2. Select **Access control (IAM)**.
3. Select **Role assignments**.

---

## Step 2: Remove the Assignment

1. Find the test user or group.
2. Find the assigned role.
3. Select **Remove**.
4. Confirm the removal.

The identity no longer has that role assignment at the selected scope.

---

# Azure RBAC Access Evaluation

When a user tries to access an Azure resource, Azure evaluates the user's permissions based on their role assignments and applicable scopes.

Example:

```text
User
 │
 ▼
Role Assignments
 │
 ▼
Scope
 │
 ▼
Permissions
 │
 ▼
Azure Resource
```

If the user has the required permission, the requested operation can be allowed.

---

# Principle of Least Privilege

Azure RBAC should be assigned according to the user's actual requirements.

Example:

If a user only needs to view resources:

```text
User
 │
 ▼
Reader
 │
 ▼
Resource Group
```

There is usually no reason to assign:

```text
Global Administrator
```

or:

```text
Owner
```

when those permissions are not required.

---

# Key Points

- Azure RBAC controls access to Azure resources.
- RBAC is an **authorization** system.
- A role assignment consists of a **security principal, role definition, and scope**.
- Security principals can include users, groups, service principals, and managed identities.
- Common built-in roles include **Owner, Contributor, Reader, and User Access Administrator**.
- Owner can manage resources and assign RBAC roles.
- Contributor can manage resources but cannot normally assign RBAC roles.
- Reader provides read-only access.
- Groups can be used for group-based RBAC assignments.
- Azure provides built-in and custom roles.
- RBAC can control management-plane access and, for supported services, data-plane access.
- Always follow the **principle of least privilege**.
