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

# When to Use Multiple Websites on One VM

This approach can be useful for:

- Small websites
- Development environments
- Testing environments
- Internal applications
- Low-traffic websites
- Multiple simple applications

For large production workloads, separate infrastructure may provide better:

- Availability
- Performance isolation
- Security isolation
- Scalability

---

Multi-Server-in-Single-Azure-VM Using Nginx

Objective

Host multiple websites on a single Azure Linux Virtual Machine using multiple Private IP addresses, Public IP addresses, and Nginx.

Architecture:

* One Azure Virtual Machine.
* One Network Interface (NIC).
* Two Private IP addresses.
* Two Public IP addresses.
* One Nginx web server.
* Two different websites.

Prerequisites

* Active Azure Subscription.
* Azure Linux Virtual Machine (Ubuntu 24.04).
* SSH access to the VM.
* Virtual Network and Subnet.
* Network Security Group (NSG).

1. Create an Azure Virtual Machine

In Azure Portal:

* Go to Virtual Machines.
* Click Create → Azure Virtual Machine.
* Configure:

Setting	Value
Image	Ubuntu 24.04 LTS
Size	Standard B1s (if available)
Authentication	SSH Public Key
Public IP	Create new
VNet	Create new or existing
Subnet	Select subnet
Inbound port	SSH (22)

* Click Review + Create.
* Wait until deployment is completed.

2. Create a Secondary Private IP Address

In Azure Portal:

* Open your Virtual Machine.
* Go to Networking.
* Click the attached Network Interface (NIC).
* Select IP configurations.
* Click Add.
* Configure:

Setting	Value
Name	ipconfig2
Private IP assignment	Dynamic
Public IP	Leave unassigned for now

7. Click Save.

Azure automatically assigns a secondary private IP address.

Example:

IP Configuration	Private IP
Primary	10.0.1.4
Secondary	10.0.1.5

3. Create a Second Public IP Address

Azure uses a Public IP resource instead of AWS Elastic IP.

In Azure Portal:

* Search for Public IP addresses.
* Click Create.
* Configure:

Setting	Value
Name	public-ip-02
IP Version	IPv4
SKU	Standard
Tier	Regional
Assignment	Static
Region	Same as VM

4. Click Create.

Associate Public IP with Secondary Private IP

* Open the VM’s Network Interface.
* Go to IP configurations.
* Select ipconfig2.
* Under Public IP address, select public-ip-02.
* Click Save.

Example:

Configuration	Private IP	Public IP
IP Config 1	10.0.1.4	20.10.10.10
IP Config 2	10.0.1.5	20.20.20.20

Note: These are example IP addresses. Use your actual Azure-assigned addresses.

4. Verify Multiple IPs on Azure Linux VM

Connect to the VM using SSH:

ssh -i key.pem azureuser@<public-ip>

Check network interfaces:

ip a

Check routes:

ip route

You should identify the primary and secondary private IP addresses.

Important: Depending on the Ubuntu image and network configuration, the secondary IP may need to be configured inside the Linux guest OS before Nginx can bind to it.

For a temporary lab configuration:

sudo ip addr add 10.0.1.5/24 dev eth0

Replace:

* 10.0.1.5 with your secondary private IP.
* eth0 with your actual network interface.
* /24 with your actual subnet prefix.

Verify:

ip addr show eth0

This command is temporary and does not survive a reboot. For permanent configuration, use the appropriate Netplan configuration on Ubuntu.

5. Install Nginx

Update Ubuntu packages:

sudo apt update

Install Nginx:

sudo apt install nginx -y

Enable Nginx:

sudo systemctl enable nginx

Start Nginx:

sudo systemctl start nginx

Check status:

sudo systemctl status nginx

6. Create Website Directories

Create two website directories:

sudo mkdir -p /var/www/site1
sudo mkdir -p /var/www/site2

Create test pages:

Website 1

echo "Welcome to Azure Site 1" | sudo tee /var/www/site1/index.html

Website 2

echo "Welcome to Azure Site 2" | sudo tee /var/www/site2/index.html

Set permissions:

sudo chown -R www-data:www-data /var/www/site1
sudo chown -R www-data:www-data /var/www/site2

7. Create Nginx Configuration for Each IP

Create a configuration file:

sudo nano /etc/nginx/sites-available/multisite

Add the following configuration:

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

Replace the example private IP addresses with your actual private IP addresses.

Enable the configuration:

sudo ln -s /etc/nginx/sites-available/multisite /etc/nginx/sites-enabled/multisite

If the default site conflicts with your configuration, disable it:

sudo rm -f /etc/nginx/sites-enabled/default

8. Test and Restart Nginx

Test the configuration:

sudo nginx -t

Restart Nginx:

sudo systemctl restart nginx

Verify Nginx is listening:

sudo ss -lntp | grep ':80'

If you get Cannot assign requested address, verify that the secondary private IP is configured inside Linux.

9. Configure Network Security Group (NSG)

In Azure Portal:

* Open your Virtual Machine.
* Go to Networking.
* Open the associated Network Security Group.
* Select Inbound security rules.
* Click Add.

Configure:

Setting	Value
Source	Any
Source port ranges	*
Destination	Any
Service	HTTP
Destination port	80
Protocol	TCP
Action	Allow
Priority	300
Name	Allow-HTTP

Click Add.

Also ensure that the Linux firewall allows HTTP traffic if it is enabled.

10. Test Websites in Browser

Open your browser.

Website 1:

http://<public-ip-1>

Expected output:

Welcome to Azure Site 1

Website 2:

http://<public-ip-2>

Expected output:

Welcome to Azure Site 2

You can also test internally from the VM:

curl http://10.0.1.4
curl http://10.0.1.5

11. Architecture

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

12. AWS vs Azure Comparison

AWS	Azure
EC2 Instance	Virtual Machine
ENI	NIC
Secondary Private IP	Secondary IP Configuration
Elastic IP	Static Public IP
Security Group	Network Security Group
Amazon Linux	Ubuntu
dnf install nginx	apt install nginx
/etc/nginx/conf.d/	/etc/nginx/sites-available/

13. Important Learning Points

* One Azure VM can have multiple private IP addresses.
* Each NIC IP configuration can have an associated Public IP.
* Azure maps Public IP addresses to Private IP configurations.
* Nginx can host multiple websites using IP-based virtual hosting.
* A single Nginx installation can serve multiple websites.
* NSG controls inbound and outbound network traffic.

14. Cleanup

To avoid unnecessary Azure charges:

* Stop and delete the Virtual Machine when the lab is complete.
* Delete unused Public IP resources.
* Delete unused NICs and disks.
* Delete the dedicated resource group if all resources inside it are no longer needed.

References

* Microsoft Learn – Configure multiple IP addresses for Azure VMs⁠￼
* Microsoft Learn – Azure Public IP addresses⁠￼
* Microsoft Learn – Network Security Groups⁠￼
* Nginx Documentation⁠￼