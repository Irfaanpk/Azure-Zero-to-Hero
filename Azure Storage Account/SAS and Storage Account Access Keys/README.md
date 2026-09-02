# 6.10 SAS and Storage Account Access Keys

Azure Storage provides different ways to authenticate and authorize access to storage data.

The main methods covered here are:

- **Shared Access Signature (SAS)**
- **Storage Account Access Keys**

---

## Shared Access Signature (SAS)

A **Shared Access Signature (SAS)** provides delegated access to Azure Storage resources.

A SAS allows you to specify:

- What resource can be accessed
- What operations are allowed
- When the access starts
- When the access expires
- Where the request can come from
- Which protocol can be used

Example:

```text
User
  ↓
SAS Token
  ↓
Azure Storage
  ↓
Limited Access
```

---

## Why Use SAS?

SAS is useful when you want to provide temporary and limited access without giving users the storage account access keys.

Example:

```text
Application
    ↓
Generate SAS
    ↓
User
    ↓
Access Blob for 1 Hour
```

After the SAS expires, access is no longer available.

---

# SAS Permissions

A SAS can provide different permissions depending on the storage resource.

Common Blob permissions include:

| Permission | Description |
|---|---|
| **Read (r)** | Read data |
| **Write (w)** | Write or modify data |
| **Delete (d)** | Delete data |
| **List (l)** | List resources |
| **Create (c)** | Create resources |
| **Add (a)** | Add data |
| **Update (u)** | Update data |

Only grant the permissions that are required.

---

## SAS Start and Expiry Time

A SAS can have a defined validity period.

Example:

```text
Start:
09:00 AM

Expiry:
10:00 AM
```

The SAS can be used during the configured time period.

```text
09:00 ───────────── 10:00
       SAS Valid
```

---

## SAS Protocol

A SAS can restrict access to specific protocols.

Common option:

```text
HTTPS only
```

Using HTTPS helps protect data while it is being transmitted.

---

## SAS IP Restrictions

A SAS can optionally restrict access to specific IP addresses or ranges.

Example:

```text
Allowed IP:
203.0.113.10
```

Requests from other IP addresses can be denied.

---

# Types of SAS

Azure Storage provides different types of SAS.

### 1. User Delegation SAS

A **User Delegation SAS** is secured using Microsoft Entra credentials and is supported for Blob Storage.

It is generally preferred when Microsoft Entra authentication can be used instead of storage account keys.

```text
Microsoft Entra ID
       ↓
User Delegation SAS
       ↓
Blob Storage
```

---

### 2. Service SAS

A **Service SAS** provides access to a specific storage service resource.

For example:

```text
Blob
Container
File Share
Queue
Table
```

Example:

```text
Service SAS
    ↓
Specific Blob
```

---

### 3. Account SAS

An **Account SAS** can provide access to one or more storage services and can include service-level permissions.

Example:

```text
Account SAS
    ↓
Blob Storage
File Storage
Queue Storage
Table Storage
```

---

## SAS Comparison

| SAS Type | Authentication Basis | Scope |
|---|---|---|
| **User Delegation SAS** | Microsoft Entra ID | Blob Storage resources |
| **Service SAS** | Storage account key | Specific service/resource |
| **Account SAS** | Storage account key | One or more storage services |

---

# SAS Example

A SAS URL may look similar to:

```text
https://storageaccount.blob.core.windows.net/images/photo.jpg
?sv=...
&st=...
&se=...
&sr=...
&sp=r
&sig=...
```

The query parameters contain information such as:

```text
sv → Storage service version
st → Start time
se → Expiry time
sr → Resource
sp → Permissions
sig → Signature
```

> Never share a SAS URL unnecessarily because anyone who possesses a valid SAS can use the permissions granted by that token.

---

# Stored Access Policies

A **Stored Access Policy** can be used with a service SAS for certain Azure Storage resources.

It allows access settings such as:

- Permissions
- Start time
- Expiry time

to be associated with a policy.

This can make it easier to manage and revoke access centrally.

---

# Storage Account Access Keys

Azure Storage accounts have **two access keys**:

```text
Key 1
Key 2
```

These keys provide access to the storage account using the permissions associated with the key.

They are sometimes called:

```text
Storage Account Keys
```

---

## Why Are There Two Keys?

Two keys allow you to rotate credentials without interrupting applications.

Example:

```text
Application
    ↓
Using Key 1

Rotate Key 2
    ↓
Update Application to Key 2
    ↓
Regenerate Key 1
```

This provides a way to rotate keys safely.

