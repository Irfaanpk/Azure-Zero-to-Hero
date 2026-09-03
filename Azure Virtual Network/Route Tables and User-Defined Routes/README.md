# Route Tables and User-Defined Routes

Azure uses routing to determine how network traffic moves between subnets, VNets, Azure services, and external networks.

By default, Azure automatically creates **system routes** for each subnet. When you need to control or change the traffic path, you can use **Route Tables** and **User-Defined Routes (UDRs)**.

---

## What is Azure Routing?

Routing is the process of deciding **where network traffic should go**.

For example:

```text
VM
 |
 | Traffic
 v
Subnet
 |
 | Route
 v
Next Hop
 |
 v
Destination
```

Azure checks the available routes and selects the route that matches the destination IP address.

---

# 1. System Routes

Azure automatically creates system routes when you create a VNet and subnet.

These routes allow Azure resources to communicate without manually configuring routes.

### Common System Routes

| Destination | Next Hop |
|---|---|
| VNet address space | Virtual network |
| 0.0.0.0/0 | Internet |
| Peered VNet | VNet peering |
| Azure service | Service-specific routing |
| On-premises network | Virtual network gateway, when configured |

You can view effective routes for a network interface to understand which routes Azure is actually using.

---

# 2. What is a Route Table?

An **Azure Route Table** is a collection of custom routes that you can apply to a subnet.

A route table itself does not route traffic.

Instead:

```text
Route Table
     |
     | Associated with
     v
   Subnet
     |
     v
Resources inside subnet
```

A route table can contain multiple custom routes.

---

# 3. What is a User-Defined Route (UDR)?

A **User-Defined Route (UDR)** is a custom route created by an administrator.

UDRs allow you to control where traffic is sent.

For example:

```text
VM
 |
 v
Subnet
 |
 | UDR
 v
Azure Firewall
 |
 v
Internet
```

Instead of allowing traffic to follow the default route directly to the Internet, you can force it through an Azure Firewall or another network virtual appliance.

---

# 4. Route Table vs UDR

| Route Table | User-Defined Route |
|---|---|
| Container for routes | Individual custom route |
| Associated with a subnet | Added to a route table |
| Can contain multiple routes | Defines one destination and next hop |
| Applied to a subnet | Controls traffic path |

Example:

```text
Route Table
│
├── Route 1 → 10.20.0.0/16 → Virtual appliance
├── Route 2 → 10.30.0.0/16 → Virtual network gateway
└── Route 3 → 0.0.0.0/0 → Internet
```

---

# 5. Route Prefix

A route prefix specifies the destination network.

Examples:

```text
10.0.0.0/16
10.0.1.0/24
192.168.1.0/24
0.0.0.0/0
```

The prefix determines which destination traffic the route applies to.

### Example

```text
Destination: 10.20.0.0/16
Next hop: Virtual appliance
```

This means:

> Traffic destined for `10.20.0.0/16` should be sent to the specified next hop.

---

# 6. Next Hop Types

A UDR specifies what should happen to matching traffic using a **next hop type**.

Common next-hop types include:

| Next Hop | Purpose |
|---|---|
| Virtual network | Send traffic within the VNet |
| Internet | Send traffic to the Internet |
| Virtual appliance | Send traffic to a firewall/NVA |
| Virtual network gateway | Send traffic through VPN/ExpressRoute gateway |
| None | Drop the traffic |

---

## Virtual Appliance

A virtual appliance is commonly used for traffic inspection or security.

Example:

```text
VM
 |
 v
Route Table
 |
 | 0.0.0.0/0
 v
Firewall / NVA
 |
 v
Internet
```

A UDR can force traffic through the appliance.

---

## Virtual Network Gateway

Traffic can be sent through a VPN Gateway or ExpressRoute gateway.

Example:

```text
Azure VNet
    |
    v
VPN Gateway
    |
    v
On-Premises Network
```

---

## None

The `None` next hop effectively drops traffic matching the route.

Example:

```text
Destination: 10.50.0.0/16
Next Hop: None
```

Traffic destined for that network is discarded.

---

# 7. Route Priority

Azure can have multiple routes for the same destination.

Azure uses the **most specific route** based on the longest matching prefix.

For example:

```text
Route 1:
10.0.0.0/8

Route 2:
10.10.0.0/16

Route 3:
10.10.1.0/24
```

For traffic going to:

```text
10.10.1.50
```

The `/24` route is the most specific match.

```text
10.10.1.0/24  ← Selected
```

---

# 8. Default Route

The default route is:

```text
0.0.0.0/0
```

It matches destinations that are not matched by a more specific route.

Example:

```text
Destination: 0.0.0.0/0
Next Hop: Internet
```

This means traffic without a more specific matching route can be sent toward the Internet.

A common use of UDRs is to replace or override the default path with a security appliance.

