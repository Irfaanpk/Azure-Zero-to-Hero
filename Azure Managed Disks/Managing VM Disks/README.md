# 9.2 Managing VM Disks

Azure Managed Disks can be attached to virtual machines as data disks. You can initialize, partition, format, mount, resize, and manage these disks according to the operating system.

---

## Attaching a Data Disk

A data disk can be attached to an existing Azure VM.

```text
Azure VM
   │
   ├── OS Disk
   │
   └── Data Disk
```

A data disk is commonly used for:

- Application data
- Database files
- Logs
- Documents
- Additional storage

---

## Detaching a Data Disk

A data disk can be detached from a VM when it is no longer required.

```text
Before:

VM
 ├── OS Disk
 └── Data Disk

        ↓ Detach

VM
 └── OS Disk
```

The managed disk remains as an Azure resource after detaching it.

---

# Linux Disk Management

After attaching a data disk to a Linux VM, the disk must be identified and prepared before it can be used.

### Basic Workflow

```text
Attach Disk
     ↓
Identify Disk
     ↓
Create Partition
     ↓
Format Filesystem
     ↓
Create Mount Point
     ↓
Mount Disk
     ↓
Configure /etc/fstab
     ↓
Permanent Mount
```

---

## Identify the Disk

Use:

```bash
lsblk
```

or:

```bash
sudo fdisk -l
```

These commands help identify the newly attached disk.

---

## Create a Partition

Example:

```bash
sudo fdisk /dev/sdc
```

Create the required partition and verify it:

```bash
lsblk
```

Example:

```text
sdc
└── sdc1
```

---

## Format the Partition

Format the partition with a filesystem.

Example using `ext4`:

```bash
sudo mkfs.ext4 /dev/sdc1
```

---

## Create a Mount Point

Create a directory:

```bash
sudo mkdir /data
```

Mount the disk:

```bash
sudo mount /dev/sdc1 /data
```

Verify:

```bash
df -h
```

---

# Permanent Mount Using /etc/fstab

A normal mount may not automatically remain after a reboot.

To configure a permanent mount, use:

```bash
/etc/fstab
```

First find the disk UUID:

```bash
sudo blkid /dev/sdc1
```

Example:

```text
/dev/sdc1: UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" TYPE="ext4"
```

Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

Add:

```text
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /data ext4 defaults,nofail 0 2
```

Test the configuration:

```bash
sudo mount -a
```

Verify:

```bash
df -h
```

Reboot the VM:

```bash
sudo reboot
```

After reboot:

```bash
df -h
```

The disk should be automatically mounted at:

```text
/data
```

> Using the UUID instead of `/dev/sdc1` is recommended because device names can change after reboot.

---

# Windows Disk Management

After attaching a data disk to a Windows VM, the disk must be initialized and formatted before it can be used.

### Basic Workflow

```text
Attach Disk
     ↓
Initialize Disk
     ↓
Create Volume
     ↓
Format Volume
     ↓
Assign Drive Letter
     ↓
Use Disk
```

---

## Open Disk Management

Inside the Windows VM:

1. Press `Win + R`.
2. Run:

```text
diskmgmt.msc
```

3. Locate the newly attached disk.
4. Initialize the disk.
5. Create a new volume.
6. Format the volume.
7. Assign a drive letter.

Example:

```text
Disk 0 → OS Disk
Disk 1 → Data Disk → D:
```

---

## Verify the Disk

Open **File Explorer** and verify the new drive.

Example:

```text
This PC
 ├── C: OS
 └── D: Data
```

The data disk and its drive letter remain available after a normal VM reboot.

---

# Resizing a Managed Disk

Managed disks can be resized when additional capacity is required.

```text
100 GB
  ↓
200 GB
```

### Azure Portal

1. Open the VM.
2. Select **Disks**.
3. Select the required managed disk.
4. Increase the disk size.
5. Save the changes.

> Increasing the Azure disk size does not always automatically expand the partition and filesystem inside the operating system. The OS-level volume may also need to be extended.

---

# Linux Lab

## Lab: Attach and Permanently Mount a Data Disk

### Objective

Attach a managed data disk to a Linux VM, format it, mount it, configure permanent mounting, and resize it.

### Steps

1. Create a managed data disk.
2. Attach it to a Linux VM.
3. Connect to the VM using SSH.
4. Identify the disk:

```bash
lsblk
```

5. Create a partition.
6. Format it using `ext4`.
7. Create a mount point:

```bash
sudo mkdir /data
```

8. Mount the partition.
9. Find its UUID:

```bash
sudo blkid
```

10. Add the UUID to:

```bash
/etc/fstab
```

11. Test:

```bash
sudo mount -a
```

12. Reboot the VM.
13. Verify that `/data` is automatically mounted.
14. Resize the managed disk from Azure Portal.
15. Extend the partition and filesystem inside Linux.
16. Verify the new capacity.

---

# Windows Lab

## Lab: Attach and Manage a Data Disk

### Objective

Attach a managed data disk to a Windows VM, initialize it, format it, assign a drive letter, and resize it.

### Steps

1. Create a managed data disk.
2. Attach it to a Windows VM.
3. Connect to the VM using RDP.
4. Open:

```text
diskmgmt.msc
```

5. Initialize the new disk.
6. Create a new volume.
7. Format the volume.
8. Assign a drive letter:

```text
D:
```

9. Create a test file on the new drive.
10. Reboot the VM.
11. Verify that the drive is still available.
12. Resize the managed disk from Azure Portal.
13. Extend the Windows volume.
14. Verify the new disk capacity.

---

# Key Points

- Data disks provide additional persistent storage for Azure VMs.
- A disk must be prepared before it can be used by the operating system.
- Linux uses partitions, filesystems, and mount points.
- `/etc/fstab` can be used for permanent Linux disk mounting.
- Using UUIDs in `/etc/fstab` is recommended.
- Windows disks can be managed through Disk Management.
- Increasing the Azure disk size may require extending the OS partition and filesystem.
- A detached managed disk remains available as an Azure resource.
