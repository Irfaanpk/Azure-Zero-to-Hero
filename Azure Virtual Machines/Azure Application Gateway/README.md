# 8.10 Azure Application Gateway

## What is Azure Application Gateway?

**Azure Application Gateway** is a managed **Layer 7 (application layer) load balancing** service for web applications.

It can route HTTP and HTTPS traffic based on application-level information such as:

- Hostname
- URL path
- HTTP headers
- Cookies

```text
                         Internet
                            │
                            ▼
                  Azure Application Gateway
                            │
              ┌─────────────┴─────────────┐
              │                           │
        Web Application 1          Web Application 2
              │                           │
           Backend VM                Backend VM
```

---

# Why Use Application Gateway?

Application Gateway is useful when a web application needs more than simple TCP/UDP load balancing.

It provides features such as:

- Layer 7 load balancing
- URL-based routing
- Host-based routing
- TLS/SSL termination
- Health probes
- Backend pools
- Web Application Firewall (WAF)

---

# Layer 7 Load Balancing

Application Gateway operates at **Layer 7 — Application Layer**.

It understands HTTP and HTTPS traffic.

```text
Client
   │
   │ HTTP / HTTPS
   ▼
Application Gateway
   │
   ├── URL / Hostname
   │
   ├── Routing Rule
   │
   └── Backend Pool
```

Unlike Azure Load Balancer, Application Gateway can make routing decisions based on web request information.

---

# Application Gateway Components

The main components are:

```text
Application Gateway
        │
        ├── Frontend IP Configuration
        ├── Listeners
        ├── Routing Rules
        ├── Backend Pools
        ├── Backend Settings
        ├── Health Probes
        └── WAF
```

---

# Frontend IP Configuration

The frontend IP is the address through which clients connect to the Application Gateway.

It can use:

- Public IP address
- Private IP address

Example:

```text
Internet
   │
   ▼
Public IP
   │
   ▼
Application Gateway
```

---

# Listener

A **listener** receives incoming HTTP or HTTPS requests.

A listener can be configured using:

- Protocol
- Port
- Frontend IP
- Hostname
- TLS certificate for HTTPS

Example:

```text
Client
   │
   │ HTTPS : 443
   ▼
Listener
   │
   ▼
Application Gateway
```

---

# HTTP Listener

An HTTP listener commonly uses:

```text
Protocol: HTTP
Port: 80
```

Example:

```text
http://example.com
        │
        ▼
Application Gateway
```

---

# HTTPS Listener

An HTTPS listener commonly uses:

```text
Protocol: HTTPS
Port: 443
```

An SSL/TLS certificate can be associated with the listener.

```text
Client
   │
   │ HTTPS
   ▼
HTTPS Listener
   │
   ▼
Application Gateway
```

---

# TLS/SSL Termination

Application Gateway can terminate TLS connections at the gateway.

Example:

```text
Client
   │
   │ HTTPS
   ▼
Application Gateway
   │
   │ HTTP / HTTPS
   ▼
Backend VM
```

The gateway handles the client-side TLS connection before forwarding the request to the backend.

This is commonly called **TLS/SSL termination**.

---

# Backend Pool

A **backend pool** contains the resources that receive traffic from Application Gateway.

Backend targets can include:

- Virtual Machines
- Virtual Machine Scale Sets
- IP addresses
- Fully qualified domain names

Example:

```text
Application Gateway
        │
        ▼
Backend Pool
   ┌────┼────┐
   │    │    │
 VM-1 VM-2 VM-3
```

---

# Backend Settings

Backend settings define how Application Gateway communicates with backend resources.

They can specify settings such as:

- Backend protocol
- Backend port
- Cookie-based affinity
- Connection settings
- Hostname configuration

Example:

```text
Application Gateway
       │
       │ HTTP : 80
       ▼
Backend VM
```

---

# Health Probes

Application Gateway uses **health probes** to determine whether backend resources are available.

Example:

