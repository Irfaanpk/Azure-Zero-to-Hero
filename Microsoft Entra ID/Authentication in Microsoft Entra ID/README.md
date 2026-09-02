# 5.5 Authentication in Microsoft Entra ID

Authentication in Microsoft Entra ID is the process of verifying the identity of a user before allowing them to access Azure resources and Microsoft services.

---

## What is Authentication?

**Authentication** verifies **who the user is**.

Example:

```text
User
  │
  ▼
Enter Username and Password
  │
  ▼
Microsoft Entra ID
  │
  ▼
Verify Identity
  │
  ▼
Authentication Successful
```

If the identity cannot be verified, access is denied.

---

## Authentication vs Authorization

Authentication and authorization are different concepts.

| Concept | Purpose | Example |
|---|---|---|
| Authentication | Verifies who you are | Sign in with username and password |
| Authorization | Determines what you can access | Allow access to a resource group |

Example:

```text
Authentication
"Who are you?"
        │
        ▼
John

Authorization
"What can John access?"
        │
        ▼
Resource Group
```

---

# Microsoft Entra Authentication

When a user signs in to Azure, Microsoft Entra ID authenticates the user.

Example:

```text
Username:
john@company.com

Password:
********

        │
        ▼

Microsoft Entra ID

        │
        ▼

Identity Verified

        │
        ▼

User Signed In
```

After successful authentication, Microsoft Entra ID issues security tokens that applications and services can use to determine the user's identity and access.

---

# Authentication Methods

Microsoft Entra ID supports different authentication methods.

Common methods include:

- Password
- Multi-factor authentication (MFA)
- Microsoft Authenticator
- FIDO2 security keys
- Passkeys
- Certificate-based authentication
- Windows Hello for Business

The available authentication methods depend on the organization's configuration and licensing.

---

# Password Authentication

Password authentication is one of the basic authentication methods.

Example:

```text
Username
    +
Password
    │
    ▼
Microsoft Entra ID
    │
    ▼
Identity Verified
```

The user provides their username and password during sign-in.

---

# Multi-Factor Authentication (MFA)

**Multi-factor authentication (MFA)** requires additional verification after the user provides their primary credentials.

Example:

```text
Username + Password
        │
        ▼
Additional Verification
        │
        ▼
Microsoft Authenticator
        │
        ▼
Authentication Successful
```

MFA provides an additional layer of protection compared to password-only authentication.

---

## Example of MFA

A user signs in:

```text
Username:
john@company.com

Password:
********
```

Microsoft Entra ID then requests additional verification:

```text
Approve sign-in request
        OR
Enter verification code
```

After successful verification, authentication is completed.

---

# Microsoft Authenticator

**Microsoft Authenticator** is a mobile application that can be used as an authentication method.

It can be used for:

- Push notifications
- Verification codes
- Passwordless authentication
- Multi-factor authentication

Example:

```text
User enters password
        │
        ▼
Microsoft Entra ID
        │
        ▼
Authenticator notification
        │
        ▼
User approves
        │
        ▼
Sign-in successful
```

---

# Passwordless Authentication

Passwordless authentication allows users to authenticate without entering a traditional password.

Examples include:

- Microsoft Authenticator
- FIDO2 security keys
- Passkeys
- Windows Hello for Business

Example:

```text
Passwordless Method
        │
        ▼
Verify User
        │
        ▼
Microsoft Entra ID
        │
        ▼
Access Granted
```

---

# FIDO2 Security Keys

**FIDO2 security keys** provide passwordless authentication using a physical security key or compatible authentication device.

Example:

```text
User
 │
 ▼
FIDO2 Security Key
 │
 ▼
Microsoft Entra ID
 │
 ▼
Authentication Successful
```

FIDO2 can help reduce dependence on passwords.

---

# Authentication Methods in Microsoft Entra ID

Authentication methods can be managed from the Microsoft Entra admin center.

Common authentication methods include:

```text
Password
Microsoft Authenticator
FIDO2 Security Key
Passkey
Certificate-based Authentication
Windows Hello for Business
```

---

# 🧪 Lab: Configure Microsoft Authenticator

## Objective

Configure Microsoft Authenticator as an authentication method for a test user.

## Prerequisites

