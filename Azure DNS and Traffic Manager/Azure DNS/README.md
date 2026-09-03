# 13.1 Azure DNS

## What is Azure DNS?

**Azure DNS** is a hosting service for DNS domains that provides name resolution using the Azure infrastructure.

DNS converts human-readable domain names into IP addresses.

```text
www.example.com
       ↓
   Azure DNS
       ↓
    IP Address
```

---

# Why Use Azure DNS?

Azure DNS provides:

- DNS zone hosting
- DNS record management
- Public DNS resolution
- Private DNS resolution
- Integration with Azure Virtual Networks
- Azure-based DNS management

---

# DNS Zone

A **DNS zone** contains DNS records for a specific domain.

Example:

```text
example.com
    │
    ├── www
    ├── api
    ├── mail
    └── ftp
```

A DNS zone can be hosted in Azure DNS.

---

# Public DNS Zone

A **public DNS zone** is used to host DNS records that can be resolved from the public internet.

Example:

```text
example.com
     ↓
Azure Public DNS Zone
     ↓
Public DNS Resolution
```

Example records:

```text
www.example.com → Public IP
api.example.com → Public IP
```

---

# Private DNS Zone

An **Azure Private DNS zone** provides DNS resolution within Azure virtual networks.

Example:

```text
Azure VNet
    │
    ▼
Private DNS Zone
    │
    ├── app.internal
    └── db.internal
```

Private DNS zones are useful for resolving private names without exposing them to the public internet.

---

# Public DNS vs Private DNS

| Public DNS | Private DNS |
|---|---|
| Public internet resolution | Private network resolution |
| Publicly accessible DNS names | Internal DNS names |
| Uses public DNS infrastructure | Used with Azure VNets |
| Example: `www.example.com` | Example: `db.internal` |

---

# DNS Records

DNS records define how domain names should be resolved.

Common record types include:

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Maps a name to another domain name |
| MX | Defines mail servers |
| TXT | Stores text-based information |
| NS | Defines authoritative name servers |

---

# A Record

An **A record** maps a hostname to an IPv4 address.

Example:

```text
www.example.com
       ↓
    20.10.10.5
```

---

# AAAA Record

An **AAAA record** maps a hostname to an IPv6 address.

Example:

```text
www.example.com
       ↓
2001:db8::10
```

---

# CNAME Record

A **CNAME** maps one hostname to another hostname.

Example:

```text
www.example.com
       ↓
app.example.com
```

---

# MX Record

An **MX record** identifies the mail servers responsible for receiving email for a domain.

Example:

```text
example.com
     ↓
Mail Server
```

---

# TXT Record

A **TXT record** stores text information associated with a domain.

TXT records are commonly used for:

- Domain verification
- Email security
- Service verification

---

# NS Record

An **NS (Name Server)** record identifies the authoritative DNS name servers for a DNS zone.

```text
example.com
     ↓
Name Servers
     ↓
Azure DNS
```

---

# DNS Resolution

DNS resolution converts a domain name into an IP address.

Example:

```text
User
 │
 │ www.example.com
 ▼
DNS Resolver
 │
 ▼
Azure DNS
 │
 ▼
IP Address
 │
 ▼
Web Server
```

---

# Azure DNS and Virtual Networks

Azure Private DNS can be linked to one or more Azure Virtual Networks.

```text
Private DNS Zone
       │
       ├── VNet 1
       │
       └── VNet 2
```

This allows resources in linked VNets to resolve private DNS names.

---

# DNS Zone Delegation

For a public domain, the domain's registrar needs to delegate the domain to the authoritative name servers of the Azure DNS zone.

```text
Domain Registrar
       ↓
Azure DNS Name Servers
       ↓
DNS Zone
       ↓
DNS Records
```

---

# Azure DNS Use Cases

Azure DNS can be used for:

- Hosting public DNS zones
- Internal DNS resolution
- Azure resource name resolution
- Custom domain management
- Private application environments
- VNet-based name resolution

---

# Practical Lab

## Lab: Create Azure DNS Zone and DNS Records

### Objective

Create a public DNS zone, add DNS records, configure domain delegation, and test DNS resolution.

### Step 1: Create DNS Zone

1. Open **Azure Portal**.
2. Search for **DNS zones**.
3. Select **Create**.
4. Select your subscription.
5. Create or select a resource group.
6. Enter your domain name.

Example:

```text
example.com
```

7. Select **Review + create**.
8. Select **Create**.

---

### Step 2: View Name Servers

Open the DNS zone.

Locate the Azure DNS name servers.

Example:

```text
ns1-xx.azure-dns.com
ns2-xx.azure-dns.net
ns3-xx.azure-dns.org
ns4-xx.azure-dns.info
```

---

### Step 3: Delegate the Domain

At your domain registrar, replace the existing name servers with the Azure DNS name servers.

```text
Domain Registrar
       ↓
Azure DNS Name Servers
```

> Domain delegation is required when you want Azure DNS to become authoritative for your public domain.

---

### Step 4: Create an A Record

Inside the DNS zone:

1. Select **Record sets**.
2. Select **Add**.
3. Enter:

```text
Name:
www

Type:
A

IP Address:
<YOUR_PUBLIC_IP>
```

4. Save the record.

The result is:

```text
www.example.com
       ↓
<YOUR_PUBLIC_IP>
```

---

### Step 5: Create a CNAME Record

Create another record:

```text
Name:
app

Type:
CNAME

Target:
www.example.com
```

Now:

```text
app.example.com
       ↓
www.example.com
       ↓
<YOUR_PUBLIC_IP>
```

---

### Step 6: Test DNS Resolution

From your local machine:

```bash
nslookup www.example.com
```

Or:

```bash
nslookup app.example.com
```

Verify that the DNS name resolves to the configured destination.

---

# Key Points

- Azure DNS hosts DNS zones and records using Azure infrastructure.
- Public DNS zones provide internet-facing DNS resolution.
- Private DNS zones provide internal DNS resolution.
- A records map names to IPv4 addresses.
- AAAA records map names to IPv6 addresses.
- CNAME records map one hostname to another hostname.
- MX records identify mail servers.
- TXT records store text-based domain information.
- NS records identify authoritative name servers.
- Private DNS zones can be linked to Azure VNets.
- Public domains must be delegated to Azure DNS name servers to use Azure DNS as the authoritative DNS service.