```text
VM
 |
 v
UDR: 0.0.0.0/0
 |
 v
Azure Firewall
 |
 v
Internet
```

---

# 9. Routing Through an Azure Firewall

A common enterprise architecture is to force subnet traffic through a firewall.

```text
                 Azure VNet
┌──────────────────────────────────────┐
│                                      │
│  Web Subnet                          │
│  ┌──────────┐                        │
│  │    VM    │                        │
│  └────┬─────┘                        │
│       │                              │
│       │ UDR                          │
│       v                              │
│  ┌──────────────┐                    │
│  │ Azure        │                    │
│  │ Firewall     │                    │
│  └──────┬───────┘                    │
│         │                            │
└─────────┼────────────────────────────┘
          v
       Internet
```

Example UDR:

```text
Address Prefix: 0.0.0.0/0
Next Hop Type: Virtual appliance
Next Hop Address: Firewall private IP
```

This forces matching traffic through the firewall.

---

# 10. Route Table Association

A route table must be associated with a subnet before its UDRs affect resources in that subnet.

```text
Route Table
     |
     | Association
     v
  Subnet
     |
     v
VM / NIC
```

A route table can be associated with multiple subnets, but each subnet can have only one route table association.

---

# 11. Route Tables and NSGs

Route tables and NSGs perform different functions.

| Route Table / UDR | NSG |
|---|---|
| Controls traffic path | Controls whether traffic is allowed/denied |
| Determines next hop | Uses allow/deny rules |
| Layer 3 routing | Network traffic filtering |
| Destination-based | Source/destination/port/protocol based |

Example:

```text
              Traffic
                 |
                 v
        ┌─────────────────┐
        │ Route Table     │
        │ Where to go?    │
        └────────┬────────┘
                 |
                 v
        ┌─────────────────┐
        │ NSG             │
        │ Allow or Deny?  │
        └────────┬────────┘
                 |
                 v
             Resource
```

---

# 12. Route Troubleshooting

When a VM cannot reach another network, check:

- VNet address spaces
- Subnet configuration
- Route tables
- UDRs
- Next-hop configuration
- NSG rules
- Peering configuration
- VPN Gateway configuration
- Firewall/NVA configuration
- Effective routes on the NIC

Azure Network Watcher can also help determine the next hop and troubleshoot connectivity.

---

# 13. Practical Lab — Create a Route Table and UDR

## Objective

Create a route table, add a UDR, associate it with a subnet, and inspect the resulting routes.

### Architecture

```text
                 Azure VNet
                     |
              ┌──────┴──────┐
              │             │
         WebSubnet      AppSubnet
              |
              v
             VM
              |
              | UDR
              v
        Custom Next Hop
```

---

## Step 1: Create a Route Table

1. Open **Azure Portal**
2. Search for **Route tables**
3. Select **Create**
4. Configure:

| Setting | Value |
|---|---|
| Subscription | Your subscription |
| Resource group | Your resource group |
| Region | Same region as VNet |
| Name | `ZeroToHero-RouteTable` |

5. Select **Review + create**
6. Select **Create**

---

## Step 2: Create a Route

Open the route table:

```text
ZeroToHero-RouteTable
```

Go to:

```text
Routes → Add
```

Configure:

| Setting | Value |
|---|---|
| Route name | `DefaultRoute` |
| Destination type | IP Addresses |
| Destination IP addresses/CIDR ranges | `0.0.0.0/0` |
| Next hop type | Virtual appliance |
| Next hop address | Firewall/NVA private IP |

> For a learning lab, you can create the route and inspect the configuration. Do not use a nonexistent next-hop address for actual connectivity testing.

---

## Step 3: Associate the Route Table with a Subnet

Inside the route table:

```text
Subnets → Associate
```

Select:

```text
Virtual network: ZeroToHero-VNet
Subnet: WebSubnet
```

Select **OK**.

The architecture is now:

```text
ZeroToHero-VNet
       |
       v
   WebSubnet
       |
       v
Route Table
       |
       v
     UDR
```

---

## Step 4: Verify the Route

Open the VM's network interface:

```text
VM
  ↓
Networking
  ↓
Network Interface
```

Look for:

```text
Effective routes
```

Review the routes available to the NIC.

You can identify:

- System routes
- User-defined routes
- Destination prefixes
- Next-hop types

---

# Key Points

- **System routes** are automatically created by Azure.
- **Route tables** contain custom routes.
- **UDRs** are administrator-defined custom routes.
- Route tables are associated with **subnets**.
- `0.0.0.0/0` represents the default route.
- UDRs can force traffic through an **Azure Firewall or network virtual appliance**.
- `Virtual network gateway` can be used for VPN/ExpressRoute traffic.
- `None` can be used to drop traffic.
- Azure selects the **most specific matching route**.
- **NSGs filter traffic; route tables determine traffic paths.**
- Effective routes help troubleshoot routing problems.
