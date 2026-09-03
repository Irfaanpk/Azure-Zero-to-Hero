# 13.2 Azure Traffic Manager

## What is Azure Traffic Manager?

**Azure Traffic Manager** is a DNS-based traffic routing service that directs users to Azure or external application endpoints based on a configured routing method.

Traffic Manager operates at the **DNS level**.

```text
User
  │
  │ DNS Query
  ▼
Azure Traffic Manager
  │
  ├── Endpoint 1
  ├── Endpoint 2
  └── Endpoint 3
```

Traffic Manager does **not** directly proxy application traffic. After DNS resolution, the user connects directly to the selected endpoint.

---

# Why Use Azure Traffic Manager?

Traffic Manager can provide:

- Traffic distribution
- Application failover
- Geographic traffic routing
- Performance-based routing
- Priority-based routing
- Weighted traffic distribution
- Endpoint health monitoring
- Multi-region application availability

---

# How Traffic Manager Works

The basic flow is:

```text
User
 │
 │ 1. DNS Query
 ▼
Traffic Manager
 │
 │ 2. Selects healthy endpoint
 ▼
DNS Response
 │
 │ 3. Endpoint IP / DNS name
 ▼
User
 │
 │ 4. Connects directly
 ▼
Application Endpoint
```

Traffic Manager only participates in the DNS resolution process.

---

# Traffic Manager Profile

A **Traffic Manager profile** contains the configuration used to route DNS queries.

A profile defines:

- Routing method
- Endpoints
- Health probes
- DNS settings
- Monitoring configuration

Example:

```text
Traffic Manager Profile
        │
        ├── Routing Method
        │
        ├── Health Probe
        │
        └── Endpoints
             ├── Region 1
             ├── Region 2
             └── Region 3
```

---

# Traffic Manager Endpoints

An **endpoint** is a destination that Traffic Manager can direct users to.

Traffic Manager supports endpoints such as:

- Azure endpoints
- External endpoints
- Nested Traffic Manager profiles

Example:

```text
Traffic Manager
      │
      ├── Azure Endpoint
      │      └── App Service - East US
      │
      └── Azure Endpoint
             └── App Service - West Europe
```

---

# Endpoint Status

Traffic Manager can monitor endpoint health.

Common endpoint states include:

- Enabled
- Disabled
- Online
- Degraded

A disabled endpoint is not considered for traffic routing.

An unhealthy endpoint can be excluded from DNS responses depending on the configured routing method.

---

# Traffic Manager Routing Methods

Traffic Manager provides several routing methods.

| Routing Method | Purpose |
|---|---|
| Priority | Primary and backup endpoint routing |
| Weighted | Distribute traffic using assigned weights |
| Performance | Direct users to the endpoint with the lowest network latency |
| Geographic | Route users based on geographic location |
| Multivalue | Return multiple healthy endpoints |
| Subnet | Route users based on their source IP subnet |

---

# Priority Routing

**Priority routing** is used for primary and backup applications.

The endpoint with the highest priority is selected first.

Example:

```text
Traffic Manager
      │
      ├── Priority 1 → East US
      │
      └── Priority 2 → West Europe
```

If the primary endpoint becomes unhealthy:

```text
East US
   ❌ Unhealthy
      ↓
West Europe
   ✅ Healthy
```

Traffic Manager routes users to the backup endpoint.

### Use Case

Priority routing is useful for:

- Disaster recovery
- Primary/secondary applications
- Application failover

---

# Weighted Routing

**Weighted routing** distributes traffic according to weights assigned to endpoints.

Example:

```text
Traffic Manager
      │
      ├── Endpoint A → Weight 80
      │
      └── Endpoint B → Weight 20
```

Approximately:

```text
80% → Endpoint A
20% → Endpoint B
```

### Use Case

Weighted routing can be useful for:

- Gradual application deployment
- Testing
- Traffic distribution
- Blue/green deployment scenarios

---

# Performance Routing

**Performance routing** directs users to the endpoint that provides the lowest network latency from the user's location.

Example:

```text
User in India
      │
      ▼
Traffic Manager
      │
      └── Lowest latency
              ↓
          India Endpoint
```

### Use Case

Performance routing is useful when applications are deployed across multiple geographic regions.

---

# Geographic Routing

**Geographic routing** directs users based on their geographic location.

Example:

```text
Traffic Manager
      │
      ├── India → India Endpoint
      ├── Europe → Europe Endpoint
      └── North America → US Endpoint
```

### Use Case

Geographic routing can be useful for:

- Regional applications
- Data residency requirements
- Geographic traffic distribution

---

# Multivalue Routing

**Multivalue routing** returns multiple healthy endpoints in the DNS response.

Example:

```text
Traffic Manager
      │
      ├── Endpoint A ✅
      ├── Endpoint B ✅
      └── Endpoint C ❌
```

DNS response can contain:

```text
Endpoint A
Endpoint B
```

Only healthy endpoints are returned.

---

# Subnet Routing

**Subnet routing** maps specific IP address ranges to specific endpoints.

Example:

```text
Client Subnet
     │
     ├── 10.10.0.0/16 → Endpoint A
     │
     └── 10.20.0.0/16 → Endpoint B
```

This allows routing decisions based on the client's source IP subnet.

---

# Health Probes

Traffic Manager uses **health probes** to determine whether endpoints are available.

A probe can check:

- Protocol
- Port
- Path
- Endpoint health

Example:

```text
Traffic Manager
      │
      │ Health Probe
      ▼
https://app.example.com/health
      │
      ├── Healthy ✅
      └── Unhealthy ❌
```

