# 9.3 Disk Encryption Set

## What is a Disk Encryption Set?

A **Disk Encryption Set (DES)** is an Azure resource that allows you to use **customer-managed keys (CMK)** to encrypt Azure managed disks.

By default, Azure managed disks use platform-managed encryption. A Disk Encryption Set allows you to control the encryption key through **Azure Key Vault**.

```text
                 Azure Key Vault
                       │
                 Customer Key
                       │
                       ▼
              Disk Encryption Set
                       │
                       ▼
                Managed Disk
                       │
                       ▼
                 Azure VM
```

---

## Why Use Disk Encryption Set?

Disk Encryption Sets are used when an organization needs greater control over encryption keys.

Common reasons include:

- Customer-managed encryption keys
- Centralized key management
- Key rotation
- Compliance requirements
- Control over the encryption key lifecycle

---

# Customer-Managed Keys

A **customer-managed key (CMK)** is an encryption key created and controlled by the customer.

The key is stored in:

```text
Azure Key Vault
```

Instead of relying only on Microsoft-managed keys:

```text
Azure Managed Key
       ↓
Managed Disk
```

You can use:

```text
Customer Key
     ↓
Azure Key Vault
     ↓
Disk Encryption Set
     ↓
Managed Disk
```

---

# Azure Key Vault

**Azure Key Vault** securely stores and manages cryptographic keys, secrets, and certificates.

For Disk Encryption Set, Key Vault is used to store the customer-managed encryption key.

```text
Key Vault
   │
   └── Customer-Managed Key
             │
             ▼
       Disk Encryption Set
```

---

# Disk Encryption Set Components

The main components are:

```text
Azure Key Vault
      │
      │ Customer-Managed Key
      ▼
Disk Encryption Set
      │
      ▼
Managed Disk
      │
      ▼
Azure VM
```

### Key Vault

Stores the customer-managed key.

### Customer-Managed Key

The key used for encryption.

### Disk Encryption Set

Azure resource that references the key and enables its use with managed disks.

### Managed Disk

The disk that uses the encryption configuration.

---

# Managed Identity

A Disk Encryption Set uses a **managed identity** to access the customer-managed key in Azure Key Vault.

```text
Disk Encryption Set
        │
        │ Managed Identity
        ▼
   Azure Key Vault
        │
        ▼
 Customer Key
```

The required permissions must be configured so the Disk Encryption Set can access the key.

---

# Creating a Disk Encryption Set

The general workflow is:

```text
1. Create Azure Key Vault
          ↓
2. Create Customer-Managed Key
          ↓
3. Create Disk Encryption Set
          ↓
4. Grant DES access to the key
          ↓
5. Use DES with Managed Disk
```

---

# Using Disk Encryption Set with Managed Disks

A Disk Encryption Set can be associated with supported managed disks.

```text
             Disk Encryption Set
                      │
                      ▼
               Managed Disk
                      │
                      ▼
                  Azure VM
```

The disk uses the customer-managed key referenced by the Disk Encryption Set.

---

# Key Rotation

Customer-managed keys can be rotated according to organizational requirements.

```text
Old Key
   ↓
New Key
   ↓
Disk Encryption Set
   ↓
Managed Disk
```

Key rotation helps organizations maintain control over the encryption key lifecycle.

---

# Encryption at Host

**Encryption at host** provides encryption for data stored on the VM host, including temporary disks and cache.

It provides an additional layer of protection beyond encryption on managed disks.

```text
VM
 │
 ├── OS Disk
 ├── Data Disk
 ├── Temporary Disk
 └── Host Cache
          │
          ▼
   Encryption at Host
```

---

# Platform-Managed vs Customer-Managed Encryption

| Platform-Managed Keys | Customer-Managed Keys |
|---|---|
| Azure manages the encryption keys | Customer controls the encryption key |
| Minimal management | Requires key management |
| Default approach for managed disks | Requires additional configuration |
| Less operational overhead | Greater control over key lifecycle |

---

# Disk Encryption Set Lab

## Objective

Create a Disk Encryption Set using a customer-managed key stored in Azure Key Vault and use it with a managed disk.

### Architecture

```text
Azure Key Vault
      │
      │ Customer-Managed Key
      ▼
Disk Encryption Set
      │
      ▼
Managed Disk
      │
      ▼
Azure VM
```

### Steps

1. Create an **Azure Key Vault**.
2. Create a customer-managed key in Key Vault.
3. Create a **Disk Encryption Set**.
4. Configure its managed identity.
5. Grant the Disk Encryption Set the required permissions to access the key.
6. Create or select a managed disk.
7. Configure the disk to use the Disk Encryption Set.
8. Attach the disk to an Azure VM.
9. Verify the disk encryption configuration.
10. Test key rotation by updating the key version.

---

# Key Points

- Disk Encryption Set enables customer-managed keys for Azure managed disks.
- Customer-managed keys are stored in Azure Key Vault.
- The Disk Encryption Set uses a managed identity to access the key.
- DES provides greater control over the encryption key lifecycle.
- Customer-managed keys can be rotated.
- Encryption at host provides an additional layer of protection for VM host-side data.
- Platform-managed encryption remains the simpler default option.
