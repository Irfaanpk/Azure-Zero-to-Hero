# 8.4 SSH Keys and VM Access

## SSH Keys

SSH (Secure Shell) is used to securely connect to Linux Azure Virtual Machines.

Azure supports SSH key authentication for Linux VMs.

SSH uses a **public key and private key pair**:

```text
Public Key
    ↓
Stored on Azure VM

Private Key
    ↓
Stored securely on your local machine
```

The private key should never be shared.

---

## Public Key and Private Key

### Public Key

- Can be shared
- Added to the Linux VM
- Used to verify the private key

### Private Key

- Must be kept secure
- Stored on your local machine
- Used when connecting to the VM

```text
Local Machine
     │
     │ Private Key
     ↓
Azure Linux VM
     │
     │ Public Key
     ↓
Authentication
```

---

## SSH Access to Linux VM

Linux VMs can be accessed using SSH.

Example:

```bash
ssh username@PUBLIC_IP
```

Example:

```bash
ssh azureuser@20.10.10.10
```

The SSH client uses the private key to authenticate with the VM.

---

## PuTTY

**PuTTY** is a popular SSH client for Windows.

It can be used to connect to Linux Azure VMs using SSH.

Basic flow:

```text
Windows Machine
      ↓
    PuTTY
      ↓
SSH Authentication
      ↓
Azure Linux VM
```

---

## PuTTYgen

**PuTTYgen** is a tool included with PuTTY that can generate and manage SSH key pairs.

It can be used to:

- Generate SSH keys
- Load existing keys
- Save private keys
- Convert key formats when required

```text
PuTTYgen
   ↓
Generate SSH Key Pair
   ↓
Public Key → Azure VM
Private Key → Local Machine
   ↓
PuTTY → SSH → Linux VM
```

---

## SSH Access Using PuTTY

Basic process:

1. Create or obtain an SSH key pair.
2. Add the public key when creating the Linux VM.
3. Keep the private key securely on your Windows machine.
4. Open **PuTTY**.
5. Enter the VM's public IP address.
6. Configure the SSH connection.
7. Select the private key using **Connection → SSH → Auth → Credentials**.
8. Connect to the VM.
9. Enter the VM username.

---

## RDP Access to Windows VM

Windows Azure VMs can be accessed using **RDP (Remote Desktop Protocol)**.

Basic flow:

```text
Windows Machine
      ↓
Remote Desktop (RDP)
      ↓
Azure Windows VM
```

You normally connect using:

```text
Public IP Address
+
Username
+
Password
```

RDP uses TCP port **3389** by default.

---

## Password Authentication

Azure VMs can use username and password authentication where supported.

Example:

```text
Username
   +
Password
   ↓
Authentication
   ↓
VM Access
```

For Linux VMs, SSH key authentication is generally preferred over password authentication.

---

## SSH Key vs Password Authentication

| Feature | SSH Key | Password |
|---|---|---|
| Authentication | Key pair | Username + password |
| Security | Strong | Depends on password strength |
| Linux SSH | Commonly preferred | Supported |
| Private credential | Private key | Password |
| Sharing | Private key should never be shared | Password should never be shared |

---

## Azure Bastion

**Azure Bastion** provides secure RDP and SSH access to Azure VMs without requiring a public IP address on the VM.

```text
User
 ↓
Azure Portal
 ↓
Azure Bastion
 ↓
Private IP
 ↓
Azure VM
```

Bastion is covered in detail in:

**Section 7.12 — Azure Bastion**

---

## Run Command

**Azure Run Command** allows you to execute commands inside an Azure VM from the Azure Portal without directly connecting through SSH or RDP.

It can be useful when:

- SSH/RDP is not working
- You need to run a command remotely
- You need to troubleshoot a VM
- You need to perform administrative tasks

---

## Reset SSH or Password Access

If you lose access to a VM, Azure provides options to reset or modify access credentials.

For example:

- Reset SSH configuration
- Reset SSH keys
- Reset user password
- Create or modify a user

These operations can be performed using Azure Portal or Azure CLI depending on the situation.

---

## Practical Lab

### Lab: Connect to a Linux VM Using SSH

1. Create a Linux Azure VM.
2. Select **SSH public key** authentication.
3. Create or use an existing SSH key pair.
4. Save the private key securely.
5. Create the VM.
6. Copy the VM's public IP address.
7. Connect using SSH:

```bash
ssh username@PUBLIC_IP
```

### Windows Using PuTTY

1. Install PuTTY.
2. Open PuTTY.
3. Enter the VM public IP address.
4. Set the port to `22`.
5. Select **SSH**.
6. Configure the private key under **SSH → Auth → Credentials**.
7. Connect to the VM.
8. Enter the VM username.

### Windows VM Using RDP

1. Create a Windows VM.
2. Configure administrator credentials.
3. Copy the VM public IP address.
4. Open **Remote Desktop Connection**.
5. Enter the public IP address.
6. Enter the administrator credentials.
7. Connect to the VM.

---

## Key Points

- SSH is commonly used to access Linux VMs.
- SSH key authentication uses a public/private key pair.
- The private key must be kept secure.
- PuTTY can be used for SSH access from Windows.
- PuTTYgen can generate and manage SSH keys.
- RDP is commonly used to access Windows VMs.
- Azure Bastion provides browser-based SSH/RDP access without exposing VM management ports.
- Run Command allows commands to be executed inside a VM without direct SSH/RDP access.
