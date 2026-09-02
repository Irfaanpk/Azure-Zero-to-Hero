# 6.11 Azure Storage Explorer

**Azure Storage Explorer** is a graphical tool that allows you to manage Azure Storage resources and data from your local machine.

It can be used to work with:

- Blob containers
- Blobs
- Azure Files
- File shares
- Queues
- Tables

For this AZ-104 section, the main focus is on **Blob Storage and Azure Files**.

---

## Why Use Storage Explorer?

Storage Explorer provides a graphical interface for managing storage data.

Common tasks include:

- Uploading blobs
- Downloading blobs
- Creating containers
- Deleting blobs
- Managing file shares
- Uploading files
- Downloading files
- Viewing blob properties
- Managing storage data

---

## Storage Explorer Authentication

Storage Explorer supports different authentication methods.

Common options include:

- Microsoft Entra ID
- Storage account access keys
- SAS

For Azure resource management, **Microsoft Entra ID** is commonly used.

---

## Storage Explorer Structure

After connecting to Azure, you can browse storage resources.

```text
Azure
└── Storage Accounts
    └── Storage Account
        ├── Blob Containers
        │   └── Containers
        │       └── Blobs
        │
        └── File Shares
            └── Files
```

---

## Storage Explorer vs Azure Portal

| Feature | Azure Portal | Storage Explorer |
|---|---|---|
| Web-based | ✅ | ❌ |
| Desktop application | ❌ | ✅ |
| Manage storage resources | ✅ | ✅ |
| Upload/download blobs | ✅ | ✅ |
| Manage file shares | ✅ | ✅ |
| Browse large amounts of storage data | ✅ | ✅ |
| Works from local machine | Browser | Desktop application |

Storage Explorer is particularly useful when you frequently work with storage data from your local computer.

---

# Install Azure Storage Explorer

Download and install **Azure Storage Explorer** on your local machine.

After installation, launch:

```text
Microsoft Azure Storage Explorer
```

---

# 🧪 Lab: Connect Storage Explorer and Manage Blob Storage

### Step 1: Install Storage Explorer

Download and install Azure Storage Explorer on your local machine.

Launch the application.

---

### Step 2: Connect to Azure

Select:

```text
Sign in with Azure
```

Sign in using your Microsoft Entra account.

Complete the authentication process.

---

### Step 3: Select Your Subscription

After signing in, Storage Explorer displays your Azure subscriptions.

Expand:

```text
Subscriptions
```

Select your Azure subscription.

---

### Step 4: Open Storage Account

Navigate to:

```text
Storage Accounts
    ↓
Your Storage Account
```

Expand the storage account.

---

### Step 5: Open Blob Containers

Navigate to:

```text
Blob Containers
```

Open an existing container.

Example:

```text
images
```

---

### Step 6: Upload a Blob

Select:

```text
Upload
```

Choose:

```text
Upload Files
```

Select a file from your local machine.

Example:

```text
photo.jpg
```

Upload the file.

---

### Step 7: Download a Blob

Select the uploaded blob.

Choose:

```text
Download
```

Select a local destination.

Verify that the file is downloaded successfully.

---

### Step 8: View Blob Properties

Right-click the blob and select:

```text
Properties
```

You can view information such as:

- Blob URL
- Size
- Blob type
- Content type
- Last modified time
- Access tier
- ETag

---

### Step 9: Create a Container

Right-click:

```text
Blob Containers
```

Select:

```text
Create Blob Container
```

Enter:

```text
storage-explorer-lab
```

Create the container.

---

### Step 10: Delete the Test Container

After completing the lab, delete:

```text
storage-explorer-lab
```

if it is no longer required.

---

## Key Points

- **Azure Storage Explorer** is a desktop application for managing Azure Storage.
- It provides a graphical interface for storage data.
- It supports Blob Storage and Azure Files.
- You can authenticate using Microsoft Entra ID, SAS, or access keys.
- Storage Explorer can upload, download, browse, and manage storage data.
- It is useful when managing Azure Storage from a local machine.
- Storage Explorer is different from the Azure Portal but can be used alongside it.
