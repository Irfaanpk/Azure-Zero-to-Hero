# 6.16 Azure File Sync

**Azure File Sync** is a service that allows you to synchronize files between an **on-premises Windows Server** and an **Azure file share**.

It helps organizations keep frequently accessed files locally while using Azure Storage as a centralized cloud storage location.

---

## Why Use Azure File Sync?

Azure File Sync is useful when an organization already has on-premises file servers but wants to use Azure Storage.

Common use cases include:

- Extending on-premises file servers to Azure
- Centralizing file data in Azure
- Reducing on-premises storage requirements
- Providing local access to frequently used files
- Disaster recovery
- Migrating file servers to Azure

---

## How Azure File Sync Works

Azure File Sync uses an **Azure File Share** as the central cloud storage location.

```text
On-Premises Windows Server
        │
        │ Azure File Sync
        ▼
Azure File Share
```

Files can be synchronized between the local Windows Server and the Azure file share.

---

## Azure File Sync Components

The main components are:

### Storage Sync Service

The **Storage Sync Service** is an Azure resource that coordinates synchronization between Azure file shares and registered Windows Servers.

```text
Storage Sync Service
        │
        ├── Server Endpoint
        │
        └── Azure File Share
```

---

### Azure File Share

The Azure File Share provides the cloud endpoint for synchronized data.

```text
Azure Storage Account
└── Azure File Share
```

---

### Azure File Sync Agent

The **Azure File Sync agent** is installed on the Windows Server.

It enables the server to communicate with Azure File Sync.

```text
Windows Server
      ↓
Azure File Sync Agent
      ↓
Storage Sync Service
      ↓
Azure File Share
```

---

### Registered Server

An on-premises Windows Server must be **registered** with the Storage Sync Service before synchronization can be configured.

```text
Storage Sync Service
        ↓
Registered Server
        ↓
Server Endpoint
```

---

### Sync Group

A **Sync Group** defines the relationship between an Azure file share and one or more server endpoints.

Example:

```text
Sync Group
   │
   ├── Azure File Share
   │
   ├── Server Endpoint 1
   │
   └── Server Endpoint 2
```

---

### Server Endpoint

A **server endpoint** represents a directory on a registered Windows Server that participates in synchronization.

Example:

```text
Windows Server
└── D:\CompanyData
          ↑
    Server Endpoint
```

---

### Cloud Endpoint

A **cloud endpoint** represents the Azure file share used by the Sync Group.

```text
Azure File Share
       ↑
 Cloud Endpoint
```

---

# Sync Group

A **Sync Group** is the logical synchronization boundary.

It contains:

- One cloud endpoint
- One or more server endpoints

Example:

```text
                 Sync Group
                     │
          ┌──────────┴──────────┐
          │                     │
   Cloud Endpoint         Server Endpoint
          │                     │
   Azure File Share      Windows Server
```

---

## Multiple Servers

Azure File Sync can synchronize the same Azure file share with multiple Windows Servers.

Example:

```text
                  Azure File Share
                         │
             ┌───────────┼───────────┐
             │           │           │
             ▼           ▼           ▼
          Server 1    Server 2    Server 3
```

This can allow users at different locations to work with synchronized file data.

---

# Cloud Tiering

**Cloud Tiering** allows Azure File Sync to keep frequently accessed files on the local server while moving less frequently accessed files to Azure Files.

```text
Frequently Used Files
        ↓
Local Windows Server

Less Frequently Used Files
        ↓
Azure File Share
```

A file that has been tiered to Azure can still appear in the local file system as a **cloud tiered file**.

When the file is accessed, Azure File Sync can download the required data to the local server.

---

## Why Use Cloud Tiering?

Cloud tiering can:

- Reduce local storage requirements
- Keep frequently accessed data locally
- Store older or less frequently accessed data in Azure
- Allow users to continue using the normal file-system interface

Example:

```text
Local Server Storage
├── Frequently accessed files
└── File metadata / tiered files

Azure File Share
└── Full cloud copy
```

---

# Azure File Sync vs Azure Files

These are different concepts.

| Feature | Azure Files | Azure File Sync |
|---|---|---|
| Main purpose | Cloud file shares | Synchronize file servers with Azure |
| On-premises server required | ❌ | ✅ |
| SMB support | ✅ | Uses Azure Files |
| Cloud storage | Azure File Share | Azure File Share |
| Synchronization | ❌ | ✅ |
| Cloud tiering | ❌ | ✅ |

---

# Azure File Sync vs File Share Mounting

Mounting an Azure File Share provides direct access to the cloud file share.

```text
Client
   ↓
SMB/NFS
   ↓
Azure File Share
```

Azure File Sync synchronizes data between an on-premises Windows Server and Azure Files.

```text
Windows Server
      ↕
Azure File Sync
      ↕
Azure File Share
```

---

# 🧪 Lab: Configure Azure File Sync

### Step 1: Create an Azure File Share

Open:

```text
https://portal.azure.com
```

Go to:

```text
Storage accounts
    ↓
Your Storage Account
    ↓
File shares
```

Create a file share:

```text
company-data
```

---

### Step 2: Create Storage Sync Service

Search for:

```text
Storage Sync Services
```

Click:

```text
+ Create
```

Configure:

```text
Subscription
Resource Group
Storage Sync Service Name
Region
```

Example:

```text
Storage Sync Service Name:
company-sync
```

Click:

```text
Review + create
```

Then:

```text
Create
```

---

### Step 3: Install Azure File Sync Agent

On your Windows Server, install the **Azure File Sync agent**.

After installation, sign in using your Azure account.

---

### Step 4: Register the Server

Open the Azure File Sync agent on the Windows Server.

Select:

```text
Server Registration
```

Sign in and select:

```text
Subscription
Resource Group
Storage Sync Service
```

Complete the registration.

The Windows Server is now registered with the Storage Sync Service.

---

### Step 5: Create a Sync Group

In the Azure Portal, open:

```text
Storage Sync Service
```

Go to:

```text
Sync groups
```

Click:

```text
+ Sync group
```

Enter:

```text
Sync Group Name:
company-sync-group
```

Select the storage account and Azure file share:

```text
Azure File Share:
company-data
```

Create the Sync Group.

---

### Step 6: Add a Server Endpoint

Open the Sync Group.

Select:

```text
+ Add server endpoint
```

Select the registered Windows Server.

Specify the local path.

Example:

```text
D:\CompanyData
```

Configure cloud tiering if required.

Click:

```text
Create
```

---

### Step 7: Test Synchronization

Create a test file on the Windows Server:

```text
D:\CompanyData\test.txt
```

Wait for synchronization.

Then open:

```text
Azure Portal
    ↓
Storage Account
    ↓
File shares
    ↓
company-data
```

Verify that:

```text
test.txt
```

appears in the Azure file share.

---

### Step 8: Test the Reverse Direction

Create a test file in the Azure file share.

Example:

```text
azure-test.txt
```

Wait for synchronization.

Verify that the file appears on the Windows Server.

---

## Important Points

- **Azure File Sync** synchronizes on-premises Windows Servers with Azure Files.
- A **Storage Sync Service** manages synchronization.
- The **Azure File Sync agent** is installed on Windows Servers.
- A **registered server** must be registered with the Storage Sync Service.
- A **Sync Group** defines the synchronization relationship.
- A **cloud endpoint** represents the Azure file share.
- A **server endpoint** represents a local Windows Server directory.
- Multiple Windows Servers can participate in a Sync Group.
- **Cloud tiering** can reduce local storage requirements.
- Azure File Sync is different from directly mounting an Azure File Share.
