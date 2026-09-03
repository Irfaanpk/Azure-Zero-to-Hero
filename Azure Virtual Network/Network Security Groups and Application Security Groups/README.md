# 7.4 Network Security Groups and Application Security Groups

**Network Security Groups (NSGs)** and **Application Security Groups (ASGs)** are Azure networking features used to control and organize network traffic.

An **NSG** provides traffic filtering through security rules, while an **ASG** allows resources to be grouped logically and referenced in NSG rules.

---

## Network Security Group (NSG)

A **Network Security Group (NSG)** is a collection of security rules that allow or deny inbound and outbound network traffic.

NSGs can be associated with:

- Subnets
- Network Interfaces (NICs)

```text
Internet
   │
   ▼
  NSG
   │
   ▼
Subnet
   │
   ▼
  VM
```

---

## Why Use NSGs?

NSGs are used to:

- Allow required network traffic
- Deny unwanted traffic
- Control inbound connections
- Control outbound connections
- Restrict access to specific ports
- Restrict traffic from specific sources
- Control communication between Azure resources

---

## NSG Security Rules

Each NSG contains security rules that define whether network traffic should be allowed or denied.

A rule can specify:

- Source
- Source port
- Destination
- Destination port
- Protocol
- Direction
- Priority
- Action

Example:

```text
Source:      Internet
Destination: Any
Protocol:    TCP
Port:        443
Action:      Allow
Priority:    100
```

---

## Inbound Rules

Inbound rules control traffic coming **into** a resource or subnet.

Example:

```text
Internet
   │
   │ TCP 443
   ▼
  NSG
   │
   ▼
 Web VM
```

Example rule:

```text
Source: Any
Destination: Any
Protocol: TCP
Destination Port: 443
Action: Allow
Priority: 100
```

---

## Outbound Rules

Outbound rules control traffic leaving a resource or subnet.

Example:

```text
VM
 │
 ▼
NSG
 │
 │ TCP 443
 ▼
Internet
```

Example:

```text
Source: Any
Destination: Internet
Protocol: TCP
Destination Port: 443
Action: Allow
Priority: 100
```

---

## NSG Rule Priority

Each NSG rule has a priority between:

```text
100 - 4096
```

A **lower number has higher priority**.

Example:

```text
Priority 100 → Allow TCP 443
Priority 200 → Deny TCP 443
```

If both rules match the same traffic, the rule with priority `100` is evaluated first.

---

## Allow and Deny

NSG rules support two actions:

```text
Allow
Deny
```

Example:

```text
Allow TCP 443
Deny TCP 22
```

Result:

```text
HTTPS → Allowed
SSH    → Denied
```

---

## Default NSG Rules

Azure automatically creates default security rules in every NSG.

Common default rules include:

```text
AllowVNetInBound
AllowAzureLoadBalancerInBound
DenyAllInBound

AllowVNetOutBound
AllowInternetOutBound
DenyAllOutBound
```

Custom rules can be created with higher priorities to override applicable default rules.

---

# Stateful Nature of NSGs

Azure NSGs are **stateful**.

This means when a connection is allowed in one direction, the return traffic for that established connection is automatically allowed.

For example:

```text
Client
  │
  │ Request
  ▼
  VM
  │
  │ Response
  ▼
Client
```

If the inbound connection is allowed by the NSG, you do not need to create a separate outbound rule specifically to allow the response traffic.

### Example

Suppose:

```text
Inbound:
Allow TCP 443
```

A client connects to the VM:

```text
Client ── TCP 443 ──► VM
```

The VM's response:

```text
VM ── Response ──► Client
```

is allowed as part of the established connection.

> **NSGs are stateful — return traffic for an allowed connection is automatically permitted.**

---

# NSG Association

An NSG can be associated with:

### Subnet

```text
VNet
 │
 └── Subnet
      │
      └── NSG
           │
           ├── VM 1
           ├── VM 2
           └── VM 3
```

### Network Interface

```text
VM
 │
 └── NIC
      │
      └── NSG
```

An NSG can be associated with both a subnet and a NIC.

---

# Subnet-Level NSG

When an NSG is associated with a subnet, its rules apply to network traffic for resources in that subnet.

```text
VNet
 │
 └── WebSubnet
      │
      ├── VM 1
      ├── VM 2
      └── VM 3
```

This is useful when multiple resources need the same network security rules.

---

# NIC-Level NSG

An NSG can also be associated directly with a NIC.

```text
VM
 │
 └── NIC
      │
      └── NSG
```

This provides more specific control for an individual network interface.

---

# NSG with Subnet and NIC

An NSG can exist at both levels:

```text
VNet
 │
 └── Subnet
      │
      ├── NSG
      │
      └── NIC
           │
           └── NSG
                │
                ▼
                VM
```

For traffic to be allowed, the applicable security rules at both levels must allow the traffic.

---

# What is an Application Security Group?

An **Application Security Group (ASG)** allows Azure resources to be grouped according to their application role.

ASGs make it easier to create application-based NSG rules without manually specifying individual IP addresses.

Example:

```text
Web Servers
   │
   ├── VM 1
   ├── VM 2
   └── VM 3
        │
        ▼
      Web-ASG
```

Another group:

```text
Application Servers
   │
   ├── VM 4
   └── VM 5
        │
        ▼
      App-ASG
```

---

## Why Use Application Security Groups?

Without ASGs:

```text
10.0.1.4
10.0.1.5
10.0.1.6
```

