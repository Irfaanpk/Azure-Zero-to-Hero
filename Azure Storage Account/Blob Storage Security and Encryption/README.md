# 6.13 Blob Storage Security and Encryption

Azure Storage provides multiple security and encryption features to protect data stored in a storage account.

This includes:

- Encryption at rest
- Microsoft-managed keys
- Customer-managed keys
- Infrastructure encryption
- Secure data access
- HTTPS
- Network security

---

## Encryption at Rest

Azure Storage automatically encrypts data before storing it.

```text
Data
 ↓
Encryption
 ↓
Azure Storage
 ↓
Encrypted Data
```

Encryption at rest protects stored data from unauthorized access to the underlying storage infrastructure.

Azure Storage encryption is enabled by default.

---

## Microsoft-Managed Keys

By default, Azure Storage uses **Microsoft-managed keys** for encryption.

Microsoft manages:

- Key creation
- Key storage
- Key rotation
- Key management

You do not need to manage the encryption keys yourself.

```text
Azure Storage
      ↓
Microsoft-Managed Key
      ↓
Encryption
```

This is the default encryption option for Azure Storage.

---

## Customer-Managed Keys

Azure Storage also supports **customer-managed keys (CMK)**.

With customer-managed keys, you control the encryption key used by the storage account.

Customer-managed keys are stored in:

- Azure Key Vault
- Azure Key Vault Managed HSM

```text
Azure Key Vault
      ↓
Customer-Managed Key
      ↓
Azure Storage
      ↓
Encrypted Data
```

---

## Why Use Customer-Managed Keys?

Customer-managed keys provide additional control over encryption.

They may be required when organizations need:

- Greater control over encryption keys
- Key rotation management
- Key access control
- Compliance requirements
- Ability to revoke access to the encryption key

---

## Customer-Managed Keys vs Microsoft-Managed Keys

| Feature | Microsoft-Managed Keys | Customer-Managed Keys |
|---|---|---|
| Key managed by | Microsoft | Customer |
| Key Vault required | ❌ | ✅ |
| Key rotation control | Microsoft | Customer |
| Configuration complexity | Low | Higher |
| Default option | ✅ | ❌ |
| Customer control | Limited | Higher |

---

# Infrastructure Encryption

Azure Storage also supports **infrastructure encryption**, which provides an additional layer of encryption.

With infrastructure encryption enabled, data can be encrypted twice:

```text
Data
 ↓
Encryption Layer 1
 ↓
Encryption Layer 2
 ↓
Storage
```

This is also referred to as **double encryption**.

It can provide additional protection for workloads with strict security or compliance requirements.

---

## Infrastructure Encryption vs Customer-Managed Keys

These are different concepts.

**Customer-managed keys** determine who controls the encryption key.

**Infrastructure encryption** provides an additional encryption layer.

They can be used together when supported by the storage configuration.

---

# HTTPS

Azure Storage supports secure access using **HTTPS**.

HTTPS encrypts data while it is transmitted between the client and Azure Storage.

```text
Client
   │
   │ HTTPS
   ▼
Azure Storage
```

HTTPS protects data **in transit**.

---

## Secure Transfer Required

Azure Storage provides a **Secure transfer required** setting.

When enabled, requests must use secure protocols such as HTTPS.

Portal path:

```text
Storage Account
    ↓
Settings
    ↓
Configuration
    ↓
Secure transfer required
```

For secure environments, keep:

```text
Secure transfer required = Enabled
```

---

# Storage Data Security Layers

Azure Storage security can be viewed as multiple layers:

```text
                    Azure Storage
                         │
        ┌────────────────┼────────────────┐
        │                │                │
   Network Security   Authentication   Encryption
        │                │                │
 Firewall / VNet      Entra ID /       At Rest
 Private Endpoint     SAS / Keys
```

Each layer provides a different type of protection.

---

## Authentication and Authorization

Storage data can be protected using:

- Microsoft Entra ID
- Azure RBAC
- SAS
- Storage account access keys

For identity-based access, Microsoft recommends using **Microsoft Entra ID and Azure RBAC** where possible.

---

## Blob Data Protection

Additional Blob Storage protection features include:

- Blob Versioning
- Blob Soft Delete
- Blob Snapshots
- Blob Immutability
- Lifecycle Management

These features are covered in the previous storage topics.

---

# 🧪 Lab: Configure Storage Encryption

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

### Step 2: View Encryption Settings

Go to:

```text
Security + networking → Encryption
```

Review the current encryption configuration.

You should see that encryption is enabled.

---

### Step 3: Review Encryption Type

Check the encryption key management option.

For the default configuration, you will see:

```text
Microsoft-managed keys
```

---

### Step 4: Enable Infrastructure Encryption

If supported by the storage account configuration, review the option for:

```text
Enable infrastructure encryption
```

Understand that this provides an additional layer of encryption.

> Infrastructure encryption availability and configuration options can depend on the storage account configuration and region.

---

### Step 5: Review Secure Transfer

Go to:

```text
Settings → Configuration
```

Verify:

```text
Secure transfer required = Enabled
```

This ensures that storage requests use secure transport.

---

## Key Points

- Azure Storage encrypts data **at rest by default**.
- **Microsoft-managed keys** are the default encryption option.
- **Customer-managed keys** provide greater control over encryption keys.
- Customer-managed keys can be stored in **Azure Key Vault** or **Key Vault Managed HSM**.
- **Infrastructure encryption** provides an additional encryption layer.
- **HTTPS** protects data in transit.
- **Secure transfer required** helps enforce secure connections.
- Azure RBAC, Microsoft Entra ID, SAS, and access keys provide different ways to control storage access.
- Encryption, network security, authentication, and authorization provide separate layers of storage security.
