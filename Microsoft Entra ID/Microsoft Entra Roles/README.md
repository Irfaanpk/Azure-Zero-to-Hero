# 5.4 Microsoft Entra Roles

Microsoft Entra roles are used to control what administrators can do within **Microsoft Entra ID**.

Instead of giving every administrator full control, specific roles can be assigned based on the administrative responsibilities of the user.

---

## What is a Microsoft Entra Role?

A **Microsoft Entra role** is a collection of permissions that allows an administrator to perform specific management tasks in Microsoft Entra ID.

Example:

```text
User: John

Role:
User Administrator
```

John can perform tasks allowed by the **User Administrator** role, such as managing users.

---

## Why Use Microsoft Entra Roles?

Microsoft Entra roles help organizations:

- Delegate administrative responsibilities
- Follow the principle of least privilege
- Separate administrative duties
- Control who can manage users, groups, and other identity objects
- Avoid giving unnecessary Global Administrator permissions

---

# Microsoft Entra Built-in Roles

Microsoft Entra ID provides many built-in administrative roles.

Some important roles for Azure administration include:

| Role | Purpose |
|---|---|
| Global Administrator | Full access to Microsoft Entra ID and most identity-related administrative features |
| User Administrator | Manage users and some user-related properties |
| Groups Administrator | Manage groups and group-related settings |
| Helpdesk Administrator | Manage password resets and certain user support tasks |
| Password Administrator | Reset passwords for supported users |
| Authentication Administrator | Manage authentication-related settings for users |
| Directory Readers | Read directory information |
| Application Administrator | Manage application registrations and enterprise applications |

> **Note:** The exact permissions available to a role depend on the role definition and Microsoft Entra configuration.

---

# Global Administrator

The **Global Administrator** role provides the highest level of administrative access in Microsoft Entra ID.

A Global Administrator can manage a wide range of Microsoft Entra resources and settings.

Example:

```text
Global Administrator
        │
        ├── Users
        ├── Groups
        ├── Applications
        ├── Authentication
        └── Directory Settings
```

Because of its broad permissions, Global Administrator should be assigned carefully.

---

# User Administrator

The **User Administrator** role is used for managing users.

Typical tasks include:

- Create users
- Delete users
- Update user properties
- Manage certain user accounts
- Manage some user-related settings

Example:

```text
User Administrator
        │
        ├── Create User
        ├── Update User
        └── Delete User
```

---

# Groups Administrator

The **Groups Administrator** role is used to manage groups.

Typical tasks include:

- Create groups
- Delete groups
- Manage group membership
- Update group properties
- Manage group-related settings

Example:

```text
Groups Administrator
        │
        ├── Create Group
        ├── Add Members
        ├── Remove Members
        └── Delete Group
```

---

# Directory Readers

The **Directory Readers** role provides read access to directory information.

A user with this role can read certain information about objects in Microsoft Entra ID without having broad administrative permissions.

Example:

```text
Directory Reader
       │
       └── Read Directory Information
```

This can be useful when an application or administrator needs directory information without requiring write permissions.

---

# Role Assignment

A Microsoft Entra role must be **assigned** to a user or other supported identity before the permissions become available.

Example:

```text
User
 │
 ▼
Microsoft Entra Role
 │
 ▼
Permissions
```

Example:

```text
John
 │
 ▼
User Administrator
 │
 ▼
User Management Permissions
```

---

# Role Assignment Through Azure Portal

Administrators can assign Microsoft Entra roles through the Azure Portal.

## Step 1: Open Microsoft Entra ID

1. Sign in to the **Azure Portal**.
2. Search for **Microsoft Entra ID**.
3. Open **Microsoft Entra ID**.

---

## Step 2: Open Roles and Administrators

1. Select **Roles & admins**.
2. Browse the available Microsoft Entra roles.
3. Search for the required role.

Example:

```text
User Administrator
```

---

