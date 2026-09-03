# Azure Virtual WAN

**Azure Virtual WAN** is a networking service that provides a centralized architecture for connecting Azure VNets, branch offices, remote users, and other networks through Microsoft-managed virtual hubs.

It is useful when an organization has many networks and wants centralized connectivity and routing instead of managing many individual connections.

---

## What is Azure Virtual WAN?

Azure Virtual WAN provides a managed networking architecture based on **Virtual WAN hubs**.

```text
                    Azure Virtual WAN
                           │
              ┌────────────┴────────────┐
              │                         │
         Virtual Hub 1             Virtual Hub 2
              │                         │
        ┌─────┼─────┐             ┌─────┼─────┐
        │     │     │             │     │     │
      VNet   VNet  VPN          VNet   VPN  ExpressRoute
```

Instead of creating many direct connections between networks, Virtual WAN provides centralized hubs for connectivity.

---

# Why Use Azure Virtual WAN?

Virtual WAN is useful for organizations with:

- Multiple Azure VNets
- Multiple Azure regions
- Branch offices
- On-premises networks
- VPN connections
- ExpressRoute connections
- Large-scale distributed networks

Example:

```text
             Virtual WAN
                  │
        ┌─────────┴─────────┐
        │                   │
    Virtual Hub 1       Virtual Hub 2
        │                   │
    ┌───┼───┐           ┌───┼───┐
    │   │   │           │   │   │
   VNet VNet VPN       VNet VPN  ER
```

---

# Azure Virtual WAN Architecture

The main components are:

```text
Azure Virtual WAN
        │
        ├── Virtual Hubs
        │      │
        │      ├── VNet Connections
        │      ├── VPN Connections
        │      └── ExpressRoute Connections
        │
        └── Hub-to-Hub Connectivity
```

### Main Components

| Component | Purpose |
|---|---|
| Virtual WAN | Global networking framework |
| Virtual Hub | Regional networking hub |
| VNet Connection | Connects an Azure VNet to a hub |
| VPN Connection | Connects VPN/branch networks |
| ExpressRoute Connection | Connects ExpressRoute circuits |
| Hub-to-Hub Connectivity | Connects virtual hubs across regions |

---

# 1. Virtual WAN

A **Virtual WAN** is the top-level networking resource.

```text
Virtual WAN
     │
     ├── Hub - East US
     ├── Hub - West Europe
     └── Hub - Southeast Asia
```

It provides the overall framework for centralized WAN connectivity.

---

# 2. Virtual Hub

A **Virtual Hub** is a regional networking resource inside Virtual WAN.

Example:

```text
Virtual WAN
     │
     ├── East US Hub
     │
     └── West Europe Hub
```

A hub can connect different networks and provide centralized routing.

---

# 3. VNet Connections

VNets can be connected to a Virtual Hub.

```text
                 Virtual Hub
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
       VNet-A      VNet-B      VNet-C
```

This allows the VNets to participate in the Virtual WAN network.

---

# 4. Hub-to-Hub Connectivity

Virtual WAN can connect virtual hubs in different Azure regions.

```text
        East US Hub
             │
             │ Hub-to-Hub
             │ Connectivity
             ▼
      West Europe Hub
```

This provides connectivity between networks connected to different hubs.

For example:

```text
VNet-A
East US
   │
   ▼
East US Hub
   │
   │ Hub-to-Hub
   ▼
West Europe Hub
   │
   ▼
VNet-B
West Europe
```

This is the appropriate Azure terminology for what is sometimes informally called **Virtual WAN peering**.

---

# 5. Centralized Networking

Virtual WAN can provide a centralized networking architecture.

```text
                  Virtual WAN
                       │
              ┌────────┴────────┐
              │                 │
          East US Hub      West Europe Hub
              │                 │
        ┌─────┼─────┐       ┌───┼────┐
        │     │     │       │   │    │
      VNet   VNet   VPN    VNet VPN   ER
```

Instead of manually connecting every network to every other network, networks connect through the Virtual WAN architecture.

---

# Virtual WAN Routing

Virtual WAN provides centralized routing through its virtual hubs.

Example:

```text
VNet-A
   │
   ▼
Virtual Hub
   │
   ▼
VPN Connection
   │
   ▼
On-Premises
```

The hub determines how traffic should be forwarded between connected networks.

This is useful for centralized connectivity between:

- VNets
- Branches
- VPN sites
- ExpressRoute-connected networks

---

# Virtual WAN and Hub-and-Spoke

