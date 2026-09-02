# 5.7 RBAC Scopes

**RBAC scope** determines where an Azure RBAC role assignment applies.

Azure provides four main RBAC scopes:

- Management Group
- Subscription
- Resource Group
- Resource

---

## What is RBAC Scope?

When assigning an Azure RBAC role, you must specify the scope where the permissions should apply.

Example:

```text
User
 │
 ▼
Reader Role
 │
 ▼
Resource Group Scope
```

This means the user receives Reader access within that resource group.

---

# Azure RBAC Scope Hierarchy

Azure RBAC scopes follow a hierarchy:

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

A role assignment at a higher scope can be inherited by resources below that scope.

---

# 1. Management Group Scope

A **Management Group** is used to organize multiple Azure subscriptions.

RBAC assignments at the management group scope can apply to subscriptions and resources within that management group.

Example:

```text
Management Group
 │
 ├── Production Subscription
 │
 ├── Development Subscription
 │
 └── Testing Subscription
```

If a role is assigned at the management group level, the permissions can be inherited by the subscriptions and resources underneath it.

---

# 2. Subscription Scope

A **Subscription** contains Azure resources and resource groups.

RBAC assignments at the subscription scope can apply to the resource groups and resources within that subscription.

Example:

```text
Subscription
 │
 ├── Resource Group 1
 │      ├── VM
 │      └── Storage Account
 │
 └── Resource Group 2
        ├── VNet
        └── Database
```

If a user receives the **Reader** role at the subscription scope, the user can read resources within that subscription according to the role permissions.

---

# 3. Resource Group Scope

A **Resource Group** contains related Azure resources.

RBAC assignments at the resource group scope apply to resources inside that resource group.

Example:

```text
Resource Group
 │
 ├── Virtual Machine
 ├── Storage Account
 ├── VNet
 └── Public IP
```

Example role assignment:

```text
User:
John

Role:
Contributor

Scope:
Development Resource Group
```

John can manage resources within that resource group according to the Contributor role.

---

# 4. Resource Scope

RBAC can also be assigned directly to an individual Azure resource.

Example:

```text
Storage Account
```

Role assignment:

```text
User:
John

Role:
Reader

Scope:
Storage Account
```

John receives Reader access only to that specific storage account.

---

# Scope Inheritance

Permissions assigned at a higher scope are inherited by lower scopes.

Example:

```text
Subscription
 │
 │  Reader Role
 │
 ├── Resource Group 1
 │      │
 │      ├── VM
 │      └── Storage Account
 │
 └── Resource Group 2
        │
        ├── VNet
        └── Database
```

A Reader assignment at the subscription level can provide read access to the resources under that subscription.

---

# Scope Example

Consider the following environment:

```text
Subscription
 │
 ├── Production-RG
 │      ├── VM
 │      └── Storage Account
 │
 └── Development-RG
        ├── VM
        └── Storage Account
```

### Assignment 1

```text
Role:
Reader

Scope:
Subscription
```

The user can read resources across the subscription.

### Assignment 2

```text
Role:
Contributor

Scope:
Development-RG
```

The user can manage resources within the development resource group.

### Assignment 3

```text
Role:
Reader

Scope:
Production VM
```

The user receives Reader access only to that specific VM.

---

# Choosing the Correct Scope

The scope should match the required level of access.

| Requirement | Recommended Scope |
|---|---|
| Access to many subscriptions | Management Group |
| Access to an entire subscription | Subscription |
| Access to an application environment | Resource Group |
| Access to one specific resource | Resource |

Example:

```text
Need access to everything
        │
        ▼
Subscription

Need access to one application
        │
        ▼
Resource Group

Need access to one resource
        │
        ▼
Resource
```

---

# Least Privilege and Scope

RBAC scope is important for implementing the **principle of least privilege**.

If a user only needs access to one storage account, assigning a role at the subscription level may provide more access than necessary.

Instead:

```text
User
 │
 ▼
Reader
 │
 ▼
Storage Account
```

is more restrictive than:

```text
User
 │
 ▼
Reader
 │
 ▼
Subscription
```

Use the smallest practical scope that satisfies the user's requirements.

---

# 🧪 Lab: Assign RBAC at Resource Scope

## Objective

Assign a Reader role to a user for a single Azure resource.

## Prerequisites

- Azure subscription
- Azure resource
- Test Microsoft Entra user
- Permission to create role assignments

---

## Step 1: Open the Resource

1. Sign in to the **Azure Portal**.
2. Open the resource you want to use for the lab.
3. Select **Access control (IAM)**.

---

## Step 2: Add Role Assignment

1. Select **Add**.
2. Select **Add role assignment**.
3. Search for **Reader**.
4. Select **Reader**.
5. Select **Next**.

---

## Step 3: Select the User

1. Select **User, group, or service principal**.
2. Select **Select members**.
3. Search for your test user.
4. Select the user.
5. Select **Next**.

---

## Step 4: Assign the Role

1. Select **Review + assign**.
2. Review the configuration.
3. Select **Review + assign** again.

The user now has Reader access to the selected resource.

---

## Step 5: Verify the Scope

1. Open **Access control (IAM)**.
2. Select **Role assignments**.
3. Find the test user.
4. Verify that the role is assigned at the resource scope.

---

# 🧪 Lab: Assign RBAC at Resource Group Scope

## Objective

Assign a Contributor role to a user at the resource group scope.

---

## Step 1: Open Resource Group

1. Open **Resource groups**.
2. Select your test resource group.
3. Select **Access control (IAM)**.

---

## Step 2: Add Role Assignment

1. Select **Add**.
2. Select **Add role assignment**.
3. Search for **Contributor**.
4. Select **Contributor**.
5. Select **Next**.

---

## Step 3: Select User

1. Select **User, group, or service principal**.
2. Select **Select members**.
3. Select your test user.
4. Select **Next**.

---

## Step 4: Assign

1. Select **Review + assign**.
2. Select **Review + assign** again.

The user now has Contributor access to the resource group and resources under it, according to the role permissions.

---

# 🧪 Lab: Compare Resource Group and Resource Scope

Create the following assignments:

```text
User:
John

Assignment 1:
Reader → Production Resource Group

Assignment 2:
Contributor → Development VM
```

The first assignment provides Reader access within the resource group.

The second assignment provides Contributor access only to the selected VM.

This demonstrates how changing the scope changes where the permissions apply.

---

# Scope and Multiple Role Assignments

A user can have multiple role assignments at different scopes.

Example:

```text
John
 │
 ├── Reader
 │     └── Subscription
 │
 └── Contributor
       └── Development Resource Group
```

In this example, John has Reader access across the subscription and additional Contributor permissions within the development resource group.

---

# Effective Permissions

A user's effective access can come from multiple role assignments.

Example:

```text
Subscription
 │
 └── Reader
       │
       ▼
Development Resource Group
 │
 └── Contributor
       │
       ▼
Virtual Machine
```

The user's permissions are evaluated from applicable role assignments and scopes.

---

# Key Points

- RBAC scope determines where a role assignment applies.
- Azure has four main scopes:
  - Management Group
  - Subscription
  - Resource Group
  - Resource
- Scope follows a hierarchy from broader to narrower levels.
- Role assignments at higher scopes can be inherited by lower scopes.
- Subscription scope can cover resource groups and resources within the subscription.
- Resource group scope applies to resources within that resource group.
- Resource scope applies to an individual resource.
- A user can have multiple role assignments at different scopes.
- Use the smallest practical scope to follow the principle of least privilege.