---

## Access Key Permissions

Storage account access keys provide broad access to the storage account.

Unlike SAS, you do not normally specify individual permissions such as:

```text
Read only
Write only
Delete only
```

for an access key.

Therefore, access keys should be protected carefully.

---

## Access Keys vs SAS

| Feature | Access Keys | SAS |
|---|---|---|
| Access scope | Broad storage account access | Limited access |
| Expiration | No automatic expiration | Can have expiry |
| Permissions | Broad | Specific permissions |
| IP restriction | Not part of the key itself | Can be configured in SAS |
| Protocol restriction | Not part of the key itself | Can be configured in SAS |
| Rotation | Key regeneration | Generate new SAS |
| Recommended for temporary access | ❌ | ✅ |

---

## Access Keys and Security

Access keys are sensitive credentials.

Do not:

- Store them in source code
- Commit them to GitHub
- Share them publicly
- Put them directly in application configuration files

Instead, use appropriate secure authentication mechanisms such as:

- Microsoft Entra ID
- Managed identities
- Key Vault

---

# SAS vs Azure RBAC

SAS and Azure RBAC provide access in different ways.

| Feature | SAS | Azure RBAC |
|---|---|---|
| Access mechanism | Signed token | Role assignment |
| Expiration | Can be configured | Role assignment remains until removed |
| Permissions | Explicit SAS permissions | Role definition |
| Identity based | Not necessarily | Yes |
| Temporary access | Excellent | Possible but requires role management |
| Typical use | Delegated access | Identity-based access |

Example:

```text
Azure RBAC

User
 ↓
Microsoft Entra ID
 ↓
Storage Blob Data Reader
 ↓
Blob Storage
```

```text
SAS

Application
 ↓
SAS Token
 ↓
Blob Storage
```

---

# 🧪 Lab: Generate a SAS and Access a Blob

### Step 1: Open Storage Account

Go to:

```text
https://portal.azure.com
```

Open:

```text
Storage accounts
```

Select your storage account.

---

### Step 2: Open Containers

Go to:

```text
Data storage → Containers
```

Open a container containing a blob.

Example:

```text
images
```

Select:

```text
photo.jpg
```

---

### Step 3: Generate SAS

Select:

```text
Generate SAS
```

Configure:

```text
Permissions:
Read
```

Set an expiry time.

Example:

```text
Start: Current time
Expiry: 1 hour later
```

Set the protocol to:

```text
HTTPS only
```

Generate the SAS.

---

### Step 4: Copy the SAS URL

Azure provides a URL containing the SAS token.

Example:

```text
https://<storage-account>.blob.core.windows.net/images/photo.jpg?<SAS-TOKEN>
```

Open the URL in a browser.

The blob should be accessible according to the permissions granted by the SAS.

---

### Step 5: Test the Permission

If you generated a **Read-only SAS**, the SAS should allow reading the blob but not operations that were not included in the SAS permissions.

---

### Step 6: Test Expiration

Wait until the SAS expires or configure a short expiry time for testing.

After expiration:

```text
SAS Request
    ↓
Expired
    ↓
❌ Access Denied
```

---

# 🧪 Lab: View and Rotate Storage Account Access Keys

### Step 1: Open Access Keys

In the Storage Account, go to:

```text
Security + networking → Access keys
```

You will see:

```text
Key 1
Key 2
```

---

### Step 2: Review Connection Information

Each key can be used to generate storage connection information.

Do not share the keys.

---

### Step 3: Regenerate a Key

For testing, select:

```text
Regenerate Key 2
```

Confirm the operation.

The old value of Key 2 is invalidated.

---

### Step 4: Understand Key Rotation

A typical rotation process is:

```text
Application → Key 1

Regenerate Key 2
      ↓
Update Application to Key 2
      ↓
Regenerate Key 1
      ↓
Update Application if required
```

This allows credentials to be rotated while maintaining application access.

---

## Important Points

- **SAS** provides delegated and limited access to Azure Storage.
- SAS can specify permissions, expiry time, IP restrictions, and protocols.
- **User Delegation SAS** uses Microsoft Entra credentials.
- **Service SAS** provides access to specific storage resources.
- **Account SAS** can provide access across multiple storage services.
- Storage accounts have **two access keys**.
- Access keys provide broad access and must be protected carefully.
- Two access keys make credential rotation easier.
- SAS is useful for temporary or delegated access.
- For identity-based access, prefer **Microsoft Entra ID and Azure RBAC** where appropriate.
- Never expose access keys or SAS tokens publicly.
