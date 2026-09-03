# 8.6 Web Servers on Azure VMs

## What is a Web Server?

A **web server** is software that receives HTTP/HTTPS requests from clients and returns web content.

```text
Client
   │
   │ HTTP / HTTPS
   ▼
Azure VM
   │
   ▼
Web Server
   │
   ▼
Website / Web Application
```

A web server can host:

- HTML pages
- CSS
- JavaScript
- Images
- Web applications
- APIs

---

# Common Web Servers

The most common web servers used on Azure VMs include:

| Web Server | Common OS | Description |
|---|---|---|
| Apache HTTP Server | Linux | Popular open-source web server |
| Nginx | Linux | High-performance web server and reverse proxy |
| IIS | Windows | Microsoft's web server for Windows |

---

# Apache

**Apache HTTP Server** is a widely used open-source web server.

It is commonly deployed on Linux VMs.

```text
Linux VM
   │
   ▼
Apache
   │
   ▼
Website
```

Common use cases:

- Static websites
- PHP applications
- Web applications
- APIs

---

# Nginx

**Nginx** is a high-performance web server that can also work as a reverse proxy and load balancer.

It is commonly used on Linux VMs.

```text
Client
   │
   ▼
Nginx
   │
   ├── Static Content
   │
   └── Application Server
```

Common use cases:

- Static websites
- Reverse proxy
- Web applications
- APIs
- High-traffic websites

---

# IIS

**Internet Information Services (IIS)** is Microsoft's web server for Windows Server.

```text
Windows VM
    │
    ▼
   IIS
    │
    ▼
 Website
```

Common use cases:

- ASP.NET applications
- .NET applications
- Windows-based web applications
- Websites requiring Windows-specific services

---

# HTTP and HTTPS

Web servers commonly listen for:

- **HTTP** — TCP port `80`
- **HTTPS** — TCP port `443`

```text
HTTP
Port 80
   │
   ▼
Web Server
```

```text
HTTPS
Port 443
   │
   ▼
Web Server
```

HTTPS provides encrypted communication between the client and the web server.

---

# Azure VM Web Server Architecture

A basic web server deployment can look like:

```text
                    Internet
                       │
                       ▼
                 Public IP
                       │
                       ▼
                  Azure NIC
                       │
                       ▼
                  Azure VM
                       │
                       ▼
                  Web Server
                 /          \
              Apache       Nginx
```

For Windows:

```text
Internet
    │
    ▼
Public IP
    │
    ▼
Azure VM
    │
    ▼
IIS
    │
    ▼
Website
```

---

# Network Access

A web server must be reachable on the required ports.

For example:

```text
HTTP  → TCP 80
HTTPS → TCP 443
```

Azure networking controls such as **Network Security Groups (NSGs)** can allow or deny traffic to the VM.

Example:

```text
Internet
    │
    │ TCP 80 / 443
    ▼
   NSG
    │
    │ Allow
    ▼
   NIC
    │
    ▼
   VM
    │
    ▼
Web Server
```

> NSGs and Azure networking are covered in detail in **Section 7 — Azure Virtual Network**.

---

# Installing a Web Server on a Linux VM

A web server can be installed after creating a Linux VM.

For example, Apache can be installed using the package manager.

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install apache2 -y
```

Start Apache:

```bash
sudo systemctl start apache2
```

Enable Apache to start automatically:

```bash
sudo systemctl enable apache2
```

Check the status:

```bash
sudo systemctl status apache2
```

---

# Installing Nginx on Linux

For Ubuntu / Debian:

```bash
sudo apt update
sudo apt install nginx -y
```

Start Nginx:

```bash
sudo systemctl start nginx
```

Enable Nginx at boot:

```bash
sudo systemctl enable nginx
```

Check the status:

```bash
sudo systemctl status nginx
```

---

# Installing IIS on Windows VM

IIS can be installed on a Windows Server VM using **Server Manager** or PowerShell.

Example PowerShell command:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

After installation, verify that the IIS service is running.

---

# Testing the Web Server

After installing the web server:

1. Obtain the VM's public IP address.
2. Make sure the required port is allowed.
3. Open a web browser.
4. Enter:

```text
http://PUBLIC_IP
```

Example:

```text
http://20.10.10.10
```

If the web server is configured correctly, the default web page should appear.

---

# Hosting a Simple Website

You can replace the default web server page with your own HTML file.

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Azure VM Website</title>
</head>
<body>
    <h1>Hello from Azure VM</h1>
</body>
</html>
```

