# 14.1 Azure Front Door

## What is Azure Front Door?

**Azure Front Door** is a global application delivery service that provides fast, secure, and highly available access to web applications.

It operates at the **Layer 7 (HTTP/HTTPS)** level and can route client requests to application origins based on configured routing rules.

```text
User
  │
  ▼
Azure Front Door
  │
  ├── Origin 1
  │
  └── Origin 2
```

---

# Why Use Azure Front Door?

Azure Front Door can provide:

- Global application delivery
- Layer 7 HTTP/HTTPS routing
- Global load balancing
- Application failover
- Health-based routing
- URL-based routing
- Domain-based routing
- Caching
- TLS/HTTPS termination
- Web Application Firewall (WAF) integration

---

# Azure Front Door Architecture

The basic architecture is:

```text
                    Internet
                       │
                       ▼
                Azure Front Door
                       │
                ┌──────┴──────┐
                ▼             ▼
          Origin Group 1   Origin Group 2
                │             │
                ▼             ▼
           Web App / VM   Web App / VM
```

---

# Azure Front Door Components

The important Front Door components are:

- Front Door profile
- Endpoint
- Custom domain
- Origin
- Origin group
- Route
- Health probe
- Rule set
- Caching

---

# Front Door Profile

A **Front Door profile** is the main resource that contains the Front Door configuration.

It contains components such as:

```text
Front Door Profile
       │
       ├── Endpoint
       ├── Origin Group
       ├── Origin
       ├── Route
       └── Rule Set
```

---

# Endpoint

An **endpoint** provides the Front Door entry point through which users access the application.

Example:

```text
User
  │
  ▼
myapp-xxxxx.z01.azurefd.net
  │
  ▼
Azure Front Door
```

Users can also access the application through a custom domain.

---

# Origin

An **origin** is the backend application that receives requests from Azure Front Door.

Examples:

- Azure App Service
- Azure Storage
- Azure VM
- Publicly accessible web application

Example:

```text
Azure Front Door
       │
       ▼
Origin
       │
       ▼
Azure App Service
```

---

# Origin Group

An **origin group** contains one or more origins.

Example:

```text
Origin Group
      │
      ├── Origin 1 → East US App Service
      │
      └── Origin 2 → West Europe App Service
```

Front Door can use origin health to determine where requests should be routed.

---

# Health Probes

Azure Front Door uses health probes to determine whether an origin is healthy.

Example:

```text
Front Door
    │
    │ Health Probe
    ▼
Origin
    │
    ├── Healthy ✅
    └── Unhealthy ❌
```

If an origin becomes unhealthy, Front Door can route traffic to another healthy origin.

---

# Routing

A **route** defines how Front Door handles incoming requests.

A route can define:

- Domain
- URL path
- Origin group
- Accepted protocols
- Redirect settings
- Forwarding behavior

Example:

```text
www.example.com/api/*
          │
          ▼
      API Origin
```

```text
www.example.com/*
          │
          ▼
      Web Origin
```

---

# Path-Based Routing

Front Door can route requests based on URL paths.

Example:

```text
www.example.com/
        ↓
Web Application

www.example.com/api/*
        ↓
API Application

www.example.com/images/*
        ↓
Storage Origin
```

This allows different parts of an application to use different origins.

---

# HTTPS and TLS

Azure Front Door supports HTTPS for secure application access.

The basic flow is:

```text
Client
  │
  │ HTTPS
  ▼
Azure Front Door
  │
  │ HTTPS
  ▼
Origin
```

Front Door can handle TLS termination at the edge before forwarding the request to the origin.

---

# Caching

Azure Front Door can cache content at Microsoft edge locations.

Example:

```text
User
  │
  ▼
Front Door Edge
  │
  ├── Cached Content → Return immediately
  │
  └── Cache Miss
          ↓
       Origin
```

Caching can reduce:

- Origin requests
- Latency
- Bandwidth usage
- Application load

---

# Rule Sets

Front Door rule sets allow request and response behavior to be modified using rules.

Examples include:

- URL redirects
- URL rewrites
- Header modification
- Cache behavior
- Request modification

Example:

```text
Request
   ↓
Rule Set
   ↓
Modify Request
   ↓
Origin
```

---

# Web Application Firewall

Azure Front Door can integrate with **Azure Web Application Firewall (WAF)**.

WAF helps protect web applications against common web attacks.

Example:

```text
Internet
   │
   ▼
Azure Front Door
   │
   ▼
WAF
   │
   ▼
Application
```

Common protections include:

- SQL injection
- Cross-site scripting
- Malicious HTTP requests
- Common web vulnerabilities

---

# Front Door Global Routing

Front Door can route requests to applications deployed in different Azure regions.

Example:

```text
                     Azure Front Door
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
             East US               West Europe
             Origin                  Origin
                 │                     │
                 ▼                     ▼
            Application           Application
```

This helps build globally distributed applications.

