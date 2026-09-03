# 15.5 Azure Monitor Agent and Data Collection Rules

## Azure Monitor Agent

**Azure Monitor Agent (AMA)** is an agent used to collect monitoring data from Azure virtual machines and other supported resources and send it to Azure Monitor.

It can collect guest operating system data such as:

- Windows Event Logs
- Linux Syslog
- Performance data
- Custom data sources

```text
Azure VM
   │
   ▼
Azure Monitor Agent
   │
   ▼
Data Collection Rule
   │
   ▼
Azure Monitor
   │
   ▼
Log Analytics Workspace
```

---

# Why Use Azure Monitor Agent?

Azure Monitor Agent is used to:

- Collect guest OS monitoring data
- Collect Windows Event Logs
- Collect Linux Syslog
- Collect performance counters
- Centralize monitoring data
- Apply consistent data collection configurations

---

# Data Collection Rule

A **Data Collection Rule (DCR)** defines what monitoring data should be collected, how it should be processed, and where it should be sent.

```text
Data Collection Rule
       │
       ├── Data Sources
       │
       ├── Data Transformations
       │
       └── Destinations
```

---

# DCR Components

A Data Collection Rule mainly defines:

### Data Sources

Specifies what data should be collected.

Examples:

```text
Windows Event Logs
Linux Syslog
Performance Counters
```

### Data Processing

Defines how collected data should be processed before it reaches the destination.

### Destinations

Specifies where the data should be sent.

Example:

```text
Log Analytics Workspace
```

---

# AMA and DCR Relationship

Azure Monitor Agent collects the data, while the Data Collection Rule controls the collection configuration.

```text
                 Data Collection Rule
                  /       |       \
                 /        |        \
                ▼         ▼         ▼
          Data Sources  Processing  Destination
                │                    │
                ▼                    ▼
        Azure Monitor Agent    Log Analytics
```

---

# Windows Event Logs

AMA can collect Windows Event Logs from Windows virtual machines.

Example:

```text
Windows VM
    │
    ▼
Windows Event Log
    │
    ▼
Azure Monitor Agent
    │
    ▼
Log Analytics Workspace
```

You can select event log channels and specify the severity levels to collect.

---

# Linux Syslog

AMA can collect Syslog data from Linux virtual machines.

Example:

```text
Linux VM
    │
    ▼
Syslog
    │
    ▼
Azure Monitor Agent
    │
    ▼
Log Analytics Workspace
```

Common Syslog facilities include:

```text
auth
cron
daemon
kern
syslog
user
```

---

# Performance Counters

AMA can collect performance information from monitored machines.

Examples:

```text
CPU
Memory
Disk
Network
```

Example:

```text
VM
 │
 ├── CPU
 ├── Memory
 ├── Disk
 └── Network
       │
       ▼
Azure Monitor Agent
       │
       ▼
Log Analytics
```

---

# Data Collection Rule Association

A DCR must be associated with the resources from which it should collect data.

Example:

```text
Data Collection Rule
        │
        ├── VM 1
        ├── VM 2
        └── VM 3
```

This allows the same collection configuration to be applied to multiple supported resources.

---

# DCR Data Flow

The overall flow is:

```text
Azure VM
   │
   ▼
Azure Monitor Agent
   │
   ▼
Data Collection Rule
   │
   ├── Collect
   ├── Process
   └── Route
         │
         ▼
Log Analytics Workspace
```

---

# AMA vs Diagnostic Settings

These services solve different monitoring requirements.

| Azure Monitor Agent | Diagnostic Settings |
|---|---|
| Collects guest OS data | Routes Azure resource monitoring data |
| Installed on supported machines | Configured on Azure resources |
| Windows Event Logs | Resource logs |
| Linux Syslog | Activity Log |
| Performance counters | Supported platform metrics |
| Uses Data Collection Rules | Defines destinations |

Example:

```text
VM Guest OS
    ↓
AMA + DCR
    ↓
Log Analytics
```

```text
Storage Account
    ↓
Diagnostic Settings
    ↓
Log Analytics
```

---

# AMA vs Log Analytics

These are also different components.

```text
Azure Monitor Agent
        ↓
Collects data

Data Collection Rule
        ↓
Defines collection

Log Analytics Workspace
        ↓
Stores and queries data
```

---

# Practical Lab

## Lab: Configure Azure Monitor Agent and Data Collection Rule

### Objective

Install Azure Monitor Agent on an Azure VM, create a Data Collection Rule, collect guest OS logs, and send them to a Log Analytics workspace.

---

## Step 1: Create a Log Analytics Workspace

1. Open **Azure Portal**.
2. Search for **Log Analytics workspaces**.
3. Select **Create**.
4. Create a workspace.

Example:

```text
Workspace:
law-ama-demo
```

5. Select **Review + create**.
6. Select **Create**.

---

## Step 2: Create or Select a Virtual Machine

Use an existing test VM or create a new Azure VM.

Example:

```text
VM:
ama-demo-vm
```

Make sure the VM is running.

---

## Step 3: Create a Data Collection Rule

1. Open **Azure Portal**.
2. Search for **Data Collection Rules**.
3. Select **Create**.
4. Enter a name.

Example:

```text
dcr-ama-demo
```

5. Select the appropriate region.
6. Select **Next: Resources**.

---

## Step 4: Add the Virtual Machine

Select:

```text
Add resources
```

Choose:

```text
ama-demo-vm
```

The VM will be associated with the DCR.

---

## Step 5: Configure Data Sources

Select **Data sources**.

For a Windows VM, add:

```text
Windows Event Logs
```

Select appropriate event log levels such as:

```text
Error
Warning
Information
```

You can also add performance counters if required.

For a Linux VM, you can configure:

```text
Syslog
```

---

## Step 6: Configure Destination

Select the destination:

```text
Log Analytics Workspace
```

Choose:

```text
law-ama-demo
```

---

## Step 7: Create the DCR

Review the configuration.

Select:

```text
Create
```

Azure will configure the Data Collection Rule and its association with the VM.

---

## Step 8: Verify Azure Monitor Agent

Open the VM.

Go to:

```text
VM
 ↓
Extensions + applications
```

Verify that the Azure Monitor Agent extension is installed.

---

## Step 9: Generate Test Data

On the VM, generate some activity.

For example:

```text
Windows:
Generate or wait for Windows Event Logs

Linux:
Generate Syslog activity
```

---

## Step 10: Query the Data

Open:

```text
Log Analytics Workspace
      ↓
Logs
```

For Windows Event Logs, query the relevant event table.

Example:

```kql
Event
| take 10
```

For Linux Syslog:

```kql
Syslog
| take 10
```

Verify that the collected data is available.

---

# Key Points

- **Azure Monitor Agent (AMA)** collects monitoring data from supported machines.
- AMA can collect Windows Event Logs, Linux Syslog, and performance data.
- **Data Collection Rules (DCRs)** define what data is collected, processed, and where it is sent.
- DCRs can be associated with multiple supported resources.
- Log Analytics Workspace can be used as a destination for collected data.
- AMA and DCR are commonly used together.
- Diagnostic Settings are primarily used to route Azure resource logs and supported metrics.
- AMA is focused on collecting **guest operating system and machine-level data**.
- Log Analytics provides centralized storage and querying of collected logs.
