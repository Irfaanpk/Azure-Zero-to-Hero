# Azure Network Watcher

**Azure Network Watcher** is a regional network monitoring and diagnostic service that helps you troubleshoot and understand network connectivity in Azure.

It provides tools to diagnose problems involving:

- Virtual Machines
- Network Interfaces
- Network Security Groups
- Route tables
- VNets
- Connectivity between Azure resources

---

## Why Use Network Watcher?

When a VM cannot communicate with another resource, several things could be causing the problem:

```text
VM
 │
 ├── NSG
 ├── Route Table
 ├── VNet
 ├── Peering
 ├── Firewall
 └── Network Interface
```

Network Watcher provides diagnostic tools that help identify where the problem is.

---

# Network Watcher Tools

Important tools for AZ-104 include:

| Tool | Purpose |
|---|---|
| IP Flow Verify | Determines whether traffic is allowed or denied |
| Next Hop | Determines the next hop for traffic |
| Connection Troubleshoot | Tests connectivity between endpoints |
| Network Topology | Displays network resources and relationships |
| Packet Capture | Captures network traffic from a VM |

---

# 1. IP Flow Verify

**IP Flow Verify** checks whether a specific network flow is allowed or denied by the configured NSGs.

For example:

```text
Source:
10.0.1.10

Destination:
10.0.2.10

Port:
22

Protocol:
TCP
```

Network Watcher evaluates the traffic and returns:

```text
Access: Allowed
```

or:

```text
Access: Denied
```

It also identifies the NSG rule responsible for the result.

### Example

```text
VM-A
 │
 │ TCP 22
 ▼
VM-B
 │
 ▼
IP Flow Verify
 │
 ├── Allowed
 │
 └── NSG Rule: Allow-SSH
```

This is especially useful when troubleshooting NSGs.

---

# 2. Next Hop

**Next Hop** determines where Azure will send traffic from a VM to a specific destination.

Example:

```text
VM
 │
 │ Destination:
 │ 10.2.0.0/16
 ▼
Next Hop
 │
 ▼
Virtual Network
```

Another example:

```text
VM
 │
 │ Destination:
 │ 0.0.0.0/0
 ▼
Next Hop
 │
 ▼
Virtual Appliance
```

This is useful for troubleshooting:

- Route tables
- UDRs
- Azure Firewall
- Network virtual appliances
- VPN Gateway
- Unexpected routing

---

# 3. Connection Troubleshoot

**Connection Troubleshoot** tests network connectivity between a source and destination.

Example:

```text
VM-A
 │
 │ TCP 443
 ▼
VM-B
```

Network Watcher can test whether the connection works and provide information about the connectivity path.

It can help identify problems such as:

- NSG blocking traffic
- Incorrect routing
- Destination unavailable
- Port not listening
- Network connectivity problems

---

# 4. Network Topology

**Network Topology** provides a visual representation of network resources and their relationships.

Example:

```text
VNet
│
├── Subnet
│    │
│    ├── NIC
│    │    │
│    │    └── VM
│    │
│    └── NSG
│
└── Route Table
```

It helps you understand how resources are connected.

---

# 5. Packet Capture

**Packet Capture** allows you to capture network traffic from a VM's network interface.

Example:

```text
VM
 │
 │ Network Traffic
 ▼
Network Watcher
 │
 ▼
Packet Capture
 │
 ▼
Capture File
```

Packet captures can help troubleshoot:

- Application connectivity
- Network traffic
- Port communication
- Protocol-level issues
- Unexpected traffic

Packet capture should be used carefully because capture files can contain sensitive network information.

---

# Network Watcher and NSG Troubleshooting

Suppose:

```text
VM-A
10.0.1.10
     │
     │ TCP 443
     ▼
VM-B
10.0.2.10
```

The connection fails.

You can use:

```text
IP Flow Verify
       │
       ▼
Check NSG
       │
       ▼
Allow / Deny
```

If traffic is allowed, check routing:

```text
Next Hop
   │
   ▼
Route Table / UDR
```

Then test the connection:

```text
Connection Troubleshoot
          │
          ▼
Connectivity Result
```

