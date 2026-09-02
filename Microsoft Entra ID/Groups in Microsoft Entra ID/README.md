# 5.3 Groups in Microsoft Entra ID

Microsoft Entra ID allows organizations to create and manage groups of users. Groups make it easier to manage access, permissions, and collaboration for multiple users at the same time.

---

## What is a Group in Microsoft Entra ID?

A **Microsoft Entra group** is a collection of users or other objects that can be managed together.

Instead of assigning access to users individually, administrators can assign access to a group.

Example:

```text
Group: Cloud-Engineers

Members:
- John
- David
- Sarah
- Alex
```

If the **Cloud-Engineers** group is given access to a resource, all members can receive that access based on the assigned permissions.

---

## Why Use Groups?

Groups simplify identity and access management.

Common uses include:

- Managing multiple users together
- Assigning access to Azure resources
- Assigning Microsoft Entra roles
- Managing application access
- Organizing users by department or team
- Implementing group-based access

### Example

Without a group:

```text
User 1 → Resource
User 2 → Resource
User 3 → Resource
User 4 → Resource
```

With a group:

```text
User 1 ─┐
User 2 ─┤
User 3 ─┼→ Group → Resource
User 4 ─┘
```

---

# Types of Groups

Microsoft Entra ID primarily provides two group types:

1. **Security Groups**
2. **Microsoft 365 Groups**

---

## Security Groups

A **Security Group** is mainly used to manage access and permissions.

Security groups can be used for:

- Azure resource access
- Azure RBAC assignments
- Application access
- Microsoft Entra role assignments
- Managing users with similar permissions

### Example

```text
Security Group: Developers

Members:
- John
- Sarah
- David

Role:
Reader

Scope:
Resource Group
```

All members can receive the assigned permissions through the group.

---

## Microsoft 365 Groups

A **Microsoft 365 Group** is designed mainly for collaboration.

It can provide shared resources such as:

- Shared mailbox
- Shared calendar
- SharePoint site
- Microsoft Teams integration

### Example

```text
Microsoft 365 Group: Project-Team

Members:
- John
- Sarah
- David
```

The group can be used to collaborate and share Microsoft 365 resources.

---

# Security Group vs Microsoft 365 Group

| Feature | Security Group | Microsoft 365 Group |
|---|---|---|
| Main Purpose | Access and permissions | Collaboration |
| Azure RBAC | Yes | Can be used where supported |
| Application Access | Yes | Limited compared to security groups |
| Shared Mailbox | No | Yes |
| Shared Calendar | No | Yes |
| SharePoint Collaboration | No | Yes |
| Microsoft Teams Integration | No | Yes |

---

# Group Membership

Group membership determines which users belong to a group.

Example:

```text
Group: Developers

Members:
- John
- Sarah
- David
```

Administrators can add or remove users from groups as organizational requirements change.

---

## Assigned Membership

With **Assigned** membership, administrators manually add and remove members.

Example:

```text
Administrator
      │
      ├── Add John
      ├── Add Sarah
      └── Remove David
```

This is commonly used for small or controlled groups.

---

## Dynamic Membership

With **Dynamic** membership, users can automatically become members of a group based on defined user attributes and rules.

Example:

```text
Rule:

Department = IT
```

Users whose department matches the rule can automatically become members of the group.

> **Note:** Dynamic membership requires the appropriate Microsoft Entra licensing.

---

# Group-Based Access

Groups can be used to simplify access management.

Example:

```text
Users
 │
 ▼
Developers Group
 │
 ▼
Azure RBAC Role
 │
 ▼
Resource Group
```

Instead of assigning an Azure RBAC role to every developer individually, the role can be assigned to the group.

---

# Groups and Azure RBAC

Microsoft Entra groups can be used when assigning Azure RBAC roles.

Example:

```text
Group: Developers

Role: Contributor

Scope: Resource Group
```

All users who are members of the group can receive the Contributor permissions at that scope.

This makes access management easier when many users require the same permissions.

---

# Group Ownership

A group can have **owners** who are responsible for managing the group.

