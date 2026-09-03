# 8.9 Azure Load Balancer

## What is Azure Load Balancer?

**Azure Load Balancer** is a Layer 4 load balancing service that distributes incoming network traffic across multiple backend resources.

It uses:

- TCP
- UDP

Azure Load Balancer helps improve:

- Availability
- Scalability
- Reliability

```text
                    Client
                       │
                       ▼
                Azure Load Balancer
                       │
              ┌────────┴────────┐
              │                 │
             VM-1              VM-2
              │                 │
              └────────┬────────┘
                       │
                 Application
```

---

# Why Use Azure Load Balancer?

Without a load balancer:

```text
Client
  │
  ▼
VM-1
```

If VM-1 becomes unavailable:

```text
Client
  │
  X
 VM-1
```

With a Load Balancer:

```text
                 Load Balancer
                /             \
              VM-1            VM-2
```

If VM-1 becomes unavailable, traffic can be sent to VM-2.

---

# Layer 4 Load Balancing

Azure Load Balancer operates at **Layer 4 — Transport Layer**.

It works with:

- TCP
- UDP

It does not inspect HTTP URLs, hostnames, or application-level content.

```text
Client
   │
   │ TCP / UDP
   ▼
Load Balancer
   │
   ├── VM-1
   ├── VM-2
   └── VM-3
```

For HTTP/HTTPS application-level routing, Azure provides **Application Gateway**, which is covered in **Section 8.10**.

---

# Azure Load Balancer SKUs

Azure Load Balancer primarily provides:

- **Standard Load Balancer**
- **Basic Load Balancer** — retired

For new deployments, use **Standard Load Balancer**.

```text
Azure Load Balancer
        │
        ▼
Standard Load Balancer
```

---

# Load Balancer Types

Azure Load Balancer can be used as:

### Public Load Balancer

Provides load balancing for internet-facing applications.

```text
Internet
    │
    ▼
Public Load Balancer
    │
    ├── VM-1
    └── VM-2
```

### Internal Load Balancer

Provides load balancing using a private IP address.

It is used for internal applications and services.

```text
Application Tier
       │
       ▼
Internal Load Balancer
       │
       ├── VM-1
       └── VM-2
```

---

# Azure Load Balancer Components

The main components are:

```text
Azure Load Balancer
       │
       ├── Frontend IP Configuration
       ├── Backend Pool
       ├── Health Probe
       ├── Load Balancing Rule
       └── Inbound NAT Rule
```

---

# Frontend IP Configuration

The **frontend IP** is the IP address through which clients access the Load Balancer.

For a public Load Balancer:

```text
Internet
   │
   ▼
Public IP
   │
   ▼
Load Balancer
```

For an internal Load Balancer:

```text
Client
   │
   ▼
Private IP
   │
   ▼
Internal Load Balancer
```

---

# Backend Pool

The **backend pool** contains the resources that receive traffic from the Load Balancer.

Example:

```text
Load Balancer
      │
      ▼
Backend Pool
 ┌────┼────┐
 │    │    │
VM-1 VM-2 VM-3
```

Backend resources can include supported Azure resources such as:

- Virtual Machines
- Virtual Machine Scale Sets

---

# Health Probe

A **health probe** checks whether backend resources are healthy and available to receive traffic.

Example:

```text
Load Balancer
      │
      │ Health Probe
      ▼
   VM-1 → Healthy
   VM-2 → Healthy
   VM-3 → Unhealthy
```

The Load Balancer sends traffic only to healthy backend instances.

---

# Health Probe Protocols

Azure Load Balancer health probes can use protocols such as:

- TCP
- HTTP
- HTTPS

Example HTTP probe:

```text
Protocol: HTTP
Port: 80
Path: /
```

The Load Balancer checks the specified endpoint and determines whether the backend is healthy.

---

# Load Balancing Rule

A **load balancing rule** defines how traffic received on the frontend is distributed to backend resources.

A rule commonly defines:

- Frontend IP
- Frontend port
- Backend port
- Protocol
- Backend pool
- Health probe

Example:

```text
Frontend:
Public IP : 80

        ↓

Backend:
VM-1 : 80
VM-2 : 80
VM-3 : 80
```

---

# Traffic Flow

Example HTTP traffic:

```text
Client
  │
  │ HTTP : 80
  ▼
Frontend IP
  │
  ▼
Load Balancing Rule
  │
  ▼
Health Probe
  │
  ▼
Backend Pool
  │
  ├── VM-1
  ├── VM-2
  └── VM-3
```

The Load Balancer distributes connections among healthy backend instances.

---

# Inbound NAT Rules

An **inbound NAT rule** allows traffic received on a specific frontend port to be forwarded to a specific backend VM and port.

Example:

```text
Public IP : 50001
       │
       ▼
Load Balancer
       │
       ▼
VM-1 : 3389
```

This can be useful for administrative access to individual VMs.

For secure VM administration, **Azure Bastion** is generally preferred instead of exposing RDP or SSH ports through the internet.

---

# Outbound Rules

Azure Load Balancer can also provide outbound connectivity for backend resources.

Outbound rules define how backend instances access external destinations through the Load Balancer's frontend public IP.

```text
VM
 │
 ▼
Load Balancer
 │
 ▼
Public IP
 │
 ▼
Internet
```

For dedicated and predictable outbound internet connectivity, **NAT Gateway** can be used.

NAT Gateway is covered in **Section 7.11**.

---

# Load Balancing Distribution

