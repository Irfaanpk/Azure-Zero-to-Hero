# 6.4 Blob Access Levels

**Blob Access Level** controls whether data in a Blob Storage container can be accessed anonymously over the internet.

It is different from the **Blob Access Tier**.

| Concept | Purpose |
|---|---|
| **Access Level** | Controls anonymous/public access |
| **Access Tier** | Controls storage and access costs based on how frequently data is accessed |

---

## Blob Access Levels

Azure Blob Storage provides three anonymous access levels:

- **Private**
- **Blob**
- **Container**

---

## 1. Private

The **Private** access level does not allow anonymous public access.

Users must be authorized to access the blobs.

```text
Internet User
     │
     │ Anonymous Request
     ▼
   Blob Storage
     │
     └── ❌ Access Denied
```

Access can be provided using:

- Microsoft Entra ID
- Azure RBAC
- SAS
- Storage account access keys

### Recommended

Use **Private** when the data should not be publicly accessible.

Examples:

- Backups
- Private documents
- Application data
- Internal files

---

## 2. Blob

The **Blob** access level allows anonymous read access to individual blobs.

A user who knows the blob URL can read the blob.

However, anonymous users **cannot list the blobs inside the container**.

Example:

```text
https://<storage-account>.blob.core.windows.net/images/photo.jpg
```

```text
Known Blob URL
      ↓
Anonymous User
      ↓
   Can Read Blob
```

But:

```text
List Container
      ↓
Anonymous User
      ↓
   ❌ Not Allowed
```

### Use Case

Useful when individual files need to be publicly readable but users should not be able to browse the entire container.

---

## 3. Container

The **Container** access level allows anonymous read access to:

- Blobs
- Container contents

Anonymous users can read blobs and list blobs within the container.

```text
Container
├── photo1.jpg
├── photo2.jpg
└── photo3.jpg

Anonymous User
       ↓
Can Read
Can List
```

### Use Case

Useful when an entire collection of blobs needs to be publicly readable.

---

## Access Level Comparison

| Access Level | Anonymous Read Blob | Anonymous List Blobs |
|---|---:|---:|
| **Private** | ❌ | ❌ |
| **Blob** | ✅ | ❌ |
| **Container** | ✅ | ✅ |

---

## Important Security Point

Public access should only be enabled when required.

For private business data, use:

```text
Private
```

instead of:

```text
Blob
```

or:

```text
Container
```

Anonymous public access can expose data to anyone who can access the public endpoint.

---

## Storage Account Public Access Setting

Blob public access also depends on the **storage account configuration**.

The storage account has a setting that controls whether anonymous access to blob data is permitted.

If anonymous public access is disabled at the storage account level, containers cannot be made publicly accessible anonymously.

Therefore:

```text
Storage Account
      ↓
Allow Blob Anonymous Access?
      ↓
Container Access Level
      ↓
Private / Blob / Container
```

---

# 🧪 Lab: Configure Blob Access Level

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

### Step 2: Check Public Access Setting

Go to:

```text
Settings → Configuration
```

Find:

```text
Allow Blob anonymous access
```

Make sure it is enabled for this lab.

Click:

```text
Save
```

> For production environments, keep anonymous access disabled unless public access is specifically required.

---

### Step 3: Open Containers

Go to:

```text
Data storage → Containers
```

Select your container.

---

### Step 4: Change Access Level

Select:

```text
Change access level
```

You will see:

```text
Private
Blob
Container
```

Select:

```text
Blob
```

Click:

```text
Save
```

---

### Step 5: Test Blob Access

Open the blob and copy its URL.

Example:

```text
https://<storage-account>.blob.core.windows.net/images/photo.jpg
```

Open the URL in a browser where you are not authenticated to Azure.

If the configuration is correct, the blob can be accessed anonymously.

---

### Step 6: Test Container Listing

Try accessing the container URL:

```text
https://<storage-account>.blob.core.windows.net/images
```

With **Blob** access level, anonymous users cannot list the container contents.

---

### Step 7: Change Back to Private

After testing, change the container access level back to:

```text
Private
```

This prevents anonymous public access.

---

## Key Points

- **Blob Access Level** controls anonymous/public access to blob data.
- **Private** allows no anonymous access.
- **Blob** allows anonymous read access to individual blobs.
- **Container** allows anonymous read and list access.
- Anonymous access must also be allowed at the storage account level.
- Access level is different from access tier.
- Use **Private** for sensitive or internal data.
- Public access should only be enabled when explicitly required.
