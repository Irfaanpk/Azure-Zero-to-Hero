# 8.7 Multiple Websites on a Single VM

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

# Practical Lab

## Lab: Host Two Websites on One Linux VM Using Nginx

### Objective

Host two different websites on a single Azure Linux VM using hostname-based routing.

### Architecture

```text
                Azure VM
                   │
                 Nginx
                /     \
               /       \
        site1.com    site2.com
           │            │
     /var/www/site1  /var/www/site2
```

---

### Step 1: Create a Linux VM

Create an Ubuntu VM in Azure.

Allow:

```text
SSH  → 22
HTTP → 80
```

---

### Step 2: Connect to the VM

```bash
ssh username@PUBLIC_IP
```

---

### Step 3: Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

Verify:

```bash
sudo systemctl status nginx
```

---

### Step 4: Create Website Directories

```bash
sudo mkdir -p /var/www/site1
sudo mkdir -p /var/www/site2
```

---

### Step 5: Create Website 1

```bash
sudo nano /var/www/site1/index.html
```

Add:

```html
<h1>Welcome to Website 1</h1>
```

---

### Step 6: Create Website 2

```bash
sudo nano /var/www/site2/index.html
```

Add:

```html
<h1>Welcome to Website 2</h1>
```

---

### Step 7: Create Nginx Configuration

Create the first configuration:

```bash
sudo nano /etc/nginx/sites-available/site1
```

Add:

```nginx
server {
    listen 80;
    server_name site1.example.com;

    root /var/www/site1;
    index index.html;
}
```

Create the second configuration:

```bash
sudo nano /etc/nginx/sites-available/site2
```

Add:

```nginx
server {
    listen 80;
    server_name site2.example.com;

    root /var/www/site2;
    index index.html;
}
```

---

### Step 8: Enable the Websites

```bash
sudo ln -s /etc/nginx/sites-available/site1 /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/site2 /etc/nginx/sites-enabled/
```

---

### Step 9: Test Nginx Configuration

```bash
sudo nginx -t
```

---

### Step 10: Reload Nginx

```bash
sudo systemctl reload nginx
```

---

### Step 11: Configure DNS

Create DNS records pointing both domains to the VM's public IP.

```text
site1.example.com → VM Public IP
site2.example.com → VM Public IP
```

---

### Step 12: Test

Open:

```text
http://site1.example.com
```

You should see:

```text
Welcome to Website 1
```

Open:

```text
http://site2.example.com
```

You should see:

```text
Welcome to Website 2
```

Both websites are running on the same Azure VM.

---

# Key Points

- Multiple websites can run on a single Azure VM.
- Hostname-based hosting is a common approach.
- Apache uses **Virtual Hosts**.
- Nginx uses **Server Blocks**.
- IIS uses **Bindings**.
- Multiple domains can point to the same VM public IP.
- DNS resolves the domain to the VM; the web server selects the website.
- Different websites can use different directories and configurations.
- Multiple websites on one VM can reduce infrastructure costs.
- Separate VMs provide better resource and failure isolation.