```text
Application Gateway
        │
        │ Health Probe
        ▼
   ┌────┴────┐
   │         │
  VM-1      VM-2
 Healthy   Unhealthy
```

Traffic is sent only to healthy backend instances.

A probe can check:

- Protocol
- Port
- Path
- Response

Example:

```text
Protocol: HTTP
Port: 80
Path: /
```

---

# Routing Rules

A routing rule determines where incoming requests should be sent.

```text
Listener
   ↓
Routing Rule
   ↓
Backend Pool
```

Application Gateway supports different routing approaches.

---

# Basic Routing

A basic routing rule forwards requests from a listener to a backend pool.

```text
Client
   ↓
Listener
   ↓
Routing Rule
   ↓
Backend Pool
   ↓
VMs
```

---

# Path-Based Routing

**Path-based routing** routes requests based on the URL path.

Example:

```text
example.com/images/*
        ↓
Image Backend
```

```text
example.com/api/*
        ↓
API Backend
```

```text
example.com/*
        ↓
Web Backend
```

Architecture:

```text
                    Application Gateway
                            │
             ┌──────────────┼──────────────┐
             │              │              │
        /images/*        /api/*          /*
             │              │              │
             ▼              ▼              ▼
        Image Pool       API Pool       Web Pool
```

---

# Host-Based Routing

Application Gateway can route traffic based on the hostname.

Example:

```text
www.example.com
        ↓
Website Backend
```

```text
api.example.com
        ↓
API Backend
```

Both applications can use the same Application Gateway.

```text
                 Application Gateway
                         │
              ┌──────────┴──────────┐
              │                     │
      www.example.com        api.example.com
              │                     │
              ▼                     ▼
        Web Backend             API Backend
```

---

# Path-Based vs Host-Based Routing

| Path-Based Routing | Host-Based Routing |
|---|---|
| Uses URL path | Uses hostname |
| `/api/*` | `api.example.com` |
| `/images/*` | `images.example.com` |
| Routes based on URL | Routes based on domain |

---

# Web Application Firewall

Application Gateway can be integrated with **Web Application Firewall (WAF)**.

WAF helps protect web applications against common web-based attacks.

```text
Internet
    │
    ▼
Application Gateway
    │
    ▼
WAF
    │
    ▼
Backend Application
```

WAF capabilities can include protection against common attacks such as:

- SQL injection
- Cross-site scripting (XSS)
- Other common web application attacks

> WAF is a security feature of Application Gateway and should not be confused with Layer 4 network filtering provided by an NSG.

---

# Application Gateway vs Azure Load Balancer

This is an important AZ-104 comparison.

| Azure Load Balancer | Application Gateway |
|---|---|
| Layer 4 | Layer 7 |
| TCP / UDP | HTTP / HTTPS |
| Network-level load balancing | Application-level load balancing |
| No URL-based routing | URL-based routing |
| No host-based routing | Host-based routing |
| Supports general TCP/UDP workloads | Designed for web applications |
| Health probes | Health probes |
| Supports inbound and outbound scenarios | Primarily application traffic routing |
| No WAF | WAF available |

Example:

```text
Azure Load Balancer
        ↓
TCP / UDP
        ↓
Backend VMs
```

```text
Application Gateway
        ↓
HTTP / HTTPS
        ↓
URL / Host Routing
        ↓
Backend Applications
```

---

# Application Gateway Architecture

A typical web application architecture can look like:

```text
                         Internet
                            │
                            ▼
                  Application Gateway
                            │
                  ┌─────────┴─────────┐
                  │                   │
             Web Backend         API Backend
                  │                   │
                VM-1                VM-2
```

With path-based routing:

```text
                    Application Gateway
                            │
             ┌──────────────┴──────────────┐
             │                             │
        /website/*                       /api/*
             │                             │
             ▼                             ▼
       Web Backend                    API Backend
```

---

# Application Gateway and VMSS

Application Gateway can send traffic to a **Virtual Machine Scale Set**.