Traffic Manager can stop returning unhealthy endpoints for DNS queries.

---

# Health Probe Configuration

A health probe can be configured with:

```text
Protocol:
HTTP / HTTPS / TCP

Port:
443

Path:
/
```

For HTTP/HTTPS probes, the endpoint should return a successful HTTP response.

---

# DNS TTL

**TTL (Time To Live)** determines how long DNS resolvers and clients can cache a Traffic Manager DNS response.

Example:

```text
TTL = 30 seconds
```

After the cached response expires, another DNS query can be made and Traffic Manager can provide an updated routing decision.

A lower TTL can help changes propagate faster, while a higher TTL can reduce DNS query frequency.

---

# Traffic Manager Failover

Traffic Manager can automatically route users away from unhealthy endpoints.

Example:

```text
User
 │
 ▼
Traffic Manager
 │
 ├── Primary Endpoint ❌
 │
 └── Secondary Endpoint ✅
              │
              ▼
         Application
```

This makes Traffic Manager useful for disaster recovery scenarios.

---

# Traffic Manager vs Load Balancer

| Traffic Manager | Azure Load Balancer |
|---|---|
| DNS-based routing | Network-level load balancing |
| Works across regions | Primarily regional |
| Routes users to endpoints | Distributes connections to backend VMs |
| Uses DNS responses | Uses frontend IP |
| Supports geographic/performance routing | Supports Layer 4 load balancing |
| Useful for global failover | Useful for distributing traffic within a region |

---

# Traffic Manager vs Application Gateway

| Traffic Manager | Application Gateway |
|---|---|
| DNS-based | Layer 7 |
| Global traffic routing | Regional application delivery |
| Routes users to endpoints | Routes HTTP/HTTPS requests |
| Health probes | Health probes |
| No application traffic proxying | Proxies application traffic |
| Supports geographic routing | Supports URL/path-based routing |
| Can provide DNS-level failover | Provides web traffic load balancing |

---

# Traffic Manager Use Cases

Traffic Manager is commonly used for:

- Multi-region applications
- Disaster recovery
- Application failover
- Global traffic distribution
- Performance-based routing
- Geographic routing
- Blue/green deployments
- Gradual traffic distribution

---

# Practical Lab

## Lab: Configure Azure Traffic Manager for Multi-Region Failover

### Objective

Create two application endpoints and configure Traffic Manager to route traffic using **Priority routing** with health probes.

### Architecture

```text
                    Internet
                       │
                       ▼
              Azure Traffic Manager
                       │
              Priority Routing
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
        Primary Endpoint   Secondary Endpoint
          East US             West Europe
              │                 │
              ▼                 ▼
         Web Application   Web Application
```

---

### Step 1: Create Two Web Applications

Create two Azure App Service Web Apps in different Azure regions.

Example:

```text
App 1:
Region: East US

App 2:
Region: West Europe
```

Make sure both applications are running.

---

### Step 2: Create Traffic Manager Profile

1. Open **Azure Portal**.
2. Search for **Traffic Manager profiles**.
3. Select **Create**.
4. Enter a profile name.

Example:

```text
tm-demo-profile
```

5. Select the routing method:

```text
Priority
```

6. Create the profile.

---

### Step 3: Add Primary Endpoint

Open the Traffic Manager profile.

1. Select **Endpoints**.
2. Select **Add**.
3. Enter:

```text
Endpoint type:
Azure endpoint

Name:
Primary

Target resource:
East US App Service
```

4. Configure the endpoint priority:

```text
Priority:
1
```

5. Save.

---

### Step 4: Add Secondary Endpoint

Add another endpoint:

```text
Endpoint type:
Azure endpoint

Name:
Secondary

Target resource:
West Europe App Service

Priority:
2
```

Save the endpoint.

The configuration becomes:

```text
Traffic Manager
      │
      ├── Priority 1 → East US
      │
      └── Priority 2 → West Europe
```

---

### Step 5: Configure Health Monitoring

Open the Traffic Manager profile's monitoring configuration.

Configure:

```text
Protocol:
HTTPS

Port:
443

Path:
/
```

Save the configuration.

Traffic Manager will periodically check the endpoints.

---

### Step 6: Test Traffic Manager

Open the Traffic Manager DNS name.

Example:

```text
tm-demo-profile.trafficmanager.net
```

Verify that traffic reaches the primary application.

---

### Step 7: Test Failover

Temporarily stop or make the primary application unhealthy.

Example:

```text
Primary
East US
   ❌
```

Wait for the health probe to detect the failure.

Traffic Manager should then route DNS resolution toward the secondary endpoint:

```text
Primary
   ❌
   ↓
Traffic Manager
   ↓
Secondary
   ✅
```

Open the Traffic Manager DNS name again and verify that the secondary application is serving the request.

---

# Key Points

- Azure Traffic Manager is a **DNS-based traffic routing service**.
- Traffic Manager does not proxy application traffic.
- Users connect directly to the selected endpoint after DNS resolution.
- A Traffic Manager profile contains routing and monitoring configuration.
- Endpoints can be Azure, external, or nested Traffic Manager profiles.
- Priority routing is useful for primary/secondary failover.
- Weighted routing distributes traffic based on configured weights.
- Performance routing selects endpoints based on network latency.
- Geographic routing routes users based on geographic location.
- Multivalue routing returns multiple healthy endpoints.
- Subnet routing routes based on client IP subnet.
- Health probes determine endpoint health.
- TTL controls DNS caching duration.
- Traffic Manager is useful for multi-region applications and disaster recovery.
