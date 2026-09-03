# 13.3 Azure DNS and Traffic Manager Comparison

## Azure DNS vs Azure Traffic Manager

Azure DNS and Azure Traffic Manager are both related to DNS, but they solve different problems.

- **Azure DNS** → Hosts and manages DNS zones and records
- **Azure Traffic Manager** → Routes users to application endpoints using DNS-based traffic routing

---

## Basic Difference

```text
Azure DNS
    │
    ├── DNS Zone
    ├── DNS Records
    └── Name Resolution
```

```text
Azure Traffic Manager
    │
    ├── Traffic Manager Profile
    ├── Endpoints
    ├── Routing Method
    └── Health Probes
```

---

# Azure DNS

Azure DNS is used to **host DNS zones and manage DNS records**.

Example:

```text
www.example.com
       ↓
    Azure DNS
       ↓
   20.10.10.5
```

### Main Features

- Public DNS zones
- Private DNS zones
- DNS records
- DNS resolution
- VNet integration for Private DNS
- Domain delegation

---

# Azure Traffic Manager

Azure Traffic Manager is used to **route users to different application endpoints** based on a configured routing method.

Example:

```text
User
  │
  ▼
Traffic Manager
  │
  ├── East US
  │
  └── West Europe
```

### Main Features

- Priority routing
- Weighted routing
- Performance routing
- Geographic routing
- Multivalue routing
- Subnet routing
- Health probes
- DNS-based failover

---

# Comparison Table

| Feature | Azure DNS | Azure Traffic Manager |
|---|---|---|
| Primary Purpose | DNS hosting | Traffic routing |
| DNS Zones | Yes | No |
| DNS Records | Yes | No |
| DNS Resolution | Yes | Uses DNS for routing |
| Public DNS | Yes | No |
| Private DNS | Yes | No |
| VNet Integration | Private DNS | No |
| Traffic Routing | No | Yes |
| Health Probes | No | Yes |
| Failover | DNS management only | Yes |
| Geographic Routing | No | Yes |
| Performance Routing | No | Yes |
| Weighted Routing | No | Yes |
| Priority Routing | No | Yes |
| Multiregion Routing | Not its purpose | Yes |

---

# Azure DNS vs Traffic Manager Architecture

### Azure DNS

```text
Client
  │
  │ DNS Query
  ▼
Azure DNS
  │
  ▼
DNS Record
  │
  ▼
IP Address
```

### Traffic Manager

```text
Client
  │
  │ DNS Query
  ▼
Traffic Manager
  │
  ▼
Routing Decision
  │
  ▼
Selected Endpoint
  │
  ▼
Client connects directly
```

---

# When to Use Azure DNS?

Use **Azure DNS** when you need to:

- Host a DNS zone
- Create DNS records
- Manage domain name resolution
- Configure public DNS
- Configure private DNS
- Resolve private Azure resources

Example:

```text
example.com
    │
    ├── www → Web Server
    ├── api → API Server
    └── mail → Mail Server
```

---

# When to Use Traffic Manager?

Use **Azure Traffic Manager** when you need to:

- Distribute users across regions
- Implement application failover
- Route users based on performance
- Route users based on geography
- Gradually distribute traffic
- Build multi-region applications

Example:

```text
                 Traffic Manager
                       │
              ┌────────┴────────┐
              ▼                 ▼
          East US           West Europe
          Primary            Secondary
             ✅                  ✅
```

---

# Can Azure DNS and Traffic Manager Work Together?

Yes.

They can be used together when an application requires both DNS management and traffic routing.

Example:

```text
                User
                  │
                  ▼
              DNS Query
                  │
                  ▼
            Traffic Manager
                  │
          ┌───────┴───────┐
          ▼               ▼
       Region 1         Region 2
          │               │
          ▼               ▼
      Application     Application
```

Azure DNS can host the application's DNS zone, while Traffic Manager can provide DNS-based traffic routing to application endpoints.

---

# Azure DNS vs Traffic Manager vs Load Balancer

| Service | Main Purpose | Scope |
|---|---|---|
| Azure DNS | DNS hosting and resolution | DNS |
| Traffic Manager | DNS-based traffic routing | Global |
| Azure Load Balancer | Layer 4 load balancing | Regional |
| Application Gateway | Layer 7 web traffic routing | Regional |

### Simple Way to Remember

```text
Azure DNS
    ↓
"Where is this domain?"

Traffic Manager
    ↓
"Which endpoint should this user go to?"

Load Balancer
    ↓
"Which backend VM should handle this connection?"

Application Gateway
    ↓
"Which backend should handle this web request?"
```

---

# Key Points

- **Azure DNS** is primarily a DNS hosting service.
- **Traffic Manager** is primarily a DNS-based traffic routing service.
- Azure DNS manages DNS zones and records.
- Traffic Manager manages endpoints and routing policies.
- Traffic Manager uses health probes to determine endpoint availability.
- Traffic Manager can route traffic across multiple regions.
- Azure DNS does not provide application traffic routing.
- Traffic Manager does not proxy application traffic.
- Azure DNS and Traffic Manager can be used together.
- Traffic Manager is useful for global routing and application failover.