## Step 3: Open the Role

1. Select **User Administrator**.
2. Select **Add assignments**.

---

## Step 4: Select the User

1. Select the user who should receive the role.
2. Select **Assign**.

The user now has the permissions provided by the assigned Microsoft Entra role.

---

# 🧪 Lab: Assign User Administrator Role

## Objective

Assign the **User Administrator** role to a test user.

## Prerequisites

- Azure account
- Microsoft Entra tenant
- Permission to manage role assignments
- Test user

---

## Step 1: Open Roles and Administrators

1. Open **Microsoft Entra ID**.
2. Select **Roles & admins**.
3. Search for **User Administrator**.

---

## Step 2: Add Assignment

1. Open **User Administrator**.
2. Select **Add assignments**.
3. Search for your test user.
4. Select the user.
5. Select **Assign**.

---

## Step 3: Verify the Assignment

1. Open the **User Administrator** role.
2. Check the assigned members.
3. Verify that the test user appears in the list.

---

# 🧪 Lab: Remove a Microsoft Entra Role

## Step 1: Open the Role

1. Open **Microsoft Entra ID**.
2. Select **Roles & admins**.
3. Open the assigned role.

---

## Step 2: Remove the Assignment

1. Select the assigned user.
2. Select **Remove assignment**.
3. Confirm the removal.

The user no longer has the permissions provided by that Microsoft Entra role.

---

# Microsoft Entra Roles vs Azure RBAC

Microsoft Entra roles and **Azure RBAC** are different permission systems.

### Microsoft Entra Roles

Used mainly to manage:

```text
Microsoft Entra ID
Users
Groups
Applications
Directory
Identity Settings
```

### Azure RBAC

Used mainly to manage access to:

```text
Azure Resources
Resource Groups
Subscriptions
Management Groups
```

Example:

```text
Microsoft Entra Role
        │
        ▼
Manage Users / Groups / Directory
```

```text
Azure RBAC Role
        │
        ▼
Manage Azure Resources
```

A detailed explanation of Azure RBAC is covered in the **Azure RBAC** section.

---

# Built-in Roles vs Custom Roles

Microsoft Entra ID provides **built-in roles** that are already created by Microsoft.

Organizations can also create **custom roles** when built-in roles do not provide the required permissions.

### Built-in Role

```text
Microsoft-provided role
        │
        ▼
Predefined permissions
```

### Custom Role

```text
Organization
      │
      ▼
Define required permissions
      │
      ▼
Custom Role
```

For AZ-104, understanding the purpose of built-in roles and role assignments is more important than going deeply into custom Microsoft Entra role design.

---

# Principle of Least Privilege

Microsoft Entra roles should be assigned according to the user's responsibilities.

For example:

```text
User Management
       │
       ▼
User Administrator
```

Instead of:

```text
User Management
       │
       ▼
Global Administrator
```

The goal is to give users only the permissions they actually need.

---

# Role Assignment Example

Consider an organization with three administrators:

```text
John
Role: Global Administrator

Sarah
Role: User Administrator

David
Role: Groups Administrator
```

Their responsibilities can be separated based on their assigned roles.

```text
John
 └── Broad Microsoft Entra Administration

Sarah
 └── User Management

David
 └── Group Management
```

---

## Key Points

- Microsoft Entra roles control administrative permissions within Microsoft Entra ID.
- Roles allow organizations to delegate administrative responsibilities.
- **Global Administrator** provides very broad administrative permissions.
- **User Administrator** is used mainly for user management.
- **Groups Administrator** is used mainly for group management.
- **Directory Readers** provides directory read access.
- Roles must be assigned to users or supported identities.
- Microsoft Entra roles are different from Azure RBAC roles.
- Built-in roles are predefined by Microsoft.
- Custom roles can be created when built-in roles do not meet specific requirements.
- Follow the **principle of least privilege** when assigning administrative roles.
