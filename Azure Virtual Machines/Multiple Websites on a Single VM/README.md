# Multiple Websites on a Single VM

## What is Hosting Multiple Websites on a Single VM?

A single Azure VM can host multiple websites.

Instead of creating a separate VM for every website, multiple websites can run on the same VM using web server configuration.

```text
                    Azure VM
                       │
                Web Server
                       │
          ┌────────────┼────────────┐
          │            │            │
       Website A    Website B    Website C
```

This can help reduce:

- Infrastructure costs
- Number of VMs to manage
- Resource usage

---

# How Multiple Websites Work

The web server determines which website should handle a request based on information such as:

- Hostname
- Port
- IP address

The most common approach is **hostname-based hosting**.

```text
www.site1.com
       │
       ▼
    Web Server
       │
       ▼
   Website 1
```

```text
www.site2.com
       │
       ▼
    Web Server
       │
       ▼
   Website 2
```

Both websites can run on the same VM.

---

# Hostname-Based Hosting

With hostname-based hosting, different domain names point to the same VM.

Example:

```text
www.site1.com ─────┐
                   │
www.site2.com ─────┼──► Azure VM
                   │
www.site3.com ─────┘
```

The web server checks the requested hostname and serves the corresponding website.

---

# Example

Suppose one Azure VM hosts three websites:

```text
VM Public IP: 20.10.10.10

www.site1.com
www.site2.com
www.site3.com
```

All three DNS records can point to:

```text
20.10.10.10
```

The web server then routes requests:

```text
www.site1.com
      ↓
Website 1

www.site2.com
      ↓
Website 2

www.site3.com
      ↓
Website 3
```

---

# Port-Based Hosting

Multiple websites can also be hosted using different ports.

Example:

```text
Website 1 → Port 80
Website 2 → Port 8080
Website 3 → Port 8081
```

```text
Client
  │
  ├── :80
  │     ↓
  │  Website 1
  │
  ├── :8080
  │     ↓
  │  Website 2
  │
  └── :8081
        ↓
     Website 3
```

However, hostname-based hosting is generally more convenient for normal websites because users can access them using standard HTTP/HTTPS ports.

---

# Virtual Hosts

Web servers use **virtual hosts** to host multiple websites on the same server.

For example:

```text
Azure VM
   │
   ▼
Apache
   │
   ├── site1.com → /var/www/site1
   │
   ├── site2.com → /var/www/site2
   │
   └── site3.com → /var/www/site3
```

Each website can have its own:

- Domain name
- Website directory
- Configuration
- Logs
- Application

---

# Apache Virtual Hosts

Apache uses virtual host configurations to identify different websites.

Example:

```text
site1.com
    ↓
/var/www/site1

site2.com
    ↓
/var/www/site2
```

A simplified Apache configuration looks like:

```apache
<VirtualHost *:80>
    ServerName site1.com
    DocumentRoot /var/www/site1
</VirtualHost>

<VirtualHost *:80>
    ServerName site2.com
    DocumentRoot /var/www/site2
</VirtualHost>
```

Apache checks the requested hostname and serves the appropriate website.

---

# Nginx Server Blocks

Nginx uses **server blocks** for hosting multiple websites.

Example:

```text
site1.com
    ↓
/var/www/site1

site2.com
    ↓
/var/www/site2
```

A simplified configuration looks like:

```nginx
server {
    listen 80;
    server_name site1.com;

    root /var/www/site1;
}

server {
    listen 80;
    server_name site2.com;

    root /var/www/site2;
}
```

---

# IIS Multiple Websites

IIS can also host multiple websites on a single Windows VM.

IIS uses **bindings** to determine which website should handle a request.

A binding can include:

- IP address
- Port
- Hostname
- Protocol

Example:

```text
IIS
 │
 ├── site1.com → Port 80
 │
 └── site2.com → Port 80
```

IIS uses the hostname binding to select the correct website.

---

# DNS Configuration

For hostname-based websites, the domain names must resolve to the VM.

Example:

```text
www.site1.com
       ↓
20.10.10.10

www.site2.com
       ↓
20.10.10.10
```

DNS does not determine which website to serve.

It only resolves the domain name to the VM's IP address.

The web server performs the final website selection using the hostname.

---

# Multiple Websites Architecture

```text
                         Internet
                            │
             ┌──────────────┴──────────────┐
             │                             │
      www.site1.com                  www.site2.com
             │                             │
             └──────────────┬──────────────┘
                            │
                         DNS
                            │
                            ▼
                    Azure VM Public IP
                            │
                            ▼
                       Web Server
                      /           \
                     /             \
               Website 1        Website 2
               /var/www/site1   /var/www/site2
```

