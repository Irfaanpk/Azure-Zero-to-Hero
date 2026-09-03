# 7.4 Network Security Groups and Application Security Groups

**Network Security Groups (NSGs)** and **Application Security Groups (ASGs)** are Azure networking features used to control and organize network traffic between Azure resources.

NSGs provide the actual traffic filtering rules, while ASGs help organize resources into logical application groups that can be referenced in NSG rules.

---

## What is a Network Security Group?

A **Network Security Group (NSG)** is a collection of security rules that allow or deny inbound and outbound network traffic.

NSGs can be associated with:

- Subnets
- Network Interfaces (NICs)

Example:

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

or:

```text
Internet
   │
   ▼
  NIC
   │
   ▼
 NSG
   │
   ▼
  VM
```

---

## Why Use NSGs?

NSGs are used to:

- Allow required network traffic
- Block unwanted traffic
- Control inbound connections
- Control outbound connections
- Restrict access to specific ports
- Restrict traffic to specific IP addresses
- Control communication between network resources

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

The NSG can allow HTTPS traffic while blocking other unwanted inbound traffic.

---

# NSG Security Rules

Each NSG contains security rules.

A rule can define:

- Source
- Source port
- Destination
- Destination port
- Protocol
- Direction
- Action
- Priority

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

Inbound rules control traffic **coming into** a resource or subnet.

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
Priority: 100
Source: Any
Destination: Any
Protocol: TCP
Destination Port: 443
Action: Allow
```

This allows HTTPS traffic.

---

## Outbound Rules

Outbound rules control traffic **leaving** a resource or subnet.

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
Priority: 100
Source: Any
Destination: Internet
Protocol: TCP
Destination Port: 443
Action: Allow
```

---

# NSG Rule Priority

Every NSG rule has a priority.

The priority is a number between:

```text
100 - 4096
```

Lower numbers have **higher priority**.

Example:

```text
Priority 100
Priority 200
Priority 300
```

The rule with priority `100` is evaluated before the rule with priority `200`.

Example:

```text
Rule 1
Priority: 100
Allow TCP 443

Rule 2
Priority: 200
Deny TCP 443
```

The traffic is allowed because the higher-priority rule matches first.

---

# Allow and Deny Rules

NSGs support two actions:

```text
Allow
Deny
```

Example:

```text
Allow TCP 443
Deny TCP 22
```

This means:

```text
HTTPS → Allowed
SSH    → Denied
```

---

# Default NSG Rules

Azure automatically creates default security rules in an NSG.

Common default rules include:

```text
AllowVNetInBound
AllowAzureLoadBalancerInBound
DenyAllInBound

AllowVNetOutBound
AllowInternetOutBound
DenyAllOutBound
```

These default rules have lower priority than custom rules.

Custom rules can override the default behavior by using a higher priority.

---

# NSG Association

An NSG can be associated with either:

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

When both are present, traffic must satisfy the applicable security rules.

---

# Subnet-Level NSG

When an NSG is associated with a subnet, its rules apply to resources in that subnet.

Example:

```text
VNet
 │
 └── WebSubnet
      │
      ├── VM 1
      ├── VM 2
      └── VM 3
```

If an NSG is associated with `WebSubnet`, it can control traffic for those resources.

---

# NIC-Level NSG

An NSG can also be associated directly with a NIC.

Example:

```text
VM
 │
 └── NIC
      │
      └── NSG
```

This allows more specific network traffic control for an individual VM.

---

# NSG Traffic Evaluation

Consider:

```text
Internet
    │
    ▼
Subnet NSG
    │
    ▼
   NIC NSG
    │
    ▼
    VM
```

For inbound traffic, both the subnet-level and NIC-level security controls are considered.

For outbound traffic, the applicable rules at both levels are also evaluated.

Traffic must be allowed by the relevant security rules.

---

# Network Security Group Example

Suppose a web server needs:

```text
HTTPS → Port 443
```

but SSH should only be allowed from an administrator's IP.

Rules:

| Priority | Source | Port | Protocol | Action |
|---:|---|---:|---|---|
| 100 | Any | 443 | TCP | Allow |
| 110 | Admin IP | 22 | TCP | Allow |
| 120 | Any | 22 | TCP | Deny |

Result:

```text
Internet ── HTTPS 443 ──► Web VM ✓

Admin IP ── SSH 22 ─────► Web VM ✓

Other IP ── SSH 22 ─────► Web VM ✗
```

---

# What is an Application Security Group?

An **Application Security Group (ASG)** allows you to group Azure resources based on their application role.

Instead of writing NSG rules using individual IP addresses, you can reference ASGs.

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

# Why Use Application Security Groups?

ASGs make NSG rules easier to manage when an application has multiple resources.

Without ASGs:

```text
Allow:
10.0.1.4 → 10.0.2.4
10.0.1.5 → 10.0.2.4
10.0.1.6 → 10.0.2.4
```

With ASGs:

```text
Web-ASG
   │
   ▼
App-ASG
```

An NSG rule can allow traffic from one application group to another.

---

# ASG Architecture

Example three-tier application:

```text
                 VNet
                  │
       ┌──────────┼──────────┐
       │          │          │
       ▼          ▼          ▼
   WebSubnet  AppSubnet  DBSubnet
       │          │          │
       ▼          ▼          ▼
    Web-ASG     App-ASG     DB-ASG
       │          │          │
      VMs        VMs        DB
```

Traffic can be controlled between these application groups.

---

# ASG with NSG

ASGs do not directly allow or deny traffic.

The **NSG provides the security rule**, while the ASG provides the logical grouping.

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

Port:
8080

Action:
Allow
```

This allows application traffic from the Web servers to the Application servers.

---

# ASG Example

Suppose we have:

```text
Web Servers:
10.0.1.4
10.0.1.5

Application Servers:
10.0.2.4
10.0.2.5

Database Server:
10.0.3.4
```

Create:

```text
Web-ASG
App-ASG
DB-ASG
```

Then create NSG rules:

```text
Web-ASG → App-ASG
TCP 8080 → Allow

App-ASG → DB-ASG
TCP 1433 → Allow
```

Architecture:

```text
Web Servers
    │
    │ TCP 8080
    ▼
Application Servers
    │
    │ TCP 1433
    ▼
Database Server
```

---

# NSG vs ASG

| NSG | ASG |
|---|---|
| Controls network traffic | Groups resources logically |
| Contains security rules | Does not contain security rules |
| Allows or denies traffic | Used as source/destination in NSG rules |
| Can be associated with subnet/NIC | Associated with NICs |
| Works with IPs, ports, protocols, and ASGs | Helps simplify application-based rules |

---

# NSG vs ASG Example

Without ASG:

```text
10.0.1.4
10.0.1.5
10.0.1.6
        │
        ▼
NSG Rule
        │
        ▼
10.0.2.4
10.0.2.5
```

With ASG:

```text
Web-ASG
   │
   ▼
NSG Rule
   │
   ▼
App-ASG
```

ASGs make the rule easier to understand and maintain.

---

# Important Points

- **NSG controls network traffic.**
- NSGs contain inbound and outbound security rules.
- NSGs can be associated with subnets or NICs.
- NSG rules use priorities.
- Lower priority numbers are evaluated first.
- NSGs support Allow and Deny actions.
- Azure provides default NSG rules.
- Custom rules can be created with higher priorities.
- **ASG groups resources based on their application role.**
- ASGs are used inside NSG rules.
- ASGs simplify security rules for multi-tier applications.
- ASGs are associated with NICs.
- ASGs do not replace NSGs; they work together with NSGs.

---

# Lab: Configure NSG and ASG

## Objective

Create an NSG, configure inbound rules, create Application Security Groups, and use them in an NSG rule.

## Architecture

```text
                    VNet
                     │
          ┌──────────┴──────────┐
          │                     │
     Web Subnet            App Subnet
          │                     │
          ▼                     ▼
      Web-ASG                 App-ASG
          │                     │
       Web VM                App VM
          │                     │
          └────── TCP 8080 ────┘
```

## Part 1 — Create a Network Security Group

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

## Part 3 — Associate the NSG with a Subnet

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

---

# Part 4 — Create Application Security Groups

Search for:

```text
Application Security Groups
```

Create:

```text
Web-ASG
```

and:

```text
App-ASG
```

Use the same subscription, resource group, and region.

---

# Part 5 — Associate VMs with ASGs

Open the Web VM's NIC.

Go to:

```text
Networking
    ↓
Application security groups
```

Add:

```text
Web-ASG
```

For the Application VM, add:

```text
App-ASG
```

The architecture is now:

```text
Web VM
   │
   ▼
Web-ASG

App VM
   │
   ▼
App-ASG
```

---

# Part 6 — Create an ASG-Based NSG Rule

Open the NSG:

```text
ZeroToHero-NSG
    ↓
Inbound security rules
```

Create a rule:

```text
Source:
Application security group

Source ASG:
Web-ASG

Destination:
Application security group

Destination ASG:
App-ASG

Protocol:
TCP

Destination port:
8080

Action:
Allow

Priority:
110

Name:
Allow-Web-to-App
```

Save the rule.

---

## Final Architecture

```text
                 ZeroToHero-VNet
                       │
          ┌────────────┴────────────┐
          │                         │
     WebSubnet                  AppSubnet
          │                         │
          ▼                         ▼
       Web VM                    App VM
          │                         │
       Web-ASG                    App-ASG
          │                         │
          └────── TCP 8080 ────────┘
                     │
                  NSG Rule
                     │
                   Allow
```

## Lab Result

You have learned how to:

- Create an NSG
- Create inbound security rules
- Associate an NSG with a subnet
- Create ASGs
- Associate resources with ASGs
- Use ASGs in NSG rules
- Control application-to-application traffic