- Microsoft Entra tenant
- Test user
- Microsoft Authenticator application
- Permission to manage authentication methods

---

## Step 1: Open Microsoft Entra ID

1. Sign in to the **Azure Portal**.
2. Search for **Microsoft Entra ID**.
3. Open **Microsoft Entra ID**.

---

## Step 2: Open Authentication Methods

1. Select **Authentication methods**.
2. Review the available authentication methods.
3. Select **Policies** if required.

---

## Step 3: Open Microsoft Authenticator

1. Select **Microsoft Authenticator**.
2. Review the configuration.
3. Configure the required users or groups.
4. Enable the method according to your lab requirements.

---

## Step 4: Register the Test User

The test user can register Microsoft Authenticator during the sign-in or security information registration process.

The user will be prompted to:

```text
Install Microsoft Authenticator
        │
        ▼
Add Work or School Account
        │
        ▼
Scan QR Code
        │
        ▼
Complete Registration
```

---

## Step 5: Test Authentication

1. Sign in using the test user.
2. Enter the username and password.
3. Complete the Microsoft Authenticator verification.
4. Verify that authentication succeeds.

---

# 🧪 Lab: View a User's Authentication Methods

## Step 1: Open Users

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Select the test user.

---

## Step 2: Open Authentication Methods

1. Select **Authentication methods**.
2. Review the authentication methods registered for the user.

Example:

```text
User:
john@company.com

Authentication Methods:
- Password
- Microsoft Authenticator
```

---

# Authentication Registration

Users can register authentication methods that are allowed by the organization.

Examples:

```text
Microsoft Authenticator
FIDO2 Security Key
Passkey
Phone
```

The registered methods can then be used during authentication.

---

# Authentication Flow

A simplified Microsoft Entra authentication flow looks like this:

```text
User
 │
 ▼
Application / Azure Portal
 │
 ▼
Microsoft Entra ID
 │
 ▼
Authentication
 │
 ├── Username + Password
 │
 └── Additional Authentication Method
 │
 ▼
Identity Verified
 │
 ▼
Security Token
 │
 ▼
Application / Azure Resource
```

Authentication verifies the user's identity before access is evaluated.

---

# Authentication and Azure Resource Access

Authentication alone does not automatically provide access to Azure resources.

After authentication, authorization determines what the user can access.

Example:

```text
User
 │
 ▼
Authentication
 │
 ▼
Microsoft Entra ID
 │
 ▼
Identity Verified
 │
 ▼
Azure RBAC
 │
 ▼
Check Permissions
 │
 ▼
Azure Resource
```

For example, a user may successfully sign in to Azure but still be unable to manage a virtual machine if the user does not have the required Azure RBAC permissions.

---

# Account Authentication Problems

Authentication can fail for several reasons.

Common examples include:

- Incorrect password
- Account disabled
- Authentication method not available
- MFA verification failure
- User has not completed registration
- Sign-in blocked by organizational configuration

Example:

```text
User
 │
 ▼
Authentication Attempt
 │
 ▼
Verification Failed
 │
 ▼
Access Denied
```

---

# 🧪 Lab: Disable a User and Test Authentication

## Objective

Understand how disabling a Microsoft Entra user affects authentication.

---

## Step 1: Disable the User

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Open the test user.
4. Open **Properties**.
5. Set **Account enabled** to **No**.
6. Select **Save**.

---

## Step 2: Test Sign-in

Try signing in using the disabled user.

The user should not be able to authenticate successfully.

---

## Step 3: Enable the User

1. Open the test user.
2. Open **Properties**.
3. Set **Account enabled** to **Yes**.
4. Select **Save**.

The user can authenticate again when the account is enabled and all required authentication conditions are satisfied.

---

## Key Points

- Authentication verifies the identity of a user.
- Microsoft Entra ID provides authentication for Azure and Microsoft services.
- Authentication is different from authorization.
- Passwords are a basic authentication method.
- MFA provides an additional authentication factor.
- Microsoft Authenticator can be used for MFA and passwordless authentication.
- FIDO2 security keys provide passwordless authentication.
- Passkeys can be used as a passwordless authentication method.
- Authentication methods can be managed in Microsoft Entra ID.
- Successful authentication does not automatically grant Azure resource permissions.
- Azure RBAC determines access to Azure resources after authentication.