You may need to manage individual IP addresses in security rules.

With ASGs:

```text
Web-ASG
   │
   ▼
NSG Rule
   │
   ▼
App-ASG
```

The rule can describe the application relationship rather than individual IP addresses.

---

# ASG with NSG

An ASG does **not** directly allow or deny traffic.

The **NSG provides the security rule**, while the **ASG provides logical grouping**.

Example:

```text
Web-ASG
   │
   │ TCP 8080
   ▼
App-ASG
```

NSG rule:

```text
Source:
Web-ASG

Destination:
App-ASG

Protocol:
TCP

Destination Port:
8080

Action:
Allow
```

---

# ASG Example

Consider a three-tier application:

```text
Web Tier
   │
   │ TCP 8080
   ▼
Application Tier
   │
   │ TCP 1433
   ▼
Database Tier
```

Create:

```text
Web-ASG
App-ASG
DB-ASG
```

NSG rules can then define:

```text
Web-ASG → App-ASG
TCP 8080 → Allow

App-ASG → DB-ASG
TCP 1433 → Allow
```

This provides application-based network traffic control.

---

# NSG vs ASG

| NSG | ASG |
|---|---|
| Controls network traffic | Groups resources logically |
| Contains security rules | Does not contain security rules |
| Allows or denies traffic | Used as source/destination in NSG rules |
| Can be associated with subnet or NIC | Associated with NICs |
| Uses IPs, ports, protocols, and ASGs | Simplifies application-based security rules |

---

# NSG and ASG Architecture

```text
                    VNet
                     │
          ┌──────────┴──────────┐
          │                     │
     Web Subnet            App Subnet
          │                     │
          ▼                     ▼
       Web VM                 App VM
          │                     │
       Web-ASG                App-ASG
          │                     │
          └────── TCP 8080 ────┘
                     │
                     ▼
                  NSG Rule
```

The **NSG controls the traffic**, while the **ASGs identify the application resources**.

---

# Important Points

- An **NSG controls inbound and outbound network traffic**.
- NSGs contain security rules.
- NSGs are **stateful**.
- NSGs can be associated with subnets or NICs.
- NSG rules have priorities from `100` to `4096`.
- Lower priority numbers are evaluated first.
- NSG rules support `Allow` and `Deny`.
- Azure automatically creates default NSG rules.
- An **ASG logically groups Azure resources** based on their application role.
- ASGs are used within NSG rules.
- ASGs do not replace NSGs.
- ASGs help simplify security rules for multi-tier applications.

---

# Lab: Configure a Network Security Group

## Objective

Create an NSG, configure inbound and outbound security rules, associate the NSG with a subnet, and test network access.

## Architecture

```text
                    Internet
                       │
                       ▼
                      NSG
                       │
                       ▼
                   WebSubnet
                       │
                       ▼
                      VM
```

## Part 1 — Create an NSG

1. Open the **Azure Portal**.
2. Search for **Network Security Groups**.
3. Select **Create**.
4. Select your subscription.
5. Select your resource group.
6. Enter:

```text
Name:
ZeroToHero-NSG
```

7. Select the same region as your VNet.
8. Select **Review + create**.
9. Select **Create**.

---

## Part 2 — Create an Inbound Rule

Open:

```text
ZeroToHero-NSG
    ↓
Inbound security rules
```

Create a rule:

```text
Source:
Any

Source port:
*

Destination:
Any

Destination port:
443

Protocol:
TCP

Action:
Allow

Priority:
100

Name:
Allow-HTTPS
```

Save the rule.

---

## Part 3 — Create Another Inbound Rule

Create a rule to allow SSH only from your own public IP.

```text
Source:
IP Addresses

Source IP:
YOUR_PUBLIC_IP

Destination:
Any

Destination port:
22

Protocol:
TCP

Action:
Allow

Priority:
110

Name:
Allow-SSH-From-My-IP
```

This allows SSH only from your specified public IP.

---

## Part 4 — Associate the NSG with a Subnet

Open:

```text
ZeroToHero-NSG
    ↓
Subnets
    ↓
Associate
```

Select:

```text
Virtual network:
ZeroToHero-VNet

Subnet:
WebSubnet
```

Save the association.

The architecture becomes:

```text
Internet
    │
    ▼
   NSG
    │
    ▼
WebSubnet
    │
    ▼
   VM
```

---

## Part 5 — Verify the Rules

Open:

```text
ZeroToHero-NSG
    ↓
Inbound security rules
```

Verify:

```text
100 → Allow HTTPS
110 → Allow SSH from your IP
```

Also review the Azure default rules.

---

## Part 6 — Test Connectivity

If you have a VM in the subnet:

### Test HTTPS

Try:

```text
https://VM_PUBLIC_IP
```

If a web server is configured and port `443` is listening, the connection can be allowed by the NSG.

### Test SSH

From your allowed public IP:

```bash
ssh username@VM_PUBLIC_IP
```

The NSG allows TCP port `22` from the configured source IP.

From another source IP, the connection should be blocked by the NSG rule configuration.

---

## Lab Result

You have learned how to:

- Create an NSG
- Create inbound security rules
- Configure rule priorities
- Allow HTTPS traffic
- Restrict SSH access to a specific IP
- Associate an NSG with a subnet
- Understand default NSG rules
- Test network traffic controlled by an NSG

> **Note:** ASGs are covered conceptually in this topic. A separate ASG lab is not required.
