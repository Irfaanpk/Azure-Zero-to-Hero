# 6.15 Azure File Shares and SMB/NFS

Azure Files provides managed file shares that can be accessed using standard file-sharing protocols.

The main protocols are:

- **SMB**
- **NFS**

---

## SMB

**Server Message Block (SMB)** is a network file-sharing protocol commonly used by Windows systems.

Azure Files supports SMB file shares for:

- Windows
- Linux
- macOS
- Azure Virtual Machines
- On-premises systems

Example:

```text
Windows Client
      ↓
     SMB
      ↓
Azure File Share
```

---

## NFS

**Network File System (NFS)** is a file-sharing protocol commonly used by Linux and Unix-based systems.

Azure Files supports NFS file shares for Linux-based workloads.

Example:

```text
Linux Client
      ↓
     NFS
      ↓
Azure File Share
```

---

## SMB vs NFS

| Feature | SMB | NFS |
|---|---|---|
| Commonly used with | Windows | Linux/Unix |
| Azure Files support | ✅ | ✅ |
| File and directory access | ✅ | ✅ |
| Windows-style authentication | ✅ | ❌ |
| Linux/Unix workloads | ✅ | ✅ |

---

## Mounting an Azure File Share

An Azure File Share can be mounted on a client machine so that it appears like a normal file system.

Example:

```text
Azure File Share
      ↓
   Mount
      ↓
Client Machine
      ↓
Local File System
```

After mounting, users and applications can work with files using normal file-system operations.

---

## SMB File Share Access

SMB shares can be accessed using a UNC path.

Example:

```text
\\<storage-account>.file.core.windows.net\<file-share>
```

Example:

```text
\\azstorage2026demo.file.core.windows.net\azfileshare
```

---

## NFS File Share Access

NFS shares are mounted using the Linux `mount` command.

Example:

```bash
sudo mount -t nfs <storage-account>.file.core.windows.net:/<file-share> /mnt/azfiles
```

The exact mount command and options depend on the Azure Files configuration.

---

## Azure Files Authentication

Access to Azure Files depends on the protocol and configuration.

For SMB, Azure Files supports identity-based authentication using:

- Microsoft Entra Domain Services
- On-premises Active Directory Domain Services
- Microsoft Entra Kerberos in supported scenarios

Storage account keys can also be used for SMB access.

NFS uses network-based access and does not use SMB authentication mechanisms.

---

## Azure Files Permissions

For SMB identity-based access, permissions can be controlled at different levels.

### Share-Level Permissions

Share-level permissions determine whether an identity can access the file share.

### File and Directory Permissions

File and directory permissions control access within the share.

For SMB, Windows-style NTFS permissions can be used.

```text
Azure File Share
      ↓
Share-Level Permission
      ↓
File / Directory Permission
      ↓
Effective Access
```

---

## Network Requirements

Azure Files access can be controlled using:

- Storage account firewall
- Virtual network rules
- Private endpoints

For SMB, the client must be able to reach the storage endpoint using the required network connectivity.

---

# 🧪 Lab: Mount an Azure File Share

## Part 1: Create the File Share

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
Data storage → File shares
```

Create or select:

```text
azfileshare
```

---

## Part 2: Open Connect

Open the file share and select:

```text
Connect
```

Azure provides connection instructions for different operating systems.

You can select:

```text
Windows
```

or:

```text
Linux
```

---

## Part 3: Connect from Windows

Select:

```text
Windows
```

Azure provides a PowerShell command for connecting the file share.

The path follows this format:

```text
\\<storage-account>.file.core.windows.net\<file-share>
```

Run the provided command using the appropriate credentials.

---

## Part 4: Connect from Linux

Select:

```text
Linux
```

Azure provides the appropriate mount instructions.

Create a mount point:

```bash
sudo mkdir -p /mnt/azfiles
```

Then mount the Azure File Share using the command provided by the Azure Portal.

Example:

```bash
sudo mount -t cifs //<storage-account>.file.core.windows.net/<file-share> /mnt/azfiles
```

---

## Part 5: Verify the Mount

On Linux:

```bash
df -h
```

You should see the Azure File Share mounted.

You can also run:

```bash
ls -la /mnt/azfiles
```

---

## Part 6: Create a Test File

Create a file inside the mounted share:

```bash
echo "Azure Files Lab" | sudo tee /mnt/azfiles/test.txt
```

Verify:

```bash
ls -la /mnt/azfiles
```

---

## Part 7: Verify from Azure Portal

Return to:

```text
Storage Account
    ↓
File shares
    ↓
azfileshare
```

Verify that:

```text
test.txt
```

is present.

---

## Important Points

- Azure Files provides managed cloud file shares.
- **SMB** is commonly used for Windows workloads.
- **NFS** is commonly used for Linux and Unix workloads.
- Azure File Shares can be mounted on client machines.
- SMB uses UNC paths such as `\\server\share`.
- Linux clients can mount Azure Files using SMB or NFS depending on the share configuration.
- SMB supports identity-based authentication in supported configurations.
- File and directory permissions control access within SMB file shares.
- Storage account network controls can restrict access to Azure Files.
- Private endpoints can provide private network connectivity to Azure Files.
