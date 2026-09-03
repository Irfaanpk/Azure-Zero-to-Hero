# 8.8 VM Extensions and Custom Script

## What are VM Extensions?

**Azure VM Extensions** are small components that can be installed on Azure Virtual Machines to perform configuration and automation tasks.

They run inside the VM and can be used to:

- Install software
- Run scripts
- Configure applications
- Collect monitoring information
- Automate VM configuration

```text
Azure VM
   │
   └── VM Extension
          │
          ├── Install Software
          ├── Run Script
          └── Configure VM
```

---

# Why Use VM Extensions?

Without extensions, administrators may need to connect to every VM and manually configure it.

```text
Without Extension

Create VM
   ↓
Connect to VM
   ↓
Install Software
   ↓
Configure Application
   ↓
Repeat for other VMs
```

With VM Extensions:

```text
Create VM
   ↓
VM Extension
   ↓
Automatic Configuration
```

This is especially useful when deploying multiple VMs.

---

# Common Azure VM Extensions

Azure provides different extensions for different tasks.

| Extension | Purpose |
|---|---|
| Custom Script Extension | Run scripts inside a VM |
| Azure Monitor Agent | Collect monitoring data |
| Microsoft Antimalware | Malware protection for Windows VMs |
| Dependency Agent | Collect dependency information |
| VM Access Extension | Manage users and access |

For AZ-104, **Custom Script Extension** is especially important for VM configuration and automation.

---

# Custom Script Extension

The **Custom Script Extension** allows you to run scripts inside an Azure VM.

It can be used to:

- Install packages
- Install web servers
- Configure applications
- Create files
- Run commands
- Perform post-deployment configuration

Example:

```text
Azure VM
   ↓
Custom Script Extension
   ↓
Install Nginx
   ↓
Configure Nginx
   ↓
Web Server Ready
```

---

# Linux Custom Script

A shell script can be executed on a Linux VM.

Example:

```bash
#!/bin/bash

apt update
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

The script can automatically install and configure Nginx.

---

# Windows Custom Script

A PowerShell script can be executed on a Windows VM.

Example:

```powershell
Install-WindowsFeature -Name Web-Server
```

This can automatically install IIS on a Windows VM.

---

# VM Extension Execution Flow

```text
Administrator
      │
      ▼
Azure Portal / Azure CLI
      │
      ▼
VM Extension
      │
      ▼
Script / Configuration
      │
      ▼
Azure VM
      │
      ▼
Task Completed
```

---

# Custom Script Extension vs Run Command

Both can execute commands inside a VM, but they are commonly used for different purposes.

| Custom Script Extension | Run Command |
|---|---|
| VM configuration and automation | Run commands on an existing VM |
| Can be part of VM deployment | Useful for troubleshooting and administration |
| Can execute scripts | Can execute commands/scripts |
| Useful for repeatable configuration | Useful for ad-hoc tasks |

---

# Run Command

**Azure Run Command** allows administrators to execute commands inside a VM without directly connecting through SSH or RDP.

Example:

```text
Azure Portal
     ↓
Run Command
     ↓
Execute Script
     ↓
Azure VM
```

It can be useful when:

- SSH is not working
- RDP is not working
- You need to troubleshoot a VM
- You need to execute an administrative command

---

# VM Extension Lifecycle

VM extensions are associated with a VM and can be:

- Installed
- Configured
- Updated
- Removed

Example:

```text
VM Created
    ↓
Install Extension
    ↓
Configure Extension
    ↓
Extension Runs
    ↓
Update / Remove
```

---

# Important Considerations

VM Extensions execute operations inside the VM, so the script should be tested before using it in production.

Consider:

- Script correctness
- Required permissions
- Network connectivity
- Package availability
- Execution time
- Idempotency

For example, a script should ideally be safe to run more than once without causing unexpected results.

---

# Practical Lab

## Lab: Install Nginx Using Custom Script Extension

### Objective

Use the Custom Script Extension to automatically install Nginx on an Azure Linux VM.

---

### Step 1: Create a Linux VM

1. Open **Azure Portal**.
2. Go to **Virtual Machines**.
3. Create a Linux VM.
4. Select an Ubuntu image.
5. Configure authentication.
6. Allow HTTP traffic on port `80`.
7. Create the VM.

---

### Step 2: Open VM Extensions

1. Open the created VM.
2. In the left menu, select **Extensions + applications**.
3. Select **Create**.
4. Choose **Custom Script for Linux**.

---

### Step 3: Provide the Script

Use:

```bash
#!/bin/bash

apt update
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

---

### Step 4: Deploy the Extension

1. Review the configuration.
2. Select **Review + create**.
3. Deploy the extension.
4. Wait for the deployment to complete.

---

### Step 5: Verify

Open the VM's public IP address:

```text
http://PUBLIC_IP
```

The Nginx default page should appear.

You can also verify through SSH:

```bash
systemctl status nginx
```

---

# Practical Lab: Run Command

## Objective

Execute a command inside an existing Linux VM without using SSH.

### Steps

1. Open the Azure VM.
2. Select **Run command**.
3. Select **RunShellScript**.
4. Enter:

```bash
hostname
```

5. Select **Run**.
6. Review the output.

Example:

```text
Output:
azure-vm-01
```

---

# VM Extensions vs Manual Configuration

```text
Manual Configuration

Create VM
   ↓
SSH / RDP
   ↓
Install Software
   ↓
Configure
```

```text
Automated Configuration

Create VM
   ↓
VM Extension
   ↓
Script
   ↓
Software Installed
   ↓
Configuration Complete
```

---

# Key Points

- VM Extensions provide a way to perform automated tasks inside Azure VMs.
- Extensions can install software and configure applications.
- **Custom Script Extension** can execute shell or PowerShell scripts.
- Custom Script Extension is useful for automated VM configuration.
- **Run Command** allows commands to be executed inside an existing VM without SSH or RDP.
- VM Extensions can be managed through the Azure Portal, Azure CLI, and other deployment methods.
- Scripts should be tested carefully before production use.
- VM Extensions are useful when the same configuration needs to be applied to multiple VMs.