The web server serves this file when a client sends an HTTP request.

---

# Apache Default Web Directory

On many Ubuntu/Debian installations, Apache uses:

```text
/var/www/html/
```

Example:

```bash
sudo nano /var/www/html/index.html
```

Add:

```html
<h1>Hello from Azure VM</h1>
```

Then access:

```text
http://PUBLIC_IP
```

---

# Nginx Default Web Directory

On many Ubuntu/Debian installations, Nginx uses:

```text
/var/www/html/
```

Example:

```bash
sudo nano /var/www/html/index.html
```

Add:

```html
<h1>Hello from Nginx on Azure</h1>
```

Then access the VM's public IP from a browser.

---

# IIS Default Website

IIS commonly serves website content from:

```text
C:\inetpub\wwwroot
```

You can place an HTML file inside this directory.

Example:

```html
<h1>Hello from IIS on Azure VM</h1>
```

Then access:

```text
http://PUBLIC_IP
```

---

# Apache vs Nginx vs IIS

| Feature | Apache | Nginx | IIS |
|---|---|---|---|
| Platform | Mainly Linux/Unix | Mainly Linux/Unix | Windows |
| Open Source | Yes | Yes | No |
| Common Use | Web hosting | Web hosting / Reverse Proxy | Windows web applications |
| .NET Integration | Possible | Possible | Strong |
| Configuration | File-based | File-based | GUI + configuration |
| Common Port | 80 / 443 | 80 / 443 | 80 / 443 |

---

# Web Server vs Web Application

A web server and a web application are not always the same thing.

```text
Client
   │
   ▼
Web Server
   │
   ▼
Web Application
   │
   ▼
Database
```

For example:

```text
Nginx
   ↓
Application
   ↓
Database
```

The web server can receive requests and forward them to the application.

---

# Practical Lab

## Lab: Host a Website Using Apache on an Azure Linux VM

### Objective

Create a Linux VM, install Apache, allow HTTP traffic, and host a simple website.

### Step 1: Create a Linux VM

1. Open **Azure Portal**.
2. Go to **Virtual Machines**.
3. Select **Create → Azure virtual machine**.
4. Select a Linux image such as Ubuntu.
5. Configure the VM.
6. Select SSH authentication.
7. Allow **HTTP (80)** in the networking configuration.
8. Create the VM.

---

### Step 2: Connect to the VM

Connect using SSH:

```bash
ssh username@PUBLIC_IP
```

---

### Step 3: Install Apache

```bash
sudo apt update
sudo apt install apache2 -y
```

---

### Step 4: Verify Apache

```bash
sudo systemctl status apache2
```

---

### Step 5: Create a Website

Edit the default page:

```bash
sudo nano /var/www/html/index.html
```

Add:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Azure VM</title>
</head>
<body>
    <h1>Hello from Azure Virtual Machine</h1>
</body>
</html>
```

Save the file.

---

### Step 6: Test the Website

Open a browser and enter:

```text
http://PUBLIC_IP
```

You should see:

```text
Hello from Azure Virtual Machine
```

---

# Practical Lab: Nginx

You can repeat the same concept using Nginx.

Install:

```bash
sudo apt update
sudo apt install nginx -y
```

Start:

```bash
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

Then open:

```text
http://PUBLIC_IP
```

---

# Practical Lab: IIS

For a Windows VM:

1. Create a Windows Server VM.
2. Connect using RDP.
3. Open **Server Manager**.
4. Select **Add Roles and Features**.
5. Select **Web Server (IIS)**.
6. Complete the installation.
7. Open a browser inside the VM.
8. Browse to:

```text
http://localhost
```

9. Verify the IIS default page.
10. Add your own website files under:

```text
C:\inetpub\wwwroot
```

---

# Key Points

- A web server receives HTTP/HTTPS requests and serves web content.
- Apache and Nginx are commonly used on Linux VMs.
- IIS is Microsoft's web server for Windows.
- HTTP commonly uses TCP port `80`.
- HTTPS commonly uses TCP port `443`.
- NSGs control network access to the VM.
- Apache commonly uses `/var/www/html/` for website files on Ubuntu/Debian.
- Nginx commonly uses `/var/www/html/` for website files on Ubuntu/Debian.
- IIS commonly uses `C:\inetpub\wwwroot`.
- Azure VMs can host static websites, web applications, and APIs.