Traditional hub-and-spoke:

```text
              Hub VNet
             /        \
            /          \
       Spoke-A        Spoke-B
```

With Virtual WAN:

```text
             Virtual WAN
                  │
             Virtual Hub
             /    |    \
            /     |     \
        VNet-A  VNet-B  VNet-C
```

Virtual WAN provides a managed hub-based networking architecture.

---

# Virtual WAN vs VNet Peering

| VNet Peering | Azure Virtual WAN |
|---|---|
| Direct VNet-to-VNet connectivity | Centralized WAN networking |
| Point-to-point connectivity | Hub-based architecture |
| Good for smaller environments | Good for larger distributed environments |
| VNets are directly peered | VNets connect to Virtual Hubs |
| No central hub required | Uses Virtual Hubs |
| Does not provide automatic transitive connectivity | Designed for centralized multi-network connectivity |

### Example

VNet Peering:

```text
VNet-A ←────→ VNet-B
```

Virtual WAN:

```text
VNet-A
   │
   ▼
Virtual Hub
   ▲
   │
VNet-B
```

---

# Virtual WAN vs VPN Gateway

These services are related but serve different purposes.

| Azure VPN Gateway | Azure Virtual WAN |
|---|---|
| Provides VPN connectivity | Provides centralized WAN architecture |
| Can connect Azure to on-premises | Can connect many networks through hubs |
| Deployed as a VNet gateway | Uses Virtual Hubs |
| Suitable for individual VPN scenarios | Suitable for large distributed environments |

Virtual WAN can include VPN connectivity as part of its architecture.

---

# Virtual WAN Use Cases

### Multi-Region Azure

```text
East US
   │
Virtual Hub
   │
   │
West Europe
   │
Virtual Hub
```

### Branch Connectivity

```text
Branch A ──┐
Branch B ──┼── Virtual WAN ── Azure VNets
Branch C ──┘
```

### Hybrid Connectivity

```text
On-Premises
     │
     ▼
VPN / ExpressRoute
     │
     ▼
Virtual Hub
     │
     ▼
Azure VNets
```

---

# Practical Lab 1 — Create Virtual WAN and Connect a VNet

## Objective

Create an Azure Virtual WAN, create a Virtual Hub, and connect an existing VNet to the hub.

### Architecture

```text
Virtual WAN
     │
     ▼
Virtual Hub
     │
     ▼
ZeroToHero-VNet
```

### Steps

1. Open **Azure Portal**
2. Search for **Virtual WAN**
3. Select **Create**
4. Create a Virtual WAN
5. Open the Virtual WAN
6. Select **Hubs**
7. Create a Virtual Hub
8. Select the Azure region
9. Configure the hub
10. Open the hub
11. Select **Virtual network connections**
12. Select **Add connection**
13. Select the existing VNet
14. Create the connection
15. Verify that the VNet connection is established

The final architecture should look like:

```text
ZeroToHero-VNet
       │
       │ VNet Connection
       ▼
Virtual Hub
       │
       ▼
Virtual WAN
```

---

# Practical Lab 2 — Hub-to-Hub Connectivity

## Objective

Create two Virtual Hubs in different Azure regions and verify connectivity between them.

### Architecture

```text
              Virtual WAN
                   │
          ┌────────┴────────┐
          │                 │
     East US Hub       West Europe Hub
          │                 │
       VNet-A             VNet-B
```

### Steps

1. Open the existing **Virtual WAN**
2. Create a second Virtual Hub in another Azure region
3. Connect a VNet to the second hub
4. Verify both hubs belong to the same Virtual WAN
5. Verify **hub-to-hub connectivity**
6. Review the hub routing configuration
7. Test connectivity between resources connected through the hubs

---

# Important Considerations

### Address Spaces

Connected VNets should use appropriate, non-overlapping address spaces.

Example:

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.1.0.0/16
```

Avoid:

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.0.0.0/16
```

---

# Key Points

- **Azure Virtual WAN** provides centralized WAN networking.
- **Virtual Hub** is the regional networking hub.
- VNets can connect to Virtual Hubs.
- Virtual WAN supports **hub-to-hub connectivity**.
- Virtual WAN can connect Azure VNets, VPN sites, and ExpressRoute networks.
- It provides centralized routing and connectivity.
- **VNet Peering** is better for simple direct VNet-to-VNet connectivity.
- **Virtual WAN** is useful for large-scale, multi-region, and hybrid networking.
- "Virtual WAN peering" is better described as **hub-to-hub connectivity**.
