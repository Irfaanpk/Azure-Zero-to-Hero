# 14.3 Azure Front Door vs Azure CDN

## Azure Front Door vs Azure CDN

Azure Front Door and Azure CDN both use Microsoft's global edge network, but they are designed for different primary purposes.

- **Azure Front Door** → Global application delivery, Layer 7 routing, load balancing, and application failover
- **Azure CDN** → Caching and fast delivery of static and cacheable content

---

# Basic Difference

```text
Azure Front Door
       ↓
Global Application Delivery
       ↓
Routing + Load Balancing + Failover + Caching
```

```text
Azure CDN
       ↓
Content Delivery
       ↓
Caching + Edge Delivery
```

---

# Azure Front Door

Azure Front Door is a **global Layer 7 application delivery service**.

It can route HTTP/HTTPS requests to different application origins.

```text
                    Internet
                       │
                       ▼
                Azure Front Door
                       │
              ┌────────┴────────┐
              ▼                 ▼
          Origin 1           Origin 2
          East US           West Europe
```

### Main Features

- Global application delivery
- Layer 7 HTTP/HTTPS routing
- Global load balancing
- Origin groups
- Health probes
- Path-based routing
- Host-based routing
- Application failover
- Caching
- HTTPS/TLS termination
- WAF integration

---

# Azure CDN

Azure CDN is primarily designed to **cache and deliver content from edge locations closer to users**.

```text
User
 │
 ▼
CDN Edge
 │
 ├── Cache Hit → Content
 │
 └── Cache Miss
         ↓
       Origin
```

### Main Features

- Content caching
- Global content delivery
- Reduced latency
- Reduced origin load
- Static content delivery
- Cache management
- Cache purge
- Custom domains
- HTTPS

---

# Comparison Table

| Feature | Azure Front Door | Azure CDN |
|---|---|---|
| Primary Purpose | Global application delivery | Content delivery |
| Layer | Layer 7 | Content delivery / edge caching |
| HTTP/HTTPS | Yes | Yes |
| Global Delivery | Yes | Yes |
| Application Routing | Yes | Limited |
| Path-Based Routing | Yes | Limited |
| Global Load Balancing | Yes | No |
| Origin Groups | Yes | No |
| Health Probes | Yes | Limited/depends on offering |
| Application Failover | Yes | Not its primary purpose |
| Content Caching | Yes | Yes |
| Static Content | Yes | Yes |
| Dynamic Applications | Yes | Not its primary focus |
| WAF Integration | Yes | Depends on CDN offering |
| TLS Termination | Yes | Yes |
| Multi-Region Applications | Excellent | Not its primary purpose |
| Cache Purging | Yes | Yes |

---

# Architecture Comparison

## Azure Front Door

```text
                         Users
                           │
                           ▼
                   Azure Front Door
                           │
                    Routing Decision
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
         Origin Group 1            Origin Group 2
              │                         │
              ▼                         ▼
          East US App              Europe App
```

Front Door decides where application requests should go.

---

## Azure CDN

```text
                         Users
                           │
                           ▼
                      CDN Edge
                           │
                    ┌──────┴──────┐
                    │             │
                Cache Hit      Cache Miss
                    │             │
                    ▼             ▼
                 Content        Origin
```

CDN focuses primarily on serving cached content efficiently.

---

# Routing

### Azure Front Door

Front Door supports application-level routing.

Example:

```text
example.com/
      ↓
Web Application

example.com/api/*
      ↓
API Application

example.com/images/*
      ↓
Storage Origin
```

### Azure CDN

CDN primarily delivers content based on the requested URL and caching configuration.

```text
cdn.example.com/image.jpg
          ↓
      CDN Edge
          ↓
    Cached Image
```

---

# Global Failover

Azure Front Door is well suited for multi-region application failover.

```text
                    Front Door
                        │
               ┌────────┴────────┐
               ▼                 ▼
           East US           West Europe
           Primary            Secondary
              ❌                  ✅
               │                 │
               └────────┬────────┘
                        ▼
                 Healthy Origin
```

If the primary origin becomes unhealthy, Front Door can route requests to another healthy origin.

Azure CDN is primarily focused on caching and content delivery rather than application-level global failover.

---

# Caching

Both services can cache content.

### Front Door

```text
User
 ↓
Front Door Edge
 ↓
Cache
 ↓
Origin
```

### CDN

```text
User
 ↓
CDN Edge
 ↓
Cache
 ↓
Origin
```

However, caching is the **core purpose of CDN**, while Front Door combines caching with global application delivery and routing.

---

# Use Azure Front Door When

Choose **Azure Front Door** when you need:

- Global web application delivery
- Multi-region applications
- Global load balancing
- Application failover
- URL/path-based routing
- Multiple origins
- Health-based routing
- WAF integration
- Global HTTPS application access

Example:

```text
Users
  │
  ▼
Front Door
  │
  ├── India → India Application
  ├── Europe → Europe Application
  └── US → US Application
```

---

# Use Azure CDN When

Choose **Azure CDN** when your primary requirement is:

- Static content delivery
- Image delivery
- CSS/JavaScript delivery
- Video delivery
- File downloads
- Content caching
- Reducing origin bandwidth
- Reducing latency for static content

Example:

```text
Users
  │
  ▼
Azure CDN
  │
  ▼
Cached Images / CSS / JS
```

---

# Front Door + CDN

For modern Azure architectures, Front Door itself provides content caching capabilities, so you should not automatically deploy a separate CDN alongside Front Door.

Use the service that matches the requirement.

```text
Application Routing
       ↓
Azure Front Door
```

```text
Static Content Caching
       ↓
Azure CDN
```

---

# Simple Decision Guide

| Requirement | Recommended Service |
|---|---|
| Host DNS records | Azure DNS |
| Global application routing | Azure Front Door |
| Global application failover | Azure Front Door |
| Multi-region web application | Azure Front Door |
| URL/path-based routing | Azure Front Door |
| WAF for global web application | Azure Front Door |
| Static content caching | Azure CDN |
| Image delivery | Azure CDN |
| CSS/JavaScript caching | Azure CDN |
| Video/file delivery | Azure CDN |
| Reduce origin load through caching | Azure CDN |

---

# Easy Way to Remember

```text
Azure DNS
    ↓
"Where does this domain resolve?"

Azure Traffic Manager
    ↓
"Which endpoint should this user be sent to?"

Azure Front Door
    ↓
"Which application origin should handle this HTTP/HTTPS request?"

Azure CDN
    ↓
"Can I serve this content faster from the edge?"
```

---

# Key Points

- Azure Front Door is primarily a **global application delivery service**.
- Azure CDN is primarily a **content caching and delivery service**.
- Front Door operates at Layer 7 for HTTP/HTTPS application traffic.
- Front Door supports global routing and application failover.
- Front Door supports origin groups and health probes.
- CDN focuses on caching and delivering content from edge locations.
- Both services can cache content.
- Front Door is better suited for global multi-region applications.
- CDN is better suited for static and cacheable content delivery.
- Choose based on whether the main requirement is **application routing** or **content caching**.
