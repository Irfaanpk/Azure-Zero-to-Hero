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
