# VPN Gateway

**Azure VPN Gateway** is a managed Azure service that provides secure connectivity between Azure Virtual Networks and external networks using encrypted VPN connections.

It is mainly used for:

- Azure VNet ↔ On-premises network
- Azure VNet ↔ Azure VNet
- Individual client/device ↔ Azure VNet

---

## What is a VPN Gateway?

A VPN Gateway is deployed inside an Azure VNet and creates encrypted VPN connections.

```text
On-Premises Network
        │
        │ Encrypted VPN Tunnel
        │
        ▼
┌──────────────────────┐
│    Azure VPN Gateway │
└──────────┬───────────┘
           │
           ▼
      Azure VNet
```

---

# Types of VPN Connectivity

Azure VPN Gateway supports two important connectivity models:

| Type | Connection |
|---|---|
| Site-to-Site (S2S) VPN | On-premises network ↔ Azure VNet |
| Point-to-Site (P2S) VPN | Individual computer ↔ Azure VNet |

---

# 1. Site-to-Site VPN

A **Site-to-Site VPN** connects an entire on-premises network to an Azure VNet.

```text
On-Premises
10.10.0.0/16
     │
     │ VPN Tunnel
     ▼
VPN Gateway
     │
     ▼
Azure VNet
10.20.0.0/16
```

Once connected, systems on both networks can communicate through the VPN tunnel.

### Common Use Case

A company has:

```text
On-Premises
├── Web Servers
├── Application Servers
└── Database
```

and wants to connect them securely to Azure:

```text
On-Premises
     │
     │ Site-to-Site VPN
     ▼
Azure VPN Gateway
     │
     ▼
Azure VNet
├── Application VM
└── Database
```

---

# 2. Point-to-Site VPN

A **Point-to-Site VPN** connects an individual computer to an Azure VNet.

```text
Developer Laptop
      │
      │ VPN
      ▼
Azure VPN Gateway
      │
      ▼
Azure VNet
      │
      ▼
Azure Resources
```

It is useful when individual users need private access to Azure resources.

---

# VPN Gateway Components

A typical Site-to-Site VPN connection contains:

```text
On-Premises                         Azure
┌─────────────────┐          ┌─────────────────────┐
│ Local Network   │          │      Azure VNet     │
│                 │          │                     │
│ VPN Device      │◄────────►│   VPN Gateway       │
└─────────────────┘  Tunnel  └─────────────────────┘
       │                              │
       ▼                              ▼
Local Network                    GatewaySubnet
Gateway                          + Public IP
```

### Main Components

- **Virtual Network Gateway** — Azure VPN Gateway
- **Gateway Subnet** — Dedicated subnet used by the VPN Gateway
- **Public IP** — Public IP used by the VPN Gateway
- **Local Network Gateway** — Represents the on-premises network and VPN device
- **Connection** — Defines the VPN connection between the two gateways

---

# Gateway Subnet

Azure VPN Gateway requires a dedicated subnet named:

```text
GatewaySubnet
```

Example:

```text
VNet: 10.0.0.0/16

Subnets:
├── WebSubnet
│   10.0.1.0/24
│
├── AppSubnet
│   10.0.2.0/24
│
└── GatewaySubnet
    10.0.255.0/27
```

The VPN Gateway is deployed into the `GatewaySubnet`.

---

# Local Network Gateway

A **Local Network Gateway** represents the on-premises network from Azure's perspective.

It contains information such as:

- On-premises VPN device public IP
- On-premises address spaces
- Optional BGP settings

Example:

```text
Local Network Gateway

Name:
OnPrem-LNG

VPN Device Public IP:
203.0.113.10

Address Space:
10.10.0.0/16
```

Azure uses this information to know:

```text
10.10.0.0/16
       ↓
On-Premises
```

---

# How Azure Connects to On-Premises

The overall connection is:

```text
┌──────────────────────┐
│   On-Premises        │
│                      │
│  10.10.0.0/16        │
│                      │
│  VPN Device           │
│  Public IP            │
└──────────┬───────────┘
           │
           │ Encrypted IPsec/IKE VPN
           │ Tunnel
           │
┌──────────▼───────────┐
│   Azure VPN Gateway  │
│                      │
│   Public IP          │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│      Azure VNet      │
│                      │
│   10.20.0.0/16       │
│                      │
│   Azure Resources    │
└──────────────────────┘
```

---

# How to Connect an On-Premises Network to Azure

The connection conceptually works like this:

```text
1. Create Azure VNet
          ↓
2. Create GatewaySubnet
          ↓
3. Create Public IP
          ↓
4. Create Azure VPN Gateway
          ↓
5. Create Local Network Gateway
          ↓
6. Create VPN Connection
          ↓
7. Configure On-Premises VPN Device
          ↓
8. Encrypted VPN Tunnel Established
```

The important point is that **both sides must be configured**.

Azure side:

```text
Azure VPN Gateway
        ↕
VPN Connection
        ↕
Local Network Gateway
```

On-premises side:

```text
VPN Device
        ↕
Internet
        ↕
Azure VPN Gateway
```

---

# VPN Tunnel

The actual communication happens through an encrypted VPN tunnel.

```text
On-Premises VPN Device
        │
        │
        │  Encrypted Tunnel
        │══════════════════════│
        │                      │
        ▼                      ▼
Internet              Azure VPN Gateway
                              │
                              ▼
                          Azure VNet
```

Common VPN protocols include:

- **IKEv2**
- **IPsec**

---

# Route-Based VPN

Azure VPN Gateway commonly uses **route-based VPN**.

Traffic is selected based on destination networks.

Example:

```text
Destination:
10.10.0.0/16

Route:
10.10.0.0/16
      ↓
VPN Gateway
      ↓
On-Premises
```

This allows Azure to send traffic destined for the on-premises network through the VPN tunnel.

---

# Azure-to-Azure VPN

VPN Gateway can also connect Azure VNets.

```text
VNet-A
10.0.0.0/16
    │
    ▼
VPN Gateway
    │
    │ VPN
    │
    ▼
VPN Gateway
    │
    ▼
VNet-B
10.1.0.0/16
```

However, when both networks are Azure VNets, **VNet Peering** is normally preferred for direct Azure-to-Azure connectivity.

---

# VPN Gateway vs VNet Peering

| VPN Gateway | VNet Peering |
|---|---|
| Creates VPN tunnels | Direct Azure network connection |
| Uses IPsec/IKE | Uses Azure backbone |
| Useful for hybrid connectivity | Useful for Azure VNet-to-VNet |
| Connects Azure to on-premises | Connects Azure VNets |
| Requires VPN Gateway | Does not require VPN Gateway |

---

# VPN Gateway vs ExpressRoute

| VPN Gateway | ExpressRoute |
|---|---|
| Uses encrypted VPN over Internet | Private dedicated connectivity |
| Internet-based | Does not use public Internet for the connection |
| Usually simpler and lower cost | Typically more expensive |
| Good for many hybrid scenarios | Used for dedicated enterprise connectivity |

---

# Key Points

- **VPN Gateway** provides secure VPN connectivity to Azure VNets.
- **Site-to-Site VPN** connects an entire on-premises network to Azure.
- **Point-to-Site VPN** connects an individual client to Azure.
- VPN Gateway requires a dedicated **GatewaySubnet**.
- A **Local Network Gateway** represents the on-premises network.
- A **Connection** joins the Azure VPN Gateway and Local Network Gateway.
- VPN traffic is encrypted using technologies such as **IPsec/IKE**.
- For direct Azure VNet-to-VNet connectivity, **VNet Peering** is usually preferred.
- VPN Gateway is especially important for **hybrid Azure environments**.