---

# Network Watcher and Route Troubleshooting

Network Watcher works well with the routing concepts covered earlier.

```text
VM
 │
 ▼
NIC
 │
 ▼
Effective Routes
 │
 ▼
Next Hop
 │
 ▼
Destination
```

If the traffic is going through the wrong destination:

```text
VM
 │
 ▼
Next Hop
 │
 ▼
Unexpected Route
```

you can inspect the route table and UDR configuration.

---

# Network Watcher and NSG

Network Watcher can help determine why traffic is being blocked.

```text
Traffic
   │
   ▼
NSG
   │
   ▼
IP Flow Verify
   │
   ├── Allowed
   │
   └── Denied
          │
          ▼
     Matching Rule
```

This makes IP Flow Verify particularly useful when working with NSGs.

---

# Network Watcher vs AWS Networking Tools

For AWS users, Network Watcher does not have a single exact AWS equivalent.

The functionality is spread across several AWS tools.

| Azure Network Watcher | AWS Equivalent |
|---|---|
| IP Flow Verify | VPC Flow Logs / security analysis tools |
| Next Hop | VPC route tables / Reachability Analyzer |
| Connection Troubleshoot | VPC Reachability Analyzer |
| Network Topology | VPC Resource Map / network visualization |
| Packet Capture | VPC Traffic Mirroring |

The concepts are similar, but the services and capabilities are not one-to-one.

---

# Practical Lab — Troubleshoot VM Connectivity

## Objective

Use Network Watcher to determine why traffic between two VMs is failing.

### Architecture

```text
VM-A
10.0.1.10
   │
   │ TCP 80
   ▼
VM-B
10.0.2.10
```

---

## Step 1: Open Network Watcher

In the Azure Portal:

```text
Search
  ↓
Network Watcher
```

Select the Network Watcher for the region containing your resources.

---

## Step 2: Use IP Flow Verify

Open:

```text
Network Watcher
    ↓
IP flow verify
```

Select the source VM/NIC and configure:

```text
Direction:
Inbound / Outbound

Protocol:
TCP

Local IP:
VM-B private IP

Local Port:
80

Remote IP:
VM-A private IP

Remote Port:
Any
```

Run the test.

Network Watcher will return:

```text
Access: Allowed
```

or:

```text
Access: Denied
```

If denied, it will identify the matching NSG rule.

---

## Step 3: Check Next Hop

Open:

```text
Network Watcher
    ↓
Next hop
```

Select the VM and specify the destination IP.

Review the returned:

```text
Next Hop Type
Next Hop IP
Route
```

This helps determine whether the traffic is following the expected route.

---

## Step 4: Use Connection Troubleshoot

Open:

```text
Network Watcher
    ↓
Connection troubleshoot
```

Select:

```text
Source:
VM-A

Destination:
VM-B

Protocol:
TCP

Destination Port:
80
```

Run the test.

Review the connectivity result and diagnostic information.

---

# Troubleshooting Flow

A useful troubleshooting sequence is:

```text
        Connectivity Problem
                │
                ▼
         IP Flow Verify
                │
          ┌─────┴─────┐
          │           │
        Denied      Allowed
          │           │
          ▼           ▼
       Check NSG   Next Hop
                      │
                      ▼
                Check Routing
                      │
                      ▼
             Connection Troubleshoot
                      │
                      ▼
                Check VM / Port
```

---

# Key Points

- **Azure Network Watcher** is used for Azure network monitoring and troubleshooting.
- **IP Flow Verify** checks whether traffic is allowed or denied by NSGs.
- **Next Hop** helps determine where traffic will be routed.
- **Connection Troubleshoot** tests connectivity between endpoints.
- **Network Topology** shows network resources and relationships.
- **Packet Capture** captures traffic from a VM for deeper troubleshooting.
- Network Watcher is especially useful for troubleshooting **NSGs, routes, and connectivity**.
- Network Watcher does not replace Azure Monitor; it focuses on **network-specific diagnostics**.
- AWS does not have one exact equivalent; similar functionality is distributed across several AWS networking tools.
