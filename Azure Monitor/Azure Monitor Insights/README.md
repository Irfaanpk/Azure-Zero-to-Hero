# 15.8 Azure Monitor Insights

## What are Azure Monitor Insights?

**Azure Monitor Insights** provide specialized monitoring views for supported Azure resources.

They make it easier to understand resource performance, health, dependencies, and operational data without manually analyzing individual metrics and logs.

```text
Azure Resources
      │
      ▼
Azure Monitor
      │
      ▼
   Insights
      │
      ├── VM Insights
      ├── Storage Insights
      └── Network Insights
```

---

# Why Use Azure Monitor Insights?

Insights help administrators:

- Monitor resource performance
- Identify performance problems
- View resource health
- Analyze utilization
- Understand dependencies
- Troubleshoot issues
- Visualize monitoring information

---

# VM Insights

**VM Insights** provides monitoring information for Azure Virtual Machines and Virtual Machine Scale Sets.

It can help monitor:

- CPU utilization
- Memory utilization
- Disk performance
- Network performance
- Processes
- Dependencies

```text
Azure VM
   │
   ▼
VM Insights
   │
   ├── Performance
   ├── Processes
   └── Dependencies
```

---

# VM Insights Performance

VM Insights provides performance information about monitored virtual machines.

Example:

```text
VM
 │
 ├── CPU
 ├── Memory
 ├── Disk
 └── Network
```

This helps identify resource bottlenecks.

---

# VM Insights Processes

VM Insights can provide information about processes running on monitored virtual machines.

Example:

```text
VM
 │
 └── Processes
       ├── nginx
       ├── sshd
       └── application
```

This can help identify processes consuming resources.

---

# VM Insights Dependencies

VM Insights can help visualize dependencies between a VM and other components.

Example:

```text
VM
 │
 ├── Web Application
 │
 ├── Database
 │
 └── External Service
```

This can help during application troubleshooting.

---

# Storage Insights

**Storage Insights** provides monitoring information for supported Azure Storage resources.

It can help analyze:

- Storage capacity
- Transactions
- Availability
- Requests
- Ingress
- Egress
- Performance

Example:

```text
Storage Account
      │
      ▼
Storage Insights
      │
      ├── Capacity
      ├── Transactions
      ├── Availability
      └── Traffic
```

---

# Network Insights

**Network Insights** provides monitoring and visualization capabilities for supported Azure networking resources.

It can help monitor resources such as:

- Virtual Networks
- VPN Gateways
- Application Gateways
- Load Balancers
- Network interfaces

Example:

```text
Azure Network
      │
      ▼
Network Insights
      │
      ├── Performance
      ├── Health
      └── Connectivity
```

---

# Insights vs Metrics

Metrics provide individual numerical measurements, while Insights provide a more complete monitoring experience for a specific resource type.

| Metrics | Insights |
|---|---|
| Individual measurements | Resource-focused monitoring view |
| CPU, requests, traffic, etc. | Combines relevant monitoring information |
| Metrics Explorer | Specialized Insights experience |
| General monitoring | Resource-specific monitoring |

Example:

```text
Metrics
  ↓
CPU = 85%

Insights
  ↓
CPU
Memory
Disk
Network
Processes
Dependencies
```

---

# Insights vs Logs

Logs provide detailed records that can be queried using KQL.

Insights provide dashboards and visualizations that make common monitoring scenarios easier to understand.

```text
Logs
  ↓
Detailed Data
  ↓
KQL Analysis
```

```text
Insights
  ↓
Pre-built Monitoring Views
  ↓
Quick Analysis
```

Both can be used together for troubleshooting.

---

# Insights and Log Analytics

Many Insights experiences use data collected through Azure Monitor and Log Analytics.

```text
Resource
   │
   ▼
Monitoring Data
   │
   ▼
Log Analytics
   │
   ▼
Azure Monitor Insights
```

The exact data collection requirements depend on the specific Insights experience.

---

# Example: VM Troubleshooting

Suppose a VM is experiencing slow performance.

### Step 1 — Open VM Insights

```text
Virtual Machine
      ↓
Insights
```

### Step 2 — Check Performance

```text
CPU      → 95%
Memory   → Normal
Disk     → Normal
Network  → Normal
```

The high CPU utilization indicates a potential performance issue.

### Step 3 — Check Processes

```text
Processes
     │
     └── application.exe → High CPU
```

### Step 4 — Investigate Logs

Use Log Analytics and KQL to investigate related events.

```text
VM Insights
     ↓
Identify Problem
     ↓
Log Analytics
     ↓
KQL
     ↓
Detailed Investigation
```

---

# Practical Lab

## Lab: Monitor an Azure VM Using VM Insights

### Objective

Enable VM Insights for an Azure Virtual Machine and use it to view performance information.

---

## Step 1: Select a Virtual Machine

1. Open **Azure Portal**.
2. Open **Virtual Machines**.
3. Select an existing test VM.

---

## Step 2: Open Insights

From the VM menu, select:

```text
Monitoring
   ↓
Insights
```

Follow the portal prompts if additional monitoring configuration is required.

---

## Step 3: Enable Required Monitoring

If prompted, configure the required monitoring components.

This may include:

```text
Azure Monitor Agent
        ↓
Data Collection Rule
        ↓
Log Analytics Workspace
```

These concepts are covered in **15.5**.

---

## Step 4: View Performance

Open the VM Insights performance view.

Review:

```text
CPU
Memory
Disk
Network
```

---

## Step 5: Analyze the VM

Use the available Insights views to identify:

- High CPU usage
- Memory utilization
- Disk activity
- Network activity
- Running processes
- Dependencies

---

## Step 6: Generate Some VM Activity

Connect to the VM and perform normal activity.

For example:

```text
Start an application
Download a file
Run a process
Generate network traffic
```

Return to VM Insights and observe the monitoring data.

---

# Key Points

- Azure Monitor Insights provide specialized monitoring experiences for supported Azure resources.
- **VM Insights** monitors virtual machines and VM Scale Sets.
- VM Insights can provide performance, process, and dependency information.
- **Storage Insights** provides monitoring information for supported Storage resources.
- **Network Insights** provides monitoring information for supported networking resources.
- Metrics provide individual measurements.
- Logs provide detailed records for analysis.
- Insights provide resource-focused monitoring views.
- Insights can be used together with Metrics and Log Analytics for troubleshooting.
- VM Insights commonly uses Azure Monitor Agent, Data Collection Rules, and Log Analytics.