---

# HTTP Request Flow

When a user accesses:

```text
http://www.site1.com
```

The process is:

```text
1. User enters www.site1.com
             ↓
2. DNS resolves the domain
             ↓
3. Request reaches Azure VM
             ↓
4. Web server receives request
             ↓
5. Web server checks hostname
             ↓
6. Matching website configuration is selected
             ↓
7. Website content is returned
```

---

# Multiple Websites vs Multiple VMs

| Multiple Websites on One VM | Separate VM per Website |
|---|---|
| Lower infrastructure cost | Higher cost |
| Easier to manage initially | More infrastructure to manage |
| Shares VM resources | Dedicated resources |
| Failure affects all websites | Failure usually affects one website |
| Suitable for smaller workloads | Suitable for isolated workloads |

---

# Multi-Server in a Single Azure VM Using Nginx

Below is the GitHub-ready `README.md` content. Copy and paste it directly into your repository.

# Multi-Server in a Single Azure VM Using Nginx

## Project Overview

This project demonstrates how to host multiple websites on a single Azure Linux Virtual Machine using multiple Private IP addresses, Public IP addresses, and Nginx.

Instead of creating multiple Virtual Machines, we use a single VM with multiple network IP configurations to host different websites.

### Architecture

```
                         Internet
                            |
              +-------------+-------------+
              |                           |
         Public IP 1                 Public IP 2
              |                           |
         Private IP 1                Private IP 2
              |                           |
              +-------------+-------------+
                            |
                   Azure Linux VM
                            |
                          Nginx
                       Port 80
                      /        \
                     /          \
              Website 1      Website 2
             /var/www/site1 /var/www/site2
```

## Technologies Used

- Microsoft Azure
- Azure Virtual Machine
- Azure Virtual Network
- Network Interface (NIC)
- Private IP Address
- Public IP Address
- Network Security Group (NSG)
- Ubuntu Linux
- Nginx Web Server

## Prerequisites

- Active Azure Subscription
- Ubuntu Linux Virtual Machine
- SSH access to the VM
- Azure Virtual Network and Subnet
- Two Public IP resources

## Implementation Steps

### Step 1: Create an Azure Virtual Machine

1. Open Azure Portal.
2. Navigate to Virtual Machines.
3. Click Create → Azure Virtual Machine.
4. Select Ubuntu Server 24.04 LTS.
5. Configure the VM size and SSH authentication.
6. Select a Virtual Network and Subnet.
7. Create the VM with an initial Public IP.
8. Complete the deployment.

### Step 2: Create a Secondary Private IP

1. Open the Virtual Machine.
2. Navigate to Networking.
3. Open the attached Network Interface (NIC).
4. Select IP configurations.
5. Click Add.
6. Enter the configuration name as `ipconfig2`.
7. Select Dynamic private IP assignment.
8. Save the configuration.

Example:

| IP Configuration | Private IP |
| ---------------- | ---------- |
| Primary          | 10.0.1.4   |
| Secondary        | 10.0.1.5   |

### Step 3: Create a Second Public IP

1. Search for Public IP addresses in Azure Portal.
2. Click Create.
3. Configure the following:

| Setting    | Value        |
| ---------- | ------------ |
| Name       | public-ip-02 |
| IP Version | IPv4         |
| SKU        | Standard     |
| Assignment | Static       |
| Region     | Same as VM   |

4. Create the Public IP resource.
5. Open the VM's Network Interface.
6. Navigate to IP configurations.
7. Select `ipconfig2`.
8. Associate the newly created Public IP.
9. Save the configuration.

Example:

| Configuration | Private IP | Public IP   |
| ------------- | ---------- | ----------- |
| Primary       | 10.0.1.4   | 20.10.10.10 |
| Secondary     | 10.0.1.5   | 20.20.20.20 |

*Note: The IP addresses are examples. Use your actual Azure-assigned addresses.*

### Step 4: Verify Multiple IP Addresses

Connect to the Azure VM:

```
ssh -i key.pem azureuser@<public-ip>
```

Check network interfaces:

```
ip a
```

Check routing:

```
ip route
```

If the secondary private IP is not configured inside the Ubuntu guest OS, configure it for this lab:

```
sudo ip addr add 10.0.1.5/24 dev eth0
```

Replace the example IP, subnet prefix, and interface name with your actual values.

Verify:

```
ip addr show eth0
```

Note: This command is temporary. Configure the secondary IP persistently using the appropriate Ubuntu Netplan configuration when required.