---

# Azure Front Door vs Regional Load Balancer

| Azure Front Door | Azure Load Balancer |
|---|---|
| Layer 7 | Layer 4 |
| HTTP/HTTPS | TCP/UDP |
| Global | Regional |
| URL-based routing | Connection-based load balancing |
| Host-based routing | No URL-based routing |
| Global failover | Regional backend distribution |
| Edge-based service | Regional load balancer |

---

# Azure Front Door vs Application Gateway

| Azure Front Door | Application Gateway |
|---|---|
| Global service | Regional service |
| Layer 7 | Layer 7 |
| Global routing | Regional routing |
| Edge-based delivery | Regional application delivery |
| Supports global failover | Supports regional backend routing |
| CDN-like caching capabilities | No general CDN role |
| WAF integration | WAF integration |

---

# Azure Front Door Use Cases

Azure Front Door is useful for:

- Global web applications
- Multi-region applications
- Global load balancing
- Application failover
- URL-based routing
- High-performance web delivery
- HTTPS termination
- Application security with WAF
- Content caching

---

# Practical Lab

## Lab: Deploy and Configure Azure Front Door

### Objective

Create two web applications in different regions, configure Azure Front Door with multiple origins, configure routing and health monitoring, and verify global application delivery.

### Architecture

```text
                         Internet
                            │
                            ▼
                    Azure Front Door
                            │
                     Origin Group
                       /         \
                      /           \
                     ▼             ▼
              East US App     West Europe App
                    │               │
                    ▼               ▼
              Web Application  Web Application
```

---

## Step 1: Create Two Web Apps

Create two Azure App Service Web Apps in different regions.

Example:

```text
Web App 1
Region: East US

Web App 2
Region: West Europe
```

Make the applications return different content so you can identify which origin is serving the request.

Example:

```text
Application 1:
"Hello from East US"

Application 2:
"Hello from West Europe"
```

---

## Step 2: Create Front Door Profile

1. Open **Azure Portal**.
2. Search for **Front Door and CDN profiles**.
3. Select **Create**.
4. Select **Azure Front Door**.
5. Choose the appropriate Front Door tier.
6. Enter a profile name.

Example:

```text
afd-demo-profile
```

---

## Step 3: Create Endpoint

Configure an endpoint for the Front Door profile.

Example:

```text
Endpoint:
afd-demo-endpoint
```

Azure provides a Front Door endpoint hostname similar to:

```text
afd-demo-endpoint-xxxxx.z01.azurefd.net
```

---

## Step 4: Create Origin Group

Create an origin group.

Example:

```text
Origin Group:
web-origin-group
```

Configure health monitoring.

Example:

```text
Protocol:
HTTPS

Path:
/
```

---

## Step 5: Add Origin 1

Add the East US App Service as the first origin.

```text
Origin 1
   ↓
East US App Service
```

---

## Step 6: Add Origin 2

Add the West Europe App Service as the second origin.

```text
Origin 2
   ↓
West Europe App Service
```

The architecture becomes:

```text
Origin Group
      │
      ├── East US
      │
      └── West Europe
```

---

## Step 7: Configure Route

Create a route connecting the Front Door endpoint to the origin group.

Example:

```text
Frontend:
Front Door Endpoint

Patterns:
/

Origin Group:
web-origin-group

Accepted Protocols:
HTTP + HTTPS
```

Enable HTTPS redirection if required.

Save the route.

---

## Step 8: Test the Front Door Endpoint

Open the Front Door endpoint hostname:

```text
https://afd-demo-endpoint-xxxxx.z01.azurefd.net
```

Verify that the application is accessible through Azure Front Door.

---

## Step 9: Test Origin Failover

Stop or make the primary application unavailable.

Example:

```text
East US
   ❌ Unhealthy

West Europe
   ✅ Healthy
```

Wait for the health probe to detect the failure.

Then access the Front Door endpoint again.

Traffic should be routed to the healthy origin.

```text
User
 │
 ▼
Azure Front Door
 │
 ├── East US ❌
 │
 └── West Europe ✅
          │
          ▼
      Application
```

---

## Step 10: Verify HTTPS

Open the Front Door endpoint using HTTPS:

```text
https://<front-door-endpoint>
```

Verify that the application is accessible securely over HTTPS.

---

# Key Points

- Azure Front Door is a **global Layer 7 application delivery service**.
- It supports HTTP and HTTPS traffic.
- A Front Door profile contains endpoints, origins, routes, and other configuration.
- Origins are backend applications.
- Origin groups contain one or more origins.
- Health probes monitor origin availability.
- Routes determine how requests are forwarded.
- Front Door supports path-based routing.
- Front Door supports global application failover.
- Front Door can cache content at edge locations.
- Rule sets can modify request and response behavior.
- Front Door integrates with Azure WAF.
- Front Door is global, while Azure Load Balancer and Application Gateway are primarily regional services.