```text
                    Application Gateway
                            │
                            ▼
                       Backend Pool
                            │
                            ▼
                           VMSS
                   ┌────────┼────────┐
                   │        │        │
                 VM-1     VM-2     VM-3
```

This provides:

- Layer 7 routing
- Health checking
- Multiple VM instances
- Application scalability

VM Scale Sets are covered in **Section 8.11**.

---

# Practical Lab

## Lab: Create an Application Gateway with Two Backend VMs

### Objective

Create an Application Gateway and route HTTP traffic to backend VMs.

### Architecture

```text
                         Internet
                            │
                            ▼
                  Application Gateway
                            │
                   ┌────────┴────────┐
                   │                 │
                 VM-1              VM-2
                   │                 │
                Website           Website
```

---

## Step 1: Prepare the Network

Create or use a VNet with separate subnets.

Example:

```text
VNet: 10.0.0.0/16

ApplicationGatewaySubnet: 10.0.1.0/24

BackendSubnet: 10.0.2.0/24
```

> Application Gateway requires a dedicated subnet.

---

## Step 2: Prepare Two Backend VMs

Create two Linux VMs in the backend subnet:

```text
VM-1
VM-2
```

Install Nginx on both VMs.

VM-1:

```bash
sudo apt update
sudo apt install nginx -y
```

VM-2:

```bash
sudo apt update
sudo apt install nginx -y
```

Configure different web pages so that you can identify which VM responds.

VM-1:

```html
<h1>Response from VM-1</h1>
```

VM-2:

```html
<h1>Response from VM-2</h1>
```

---

## Step 3: Create Application Gateway

1. Open **Azure Portal**.
2. Search for **Application Gateways**.
3. Select **Create**.
4. Select the required subscription.
5. Select the resource group.
6. Enter an Application Gateway name.
7. Select the required region.
8. Select the appropriate tier.

---

## Step 4: Configure Frontend

Configure a frontend IP.

For an internet-facing application:

```text
Frontend Type: Public
```

Create or select a public IP address.

---

## Step 5: Configure Backend Pool

Create a backend pool and add:

```text
VM-1
VM-2
```

---

## Step 6: Configure Listener

Create an HTTP listener:

```text
Protocol: HTTP
Port: 80
```

---

## Step 7: Configure Backend Settings

Configure:

```text
Protocol: HTTP
Port: 80
```

---

## Step 8: Configure Health Probe

Configure an HTTP health probe:

```text
Protocol: HTTP
Port: 80
Path: /
```

The Application Gateway uses the probe to determine backend health.

---

## Step 9: Configure Routing Rule

Create a basic routing rule:

```text
Listener
   ↓
Backend Settings
   ↓
Backend Pool
   ↓
VM-1 + VM-2
```

---

## Step 10: Deploy

Review the configuration and create the Application Gateway.

Wait for deployment to complete.

---

## Step 11: Test

Copy the Application Gateway public IP address.

Open:

```text
http://APPLICATION_GATEWAY_PUBLIC_IP
```

The request should be forwarded to a healthy backend VM.

---

## Step 12: Verify Health

Open the Application Gateway and check the backend health.

Example:

```text
VM-1 → Healthy
VM-2 → Healthy
```

If a backend becomes unavailable:

```text
VM-1 → Unhealthy
VM-2 → Healthy
```

Application Gateway should stop sending new traffic to the unhealthy backend.

---

# Key Points

- Azure Application Gateway is a **Layer 7 load balancer**.
- It is designed primarily for HTTP and HTTPS workloads.
- It supports URL path-based routing.
- It supports hostname-based routing.
- It uses listeners to receive client requests.
- Backend pools contain the resources that receive traffic.
- Backend settings define how traffic is sent to backend resources.
- Health probes determine backend health.
- Application Gateway supports TLS/SSL termination.
- Application Gateway can integrate with **Web Application Firewall (WAF)**.
- Application Gateway can route traffic to VMs and VM Scale Sets.
- **Azure Load Balancer** operates at Layer 4, while **Application Gateway** operates at Layer 7.
