# 5.2 Users in Microsoft Entra ID

Microsoft Entra ID allows organizations to create and manage user identities. Users can authenticate to Microsoft Entra ID and access Azure resources based on their assigned permissions.

---

## What is a Microsoft Entra User?

A **Microsoft Entra user** is an identity that can authenticate to Microsoft Entra ID and access resources based on assigned permissions.

Users can represent:

- Employees
- Administrators
- Developers
- External users
- Other users who require access to organizational resources

---

## Types of Users

### Member Users

A **member user** normally belongs to the organization.

Example:

```text
john@company.com
```

Member users are commonly used for employees and internal users.

### Guest Users

A **guest user** is an external user who is invited to collaborate with the organization.

Example:

```text
external-user@example.com
```

Guest users can be given access to required resources without becoming internal members of the organization.

---

## User Properties

A Microsoft Entra user contains different properties used to identify and manage the user.

Common properties include:

- Display Name
- User Principal Name (UPN)
- First Name
- Last Name
- Email
- Job Title
- Department
- Location
- Account Status

### Example

```text
Display Name: John Smith
User Principal Name: john@company.com
Job Title: Cloud Engineer
Department: IT
```

---

## User Principal Name (UPN)

The **User Principal Name (UPN)** is the sign-in name of a Microsoft Entra user.

Example:

```text
john@company.com
```

The UPN is commonly used when the user signs in to Azure and other Microsoft services.

---

## User Account Status

A Microsoft Entra user account can be **enabled** or **disabled**.

### Enabled User

An enabled user can authenticate and access resources for which they have permission.

### Disabled User

A disabled user cannot authenticate using the disabled account.

Disabling an account can be useful when a user temporarily or permanently no longer requires access.

---

## User Lifecycle

User management generally involves the following lifecycle:

```text
Create User
    │
    ▼
Assign Access
    │
    ▼
User Uses Resources
    │
    ▼
Disable User
    │
    ▼
Delete User
```

Administrators can manage users throughout their lifecycle based on organizational requirements.

---

# 🧪 Lab: Create a Microsoft Entra User

## Objective

Create a new Microsoft Entra user using the Azure Portal.

## Prerequisites

- Azure account
- Azure subscription
- Permission to create Microsoft Entra users

---

## Step 1: Open Microsoft Entra ID

1. Sign in to the **Azure Portal**.
2. Search for **Microsoft Entra ID**.
3. Open **Microsoft Entra ID**.

---

## Step 2: Open Users

1. From the Microsoft Entra ID page, select **Users**.
2. Select **New user**.
3. Select **Create new user**.

---

## Step 3: Enter User Information

Enter the required user information.

Example:

```text
Username: john
Display name: John Smith
```

The complete sign-in name will use the tenant domain.

Example:

```text
john@abccompany.onmicrosoft.com
```

---

## Step 4: Configure User Properties

Configure additional properties if required.

```text
First name: John
Last name: Smith
Job title: Cloud Engineer
Department: IT
```

---

## Step 5: Create the User

1. Select **Review + create**.
2. Review the configuration.
3. Select **Create**.

The Microsoft Entra user will now be created.

---

## Step 6: Verify the User

1. Go back to **Microsoft Entra ID**.
2. Select **Users**.
3. Search for the newly created user.
4. Open the user.
5. Verify the user properties.

---

## Expected Result

The newly created user should appear in the Microsoft Entra users list.

---

# 🧪 Lab: Create a Guest User

## Objective

Invite an external user to the Microsoft Entra tenant as a guest user.

---

## Step 1: Open Users

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Select **New user**.
4. Select **Invite external user**.

---

## Step 2: Enter Guest Information

Enter the external user's information.

```text
Email: external-user@example.com
Display name: External User
```

---

## Step 3: Send Invitation

1. Select **Review + invite**.
2. Review the details.
3. Select **Invite**.

Microsoft Entra ID sends an invitation to the external user.

---

## Step 4: Verify Guest User

1. Open **Users**.
2. Search for the invited user.
3. Open the user.
4. Verify that the **User type** is `Guest`.

---

# 🧪 Lab: Disable a User

## Step 1: Open the User

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Select the test user.

---

## Step 2: Disable the Account

1. Open **Properties**.
2. Find **Account enabled**.
3. Set it to **No**.
4. Select **Save**.

The user account is now disabled.

---

## Step 3: Enable the Account

To enable the account again:

1. Open the user.
2. Open **Properties**.
3. Set **Account enabled** to **Yes**.
4. Select **Save**.

---

# 🧪 Lab: Delete a User

## Step 1: Select the User

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Select the test user.

## Step 2: Delete the User

1. Select **Delete**.
2. Confirm the deletion.

The user will be moved to **Deleted users** and can be restored during the available retention period.

> **Note:** Use a test user for this lab. Do not delete production users.

---

## Key Points

- Microsoft Entra users represent identities that can authenticate and access resources.
- Member users normally belong to the organization.
- Guest users are external users invited to collaborate.
- The UPN is commonly used as the user's sign-in name.
- User properties provide information about the user.
- User accounts can be enabled or disabled.
- Users can be created, managed, disabled, and deleted through the Azure Portal.
- Users can be assigned access through groups, Microsoft Entra roles, and Azure RBAC.