Azure Load Balancer distributes traffic across healthy backend instances.

Example:

```text
              Load Balancer
                    │
          ┌─────────┼─────────┐
          │         │         │
        VM-1      VM-2      VM-3
       Healthy   Healthy   Healthy
```

If VM-2 becomes unhealthy:

```text
              Load Balancer
                    │
          ┌─────────┴─────────┐
          │                   │
        VM-1                VM-3
       Healthy              Healthy

        VM-2
      Unhealthy
```

VM-2 is removed from the set of healthy backend instances until the health probe indicates that it is healthy again.

---

# Session Persistence

Azure Load Balancer can support session persistence options that influence how client connections are distributed.

Common options include:

- None
- Client IP
- Client IP and protocol

Session persistence can be useful when an application needs related connections from the same client to reach the same backend instance.

---

# Azure Load Balancer vs Application Gateway

This is an important AZ-104 comparison.

| Azure Load Balancer | Application Gateway |
|---|---|
| Layer 4 | Layer 7 |
| TCP / UDP | HTTP / HTTPS |
| Network-level load balancing | Application-level load balancing |
| No URL-based routing | URL-based routing |
| No host-based routing | Host-based routing |
| Health probes | Health probes |
| Suitable for TCP/UDP workloads | Suitable for web applications |
| Supports public and internal load balancing | Supports web traffic routing |

Example:

```text
Load Balancer
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
URL / Host-based Routing
    ↓
Backend Applications
```

---

# Azure Load Balancer vs NAT Gateway

These services are different.

| Load Balancer | NAT Gateway |
|---|---|
| Distributes traffic | Provides outbound internet connectivity |
| Supports inbound load balancing | Primarily outbound |
| Uses frontend IP | Uses public IP or public IP prefix |
| Backend pools | Subnet association |
| Health probes | No backend health probes |
| Load balancing rules | SNAT for outbound connections |

---

# Load Balancer Architecture

A common architecture for a highly available web application is:

```text
                         Internet
                            │
                            ▼
                  Public Load Balancer
                            │
                  ┌─────────┴─────────┐
                  │                   │
                VM-1                VM-2
                  │                   │
             Web Server          Web Server
```

The Load Balancer distributes incoming traffic between the VMs.

---

# Practical Lab

## Lab: Create a Public Azure Load Balancer

### Objective

Create a Public Load Balancer and distribute HTTP traffic between two Azure VMs.

### Architecture

```text
                    Internet
                       │
                       ▼
               Public IP Address
                       │
                       ▼
                Azure Load Balancer
                       │
              ┌────────┴────────┐
              │                 │
            VM-1              VM-2
              │                 │
            Nginx             Nginx
```

---

## Step 1: Prepare Two VMs

Create two Linux VMs:

```text
VM-1
VM-2
```

Place them in the same VNet.

Install a web server such as Nginx on both VMs.

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

---

## Step 2: Create the Load Balancer

1. Open **Azure Portal**.
2. Search for **Load Balancers**.
3. Select **Create**.
4. Select **Standard** SKU.
5. Select **Public** load balancer.
6. Select the subscription.
7. Select the resource group.
8. Enter a Load Balancer name.
9. Select the required region.

---

## Step 3: Create Frontend IP

Configure a frontend public IP address.

Example:

```text
Frontend IP:
LoadBalancer-Public-IP
```

---

## Step 4: Create Backend Pool

Create a backend pool and add:

```text
VM-1
VM-2
```

The Load Balancer will use these VMs as backend resources.

---

## Step 5: Create Health Probe

Create an HTTP health probe:

```text
Protocol: HTTP
Port: 80
Path: /
```

The probe checks whether the web servers are available.

---

## Step 6: Create Load Balancing Rule

Create a rule such as:

```text
Protocol: TCP
Frontend Port: 80
Backend Port: 80
Backend Pool: VM-1 + VM-2
Health Probe: HTTP
```

---

## Step 7: Configure NSG

Make sure the required HTTP traffic is allowed through the applicable Network Security Group.

```text
Internet
   │
   │ TCP 80
   ▼
Load Balancer
   │
   ▼
NSG
   │
   ▼
Backend VMs
```

---

## Step 8: Test the Load Balancer

Copy the Load Balancer's public IP address.

Open:

```text
http://LOAD_BALANCER_PUBLIC_IP
```

The request should be forwarded to one of the backend VMs.

---

## Step 9: Test High Availability

Stop or make one backend VM unavailable.

For example:

```text
VM-1 → Unavailable
VM-2 → Healthy
```

The health probe detects that VM-1 is unhealthy.

```text
Load Balancer
      │
      └── VM-2
          Healthy
```

Requests should continue reaching the healthy backend VM.

---

# Key Points

- Azure Load Balancer is a **Layer 4** load balancing service.
- It supports **TCP and UDP** traffic.
- A Public Load Balancer provides internet-facing load balancing.
- An Internal Load Balancer provides private load balancing.
- The **frontend IP** receives client traffic.
- The **backend pool** contains backend resources.
- **Health probes** determine backend health.
- **Load balancing rules** define how traffic is distributed.
- **Inbound NAT rules** can forward traffic to a specific backend VM.
- **Outbound rules** can provide outbound connectivity through the Load Balancer.
- Standard Load Balancer should be used for new deployments.
- Azure Load Balancer is different from **Application Gateway**, which operates at Layer 7.
- NAT Gateway is designed specifically for predictable outbound internet connectivity.