### Step 5: Install Nginx

Update the package repository:

```
sudo apt update
```

Install Nginx:

```
sudo apt install nginx -y
```

Enable Nginx:

```
sudo systemctl enable nginx
```

Start Nginx:

```
sudo systemctl start nginx
```

Verify:

```
sudo systemctl status nginx
```

### Step 6: Create Website Directories

Create two directories:

```
sudo mkdir -p /var/www/site1
sudo mkdir -p /var/www/site2
```

Create Website 1:

```
echo "Welcome to Azure Site 1" | sudo tee /var/www/site1/index.html
```

Create Website 2:

```
echo "Welcome to Azure Site 2" | sudo tee /var/www/site2/index.html
```

Set permissions:

```
sudo chown -R www-data:www-data /var/www/site1
sudo chown -R www-data:www-data /var/www/site2
```

### Step 7: Configure Nginx for Multiple IP Addresses

Create the configuration file:

```
sudo nano /etc/nginx/sites-available/multisite
```

Add the following configuration:

```
server {
    listen 10.0.1.4:80;
    server_name _;

    root /var/www/site1;
    index index.html;
}

server {
    listen 10.0.1.5:80;
    server_name _;

    root /var/www/site2;
    index index.html;
}
```

Replace the IP addresses with your actual private IP addresses.

Enable the configuration:

```
sudo ln -s /etc/nginx/sites-available/multisite /etc/nginx/sites-enabled/multisite
```

Disable the default configuration if it conflicts:

```
sudo rm -f /etc/nginx/sites-enabled/default
```

Test the configuration:

```
sudo nginx -t
```

Restart Nginx:

```
sudo systemctl restart nginx
```

Verify listening ports:

```
sudo ss -lntp | grep ':80'
```

Important: Nginx must be able to bind to both private IP addresses. If it reports `Cannot assign requested address`, verify the secondary IP inside the Linux guest OS.

### Step 8: Configure Network Security Group

1. Open the Virtual Machine in Azure Portal.
2. Navigate to Networking.
3. Open the associated Network Security Group.
4. Select Inbound security rules.
5. Click Add.

Configure:

| Setting          | Value                              |
| ---------------- | ---------------------------------- |
| Source           | Any (or restricted trusted source) |
| Source Port      | \*                                 |
| Destination      | Any                                |
| Service          | HTTP                               |
| Destination Port | 80                                 |
| Protocol         | TCP                                |
| Action           | Allow                              |
| Priority         | 300                                |
| Name             | Allow-HTTP                         |

Save the rule.

### Step 9: Test Websites

Open your browser and test both Public IP addresses.

**Website 1**

```
http://<public-ip-1>
```

Expected Output:

```
Welcome to Azure Site 1
```

**Website 2**

```
http://<public-ip-2>
```

Expected Output:

```
Welcome to Azure Site 2
```

You can also test from the VM:

```
curl http://10.0.1.4
curl http://10.0.1.5
```

## AWS vs Azure Comparison

| AWS                  | Azure                      |
| -------------------- | -------------------------- |
| EC2 Instance         | Virtual Machine            |
| ENI                  | NIC                        |
| Secondary Private IP | Secondary IP Configuration |
| Elastic IP           | Static Public IP           |
| Security Group       | Network Security Group     |
| Amazon Linux         | Ubuntu                     |
| dnf install nginx    | apt install nginx          |

## Key Learning Outcomes

- Configure multiple private IP addresses on an Azure NIC.
- Associate multiple Public IP addresses with a single VM.
- Understand Public IP and Private IP mapping.
- Install and configure Nginx on Ubuntu.
- Host multiple websites using IP-based virtual hosting.
- Configure Azure Network Security Groups.

## Troubleshooting

**Nginx configuration error**

```
sudo nginx -t
```

**Check IP addresses**

```
ip a
```

**Check Nginx status**

```
sudo systemctl status nginx
```

**Check listening ports**

```
sudo ss -lntp | grep ':80'
```

**Check firewall**

```
sudo ufw status
```

## Cleanup

To avoid unnecessary Azure charges:

1. Delete the Virtual Machine after completing the lab.
2. Delete unused Public IP resources.
3. Delete unused Network Interfaces and disks.
4. Delete the dedicated Resource Group if it is no longer required.

## Official Documentation

- [Azure Virtual Machine Multiple IP Addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/virtual-network-multiple-ip-addresses-portal)
- [Azure Public IP Addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses)
- [Azure Network Security Groups](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [Nginx Documentation](https://nginx.org/en/docs/)

---
