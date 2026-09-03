# 15.1 Introduction to Azure Monitor

## What is Azure Monitor?

**Azure Monitor** is a monitoring and observability service in Microsoft Azure that collects and analyzes data from Azure resources, applications, and other environments.

It helps you understand:

- Resource performance
- Application performance
- Availability
- Errors and failures
- Resource activity
- Operational issues

```text
Azure Resources
      │
      ▼
Azure Monitor
      │
      ├── Metrics
      ├── Logs
      ├── Alerts
      └── Insights
```

---

# Why Use Azure Monitor?

Azure Monitor helps administrators:

- Monitor resource health
- Track performance
- Identify problems
- Troubleshoot issues
- Detect abnormal activity
- Create alerts
- Analyze logs
- Monitor applications
- Understand resource utilization

---

# Azure Monitor Architecture

The basic monitoring flow is:

```text
Azure Resources
      │
      ▼
Monitoring Data
      │
      ├── Metrics
      └── Logs
            │
            ▼
       Azure Monitor
            │
      ┌─────┼─────┐
      ▼     ▼     ▼
   Analyze Alerts Insights
```

---

# Monitoring Data

Azure Monitor collects different types of monitoring data.

The two major categories are:

```text
Azure Monitor
     │
     ├── Metrics
     │
     └── Logs
```

### Metrics

Metrics are numerical measurements collected at regular intervals.

Examples:

- CPU percentage
- Memory usage
- Network traffic
- Request count
- Storage capacity
- Availability

Example:

```text
VM CPU Usage

10:00 → 25%
10:05 → 40%
10:10 → 75%
10:15 → 90%
```

Metrics are useful for quickly understanding resource performance.

---

### Logs

Logs contain detailed records of events and activities.

Examples:

- Application errors
- Resource events
- Security-related events
- Network events
- Operating system events

Logs can be queried and analyzed using **Log Analytics and KQL**.

```text
Logs
  │
  ▼
Log Analytics Workspace
  │
  ▼
KQL Query
  │
  ▼
Results
```

---

# Metrics vs Logs

| Metrics | Logs |
|---|---|
| Numerical measurements | Detailed records |
| Usually lightweight | Can contain detailed information |
| Good for performance trends | Good for investigation |
| Near real-time monitoring | Detailed analysis |
| Example: CPU = 80% | Example: Application error |

---

# Azure Monitor Data Sources

Azure Monitor can collect data from:

- Azure resources
- Virtual machines
- Applications
- Operating systems
- Azure Activity Log
- Resource logs
- Custom sources

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
   Azure Monitor
```

---

# Azure Activity Log

The **Azure Activity Log** records subscription-level management operations.

Examples:

- Resource creation
- Resource deletion
- Resource configuration changes
- Role assignments
- Policy changes

Example:

```text
Administrator
      │
      ▼
Delete Storage Account
      │
      ▼
Azure Activity Log
```

Activity Log is automatically collected and retained by Azure for a limited period.

---

# Resource Logs

**Resource logs** provide information about operations performed within an Azure resource.

Examples:

```text
Storage Account
      ↓
Resource Logs

Key Vault
      ↓
Resource Logs

Application Gateway
      ↓
Resource Logs
```

Resource logs generally need to be configured through **Diagnostic Settings** before being sent to a destination such as Log Analytics.

---

# Azure Monitor Metrics Explorer

**Metrics Explorer** allows you to visualize metric data for Azure resources.

Example:

```text
VM
 │
 ▼
Metrics Explorer
 │
 ├── CPU Percentage
 ├── Network In
 └── Network Out
```

You can use metrics to identify:

- Performance problems
- Resource utilization
- Traffic patterns
- Capacity issues

---

# Azure Monitor Logs

Azure Monitor Logs provides a centralized way to collect and analyze log data.

Logs can be stored in a **Log Analytics workspace**.

```text
Azure Resources
      │
      ▼
Log Data
      │
      ▼
Log Analytics Workspace
      │
      ▼
KQL
```

Log Analytics and KQL are covered in detail in **15.3**.

---

# Azure Monitor Alerts

Azure Monitor can generate alerts when a monitoring condition is met.

Example:

```text
VM CPU > 80%
      │
      ▼
Alert Rule
      │
      ▼
Action Group
      │
      ▼
Email Notification
```

Alerts can be based on monitoring data such as:

- Metrics
- Logs
- Activity Log

Alert configuration is covered in **15.6**.

---

# Azure Monitor Insights

**Azure Monitor Insights** provides specialized monitoring experiences for supported Azure resources.

Examples:

- VM Insights
- Storage Insights
- Network Insights

```text
Azure Monitor
      │
      └── Insights
           ├── VM Insights
           ├── Storage Insights
           └── Network Insights
```

These provide additional views into resource performance and health.

---

# Azure Monitor vs Activity Log

| Azure Monitor | Activity Log |
|---|---|
| Complete monitoring platform | Subscription-level activity record |
| Metrics and logs | Management operations |
| Alerts | Records administrative changes |
| Insights | Example: resource creation |
| Application monitoring | Example: resource deletion |

Activity Log is one of the data sources that can be used with Azure Monitor.

---

# Azure Monitor vs Log Analytics

| Azure Monitor | Log Analytics |
|---|---|
| Complete monitoring and observability platform | Log analysis capability |
| Metrics, logs, alerts, insights | Primarily log querying and analysis |
| Covers multiple monitoring features | Uses KQL for queries |
| Can generate alerts | Helps investigate log data |

---

# Azure Monitor Use Cases

Azure Monitor can be used to:

- Monitor Azure VM performance
- Monitor storage resources
- Monitor network resources
- Analyze logs
- Monitor applications
- Create performance alerts
- Troubleshoot resource issues
- Track resource activity
- Identify availability problems

---

# Key Points

- Azure Monitor is Azure's monitoring and observability platform.
- It collects and analyzes **metrics and logs**.
- Metrics are numerical measurements used for performance monitoring.
- Logs contain detailed records used for investigation and troubleshooting.
- Activity Log records subscription-level management operations.
- Resource logs provide detailed information from supported Azure resources.
- Log Analytics provides log querying and analysis using KQL.
- Azure Monitor can generate alerts based on monitoring data.
- Insights provide specialized monitoring experiences for supported resources.
- Diagnostic Settings are used to configure the collection and routing of resource logs.