Owners can manage aspects of the group depending on the group type and their permissions.

Example:

```text
Group: Developers

Owner:
- John

Members:
- Sarah
- David
- Alex
```

The owner can help manage the group's membership.

---

# Nested Groups

Groups can contain other groups in supported scenarios.

Example:

```text
IT Group
   │
   ├── Developers Group
   │      ├── John
   │      └── Sarah
   │
   └── Administrators Group
          ├── David
          └── Alex
```

Nested groups can help organize users and groups, but support and behavior can vary depending on the scenario.

---

# 🧪 Lab: Create a Security Group

## Objective

Create a security group and add users as members.

## Prerequisites

- Azure account
- Microsoft Entra tenant
- Permission to create groups
- At least one test user

---

## Step 1: Open Groups

1. Sign in to the **Azure Portal**.
2. Search for **Microsoft Entra ID**.
3. Open **Microsoft Entra ID**.
4. Select **Groups**.
5. Select **New group**.

---

## Step 2: Configure the Group

Configure the group as follows:

```text
Group type: Security
Group name: Developers
Group description: Development team
Membership type: Assigned
```

---

## Step 3: Add Members

1. Select **No members selected**.
2. Search for the test users.
3. Select the users.
4. Select **Select**.

Example:

```text
Developers

Members:
- John
- Sarah
- David
```

---

## Step 4: Create the Group

1. Review the configuration.
2. Select **Create**.

The security group is now created.

---

## Step 5: Verify the Group

1. Open **Microsoft Entra ID**.
2. Select **Groups**.
3. Search for **Developers**.
4. Open the group.
5. Select **Members**.
6. Verify that the users were added.

---

# 🧪 Lab: Add a User to an Existing Group

## Step 1: Open the Group

1. Open **Microsoft Entra ID**.
2. Select **Groups**.
3. Search for the required group.
4. Open the group.

---

## Step 2: Add Member

1. Select **Members**.
2. Select **Add members**.
3. Search for the user.
4. Select the user.
5. Select **Select**.

The user is now a member of the group.

---

# 🧪 Lab: Remove a User from a Group

## Step 1: Open the Group

1. Open **Microsoft Entra ID**.
2. Select **Groups**.
3. Open the required group.
4. Select **Members**.

---

## Step 2: Remove the Member

1. Select the user.
2. Select **Remove**.
3. Confirm the removal.

The user is no longer a member of the group.

---

# 🧪 Lab: Assign Azure RBAC to a Group

## Objective

Assign an Azure RBAC role to a group instead of assigning the role to individual users.

---

## Step 1: Open a Resource Group

1. Open **Azure Portal**.
2. Select **Resource groups**.
3. Open your test resource group.
4. Select **Access control (IAM)**.

---

## Step 2: Add Role Assignment

1. Select **Add**.
2. Select **Add role assignment**.
3. Select a role such as **Reader**.
4. Select **Next**.

---

## Step 3: Select the Group

Under **Assign access to**, select:

```text
User, group, or service principal
```

Select **Select members**.

Search for:

```text
Developers
```

Select the group.

---

## Step 4: Assign the Role

1. Select **Review + assign**.
2. Select **Review + assign** again.

The group now has the assigned Azure RBAC role at the selected scope.

All users who are members of the group can receive the permissions provided by that role.

---

# Group Lifecycle

Groups can be managed throughout their lifecycle.

```text
Create Group
    │
    ▼
Add Members
    │
    ▼
Assign Access
    │
    ▼
Manage Membership
    │
    ▼
Remove Members
    │
    ▼
Delete Group
```

---

# Key Points

- Microsoft Entra groups allow multiple users to be managed together.
- **Security Groups** are mainly used for access and permissions.
- **Microsoft 365 Groups** are mainly used for collaboration.
- Group membership can be **Assigned** or **Dynamic**.
- Groups can be used with Azure RBAC.
- Assigning permissions to a group is easier than assigning the same permissions to many users individually.
- Group owners can help manage group membership.
- Groups are useful for organizing users by teams, departments, or access requirements.
