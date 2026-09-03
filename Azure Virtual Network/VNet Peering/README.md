# VNet Peering

**VNet Peering** allows two Azure Virtual Networks (VNets) to communicate with each other over the Azure backbone network.

Instead of sending traffic through the public internet, peered VNets communicate using private IP addresses.

---

## What is VNet Peering?

VNet Peering creates a direct network connection between two VNets.

```text
┌──────────────────────┐
│      VNet-A          │
│  10.0.0.0/16         │
│                      │
│  ┌──────────────┐    │
│  │     VM-A     │    │
│  └──────────────┘    │
└──────────┬───────────┘
           │
           │ VNet Peering
           │ Azure Backbone
           │
┌──────────▼───────────┐
│      VNet-B          │
│  10.1.0.0/16         │
│                      │
│  ┌──────────────┐    │
│  │     VM-B     │    │
│  └──────────────┘    │
└──────────────────────┘
```

After peering is configured, resources in the VNets can communicate using their private IP addresses, subject to NSGs and other network controls.

---

# Why Use VNet Peering?

VNet Peering is commonly used when applications are distributed across multiple VNets.

For example:

```text
VNet-A
Application
    │
    │ Peering
    │
    ▼
VNet-B
Database
```

Common use cases:

- Connect application VNets
- Connect development and shared-service VNets
- Connect application and database VNets
- Connect VNets across Azure regions
- Build hub-and-spoke architectures
- Allow private communication between workloads

---

# How VNet Peering Works

A peering connection is created between two VNets.

For example:

```text
VNet-A
10.0.0.0/16
    │
    │ Peering
    │
    ▼
VNet-B
10.1.0.0/16
```

The VNets must have **non-overlapping address spaces**.

For example:

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.1.0.0/16
```

This works.

But:

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.0.0.0/16
```

is not a valid design for peering because the address spaces overlap.

---

# Regional VNet Peering

VNets in the **same Azure region** can be connected using VNet Peering.

Example:

```text
Azure Region
┌─────────────────────────────────┐
│                                 │
│  VNet-A              VNet-B     │
│  10.0.0.0/16         10.1.0.0/16│
│      │                   │       │
│      └────── Peering ────┘       │
│                                 │
└─────────────────────────────────┘
```

This is commonly called **VNet peering** or **regional VNet peering**.

---

# Global VNet Peering

**Global VNet Peering** connects VNets located in different Azure regions.

Example:

```text
East US                         West Europe
┌──────────────┐               ┌──────────────┐
│    VNet-A    │               │    VNet-B    │
│ 10.0.0.0/16  │               │ 10.1.0.0/16  │
└───────┬──────┘               └───────┬──────┘
        │                              │
        └──── Global VNet Peering ─────┘
```

This allows resources in different Azure regions to communicate using private IP addresses.

---

# VNet Peering Properties

When creating a peering, Azure provides options that control how the VNets interact.

Important options include:

### Allow Virtual Network Access

Allows resources in the peered VNets to communicate with each other.

```text
VNet-A ←→ VNet-B
```

---

### Allow Forwarded Traffic

Allows traffic that has been forwarded from another source to pass through the peering.

This can be useful when traffic is being inspected or forwarded through network appliances.

---

### Allow Gateway Transit

Allows a peered VNet to use the VPN or ExpressRoute gateway in the other VNet.

Example:

```text
Spoke VNet
     │
     │ Peering
     ▼
Hub VNet
     │
     ▼
VPN Gateway
     │
     ▼
On-Premises
```

The hub VNet contains the gateway, while the spoke can use that gateway through peering.

---

### Use Remote Gateways

Allows a VNet to use the remote VNet's VPN or ExpressRoute gateway.

For example:

```text
Spoke VNet
    │
    │ Use Remote Gateway
    ▼
Hub VNet
    │
    ▼
VPN Gateway
```

`Use remote gateways` is configured on the VNet that wants to use the remote gateway.

---

# Gateway Transit

**Gateway Transit** allows a peered VNet to use a VPN Gateway or ExpressRoute Gateway in another VNet.

This is useful in hub-and-spoke architectures.

```text
                 Hub VNet
              ┌─────────────┐
              │ VPN Gateway │
              └──────┬──────┘
                     │
             ┌───────┴───────┐
             │               │
          Peering          Peering
             │               │
             ▼               ▼
        Spoke VNet-A     Spoke VNet-B
```

Instead of creating a separate VPN Gateway in every spoke VNet, the spokes can use the hub gateway.

---

# Service Chaining

**Service chaining** allows traffic to be sent through a network virtual appliance or firewall in another VNet.

Example:

```text
Spoke VNet
     │
     │ Peering
     ▼
Hub VNet
     │
     ▼
Firewall / NVA
     │
     ▼
Destination
```

A route table/UDR can be used to direct traffic toward the network appliance.

For example:

```text
Destination: 10.20.0.0/16
Next Hop: Virtual Appliance
```

The virtual appliance can inspect or process the traffic before forwarding it.

---

# VNet Peering is Non-Transitive

VNet Peering does **not automatically provide transitive connectivity**.

Consider:

```text
VNet-A
   │
   │ Peering
   ▼
VNet-B
   │
   │ Peering
   ▼
VNet-C
```

This does **not** automatically mean:

```text
VNet-A ←→ VNet-C
```

The peering relationship between A and B does not automatically extend through B to C.

If A must communicate with C, you need an appropriate connectivity design, such as:

```text
VNet-A ←→ VNet-C
```

or a centralized networking architecture using a hub, routing, firewall, or Azure Virtual WAN.

---

# VNet Peering and NSGs

VNet Peering does not bypass NSGs.

Example:

```text
VM-A
 │
 │ Private traffic
 ▼
VNet Peering
 │
 ▼
VM-B
```

If an NSG blocks the required traffic, the communication will still fail.

Therefore, when troubleshooting peering, check:

- VNet peering status
- Address spaces
- NSG rules
- Route tables
- Effective routes
- VM firewall
- IP configuration

---

# VNet Peering vs VPN Gateway

| VNet Peering | VPN Gateway |
|---|---|
| Connects Azure VNets directly | Creates VPN connectivity |
| Uses Azure backbone | Uses encrypted VPN tunnels |
| Private Azure-to-Azure connectivity | Commonly used for Azure-to-on-premises |
| Low-latency private connectivity | Useful for hybrid connectivity |
| No VPN tunnel required | VPN tunnel required |

Example:

```text
VNet-A ←── Peering ──→ VNet-B
```

versus:

```text
Azure VNet
     │
     ▼
VPN Gateway
     │
  VPN Tunnel
     │
     ▼
On-Premises
```

---

# VNet Peering vs Azure Virtual WAN

| VNet Peering | Azure Virtual WAN |
|---|---|
| Direct VNet-to-VNet connectivity | Centralized WAN networking |
| Suitable for a smaller number of connections | Suitable for large distributed environments |
| Point-to-point model | Hub-based architecture |
| Peering between VNets | Virtual hubs connect VNets and networks |
| No central hub required | Uses Virtual WAN hubs |

Example:

### VNet Peering

```text
VNet-A ←→ VNet-B
```

### Virtual WAN

```text
             Virtual WAN
                  │
          ┌───────┴───────┐
          │ Virtual Hub   │
          └───────┬───────┘
             ┌────┴────┐
             ▼         ▼
           VNet-A    VNet-B
```

---

# Practical Lab — Create VNet Peering

## Objective

Create two VNets and connect them using VNet Peering.

### Architecture

```text
┌──────────────────────┐
│       VNet-A         │
│    10.0.0.0/16       │
│                      │
│   ┌──────────────┐   │
│   │     VM-A     │   │
│   └──────────────┘   │
└──────────┬───────────┘
           │
           │ VNet Peering
           │
┌──────────▼───────────┐
│       VNet-B         │
│    10.1.0.0/16       │
│                      │
│   ┌──────────────┐   │
│   │     VM-B     │   │
│   └──────────────┘   │
└──────────────────────┘
```

---

## Step 1: Create VNet-A

Open:

```text
Azure Portal
    ↓
Virtual networks
    ↓
Create
```

Configure:

| Setting | Value |
|---|---|
| Name | `ZeroToHero-VNet-A` |
| Address space | `10.0.0.0/16` |
| Subnet | `Subnet-A` |
| Subnet range | `10.0.1.0/24` |

Create the VNet.

---

## Step 2: Create VNet-B

Create another VNet:

| Setting | Value |
|---|---|
| Name | `ZeroToHero-VNet-B` |
| Address space | `10.1.0.0/16` |
| Subnet | `Subnet-B` |
| Subnet range | `10.1.1.0/24` |

Create the VNet.

---

## Step 3: Create the Peering

Open:

```text
ZeroToHero-VNet-A
    ↓
Peerings
    ↓
+ Add
```

Configure the local peering:

```text
Peering link name:
VNet-A-to-VNet-B

Remote virtual network:
ZeroToHero-VNet-B

Allow virtual network access:
Enabled
```

Create the peering.

---

## Step 4: Create the Reverse Peering

Azure peering uses a connection in each direction.

Create the reverse connection:

```text
ZeroToHero-VNet-B
    ↓
Peerings
    ↓
+ Add
```

Configure:

```text
Peering link name:
VNet-B-to-VNet-A

Remote virtual network:
ZeroToHero-VNet-A

Allow virtual network access:
Enabled
```

---

## Step 5: Verify Peering Status

Open both VNets:

```text
VNet-A
  ↓
Peerings

VNet-B
  ↓
Peerings
```

The peering state should show:

```text
Connected
```

---

## Step 6: Test Connectivity

If you created VMs in both VNets, test communication using their **private IP addresses**.

Example:

```text
VM-A
Private IP: 10.0.1.4
        │
        │ Peering
        ▼
VM-B
Private IP: 10.1.1.4
```

From VM-A:

```bash
ping 10.1.1.4
```

or test a specific application port:

```bash
nc -zv 10.1.1.4 80
```

> Connectivity can be blocked by NSGs or the operating system firewall, so a failed ping does not necessarily mean peering is broken.

---

# Key Points

- **VNet Peering** connects Azure VNets using the Azure backbone.
- Peered VNets can communicate using **private IP addresses**.
- VNets must use **non-overlapping address spaces**.
- **Regional VNet Peering** connects VNets in the same region.
- **Global VNet Peering** connects VNets across Azure regions.
- Peering is **non-transitive by default**.
- **Gateway Transit** allows a peered VNet to use a remote VPN/ExpressRoute gateway.
- **Service Chaining** can send traffic through a network virtual appliance.
- NSGs still control traffic across peered VNets.
- **VNet Peering** is best for direct VNet-to-VNet connectivity.
- **Azure Virtual WAN** is better suited for centralized, large-scale WAN connectivity.
