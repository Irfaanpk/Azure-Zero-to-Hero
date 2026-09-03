# 15.6 Azure Monitor Alerts and Action Groups

## What are Azure Monitor Alerts?

**Azure Monitor Alerts** proactively notify you when an important condition is detected in your Azure resources.

An alert rule monitors a specific signal and triggers when the configured condition is met.

```text
Azure Resource
      │
      ▼
Monitoring Data
      │
      ▼
   Alert Rule
      │
      │ Condition Met
      ▼
     Alert
      │
      ▼
 Action Group
      │
      ▼
Notification / Action
```

---

# Why Use Alerts?

Azure Monitor alerts help you:

- Detect resource problems
- Monitor performance
- Identify failures
- Respond to availability issues
- Receive notifications
- Trigger automated actions

Example:

```text
VM CPU > 80%
      │
      ▼
Alert Triggered
      │
      ▼
Email Notification
```

---

# Alert Rule

An **alert rule** defines when Azure Monitor should generate an alert.

An alert rule generally contains:

```text
Alert Rule
    │
    ├── Scope
    ├── Signal
    ├── Condition
    ├── Evaluation
    └── Action Group
```

---

# Alert Scope

The **scope** defines which resource or resources the alert monitors.

Example:

```text
Scope
  ↓
Virtual Machine
  ↓
CPU Percentage
```

An alert can be configured for an individual resource or supported groups of resources depending on the alert type.

---

# Alert Signals

A **signal** is the monitoring data used by an alert rule.

Common signal types include:

- Metrics
- Logs
- Activity Log

Example:

```text
Signal:
Percentage CPU
```

---

# Metric Alerts

A **metric alert** triggers when a metric meets a configured condition.

Example:

```text
Metric:
Percentage CPU

Condition:
Greater than 80%

Duration:
5 minutes
```

Flow:

```text
VM CPU
  │
  ▼
85%
  │
  ▼
Condition: > 80%
  │
  ▼
Alert
```

---

# Log Search Alerts

A **log search alert** uses a query against Azure Monitor Logs to determine whether an alert condition is met.

Example:

```text
Log Data
   │
   ▼
KQL Query
   │
   ▼
Condition
   │
   ▼
Alert
```

Example query:

```kql
AzureActivity
| where ActivityStatus == "Failed"
| count
```

The alert can be configured based on the query result.

---

# Activity Log Alerts

Activity Log alerts monitor subscription-level management operations.

Examples:

```text
Delete Resource
Create Resource
Modify Resource
Role Assignment
```

Example:

```text
Resource Deleted
      │
      ▼
Activity Log
      │
      ▼
Activity Log Alert
      │
      ▼
Notification
```

---

# Alert Conditions

An alert condition defines when an alert should be triggered.

Example:

```text
Metric:
Percentage CPU

Operator:
Greater than

Threshold:
80%

Evaluation:
5 minutes
```

When the condition is satisfied, the alert is triggered.

---

# Alert States

Azure Monitor alerts can have states that indicate their current condition.

Common states include:

| State | Meaning |
|---|---|
| Fired | Alert condition has been met |
| Resolved | Alert condition is no longer met |

Example:

```text
CPU > 80%
    ↓
Fired

CPU < 80%
    ↓
Resolved
```

---

# Action Groups

An **Action Group** defines what should happen when an alert is triggered.

```text
Alert
  │
  ▼
Action Group
  │
  ├── Email
  ├── SMS
  ├── Push Notification
  ├── Voice
  └── Automation / Webhook
```

The available action types depend on the configuration and Azure service capabilities.

---

# Why Use Action Groups?

Action Groups allow the same notification or action configuration to be reused by multiple alert rules.

Example:

```text
Alert 1 ─────┐
             │
Alert 2 ─────┼──► Action Group ──► Email
             │
Alert 3 ─────┘
```

Instead of configuring email recipients separately for every alert, you can reuse an Action Group.

---

# Common Action Group Actions

Examples include:

### Email

```text
Alert
  ↓
Action Group
  ↓
Email
```

### SMS

```text
Alert
  ↓
Action Group
  ↓
SMS
```

### Push Notification

```text
Alert
  ↓
Action Group
  ↓
Mobile Notification
```

### Automation

Action Groups can also integrate with supported automation and external notification mechanisms.

---

# Alert Rule vs Action Group

These are different concepts.

| Alert Rule | Action Group |
|---|---|
| Defines what condition to monitor | Defines what action to perform |
| Contains scope and condition | Contains notification/action configuration |
| Detects the problem | Responds to the alert |
| Example: CPU > 80% | Example: Send email |

Simple way to remember:

```text
Alert Rule
"What happened?"

Action Group
"What should we do about it?"
```

---

# Alert Severity

Alerts can be assigned different severity levels to indicate their importance.

| Severity | Meaning |
|---|---|
| Sev 0 | Critical |
| Sev 1 | Error |
| Sev 2 | Warning |
| Sev 3 | Informational |
| Sev 4 | Verbose |

Severity helps prioritize alerts.

---

# Alert Evaluation

Azure Monitor evaluates alert conditions based on the configured evaluation frequency and time window.

Example:

```text
CPU > 80%
      │
      ├── 10:00 → 82%
      ├── 10:01 → 85%
      ├── 10:02 → 88%
      ├── 10:03 → 90%
      └── 10:04 → 91%
                 │
                 ▼
              Alert
```

The exact behavior depends on the alert rule configuration.

---

# Alert Lifecycle

A simplified alert lifecycle is:

```text
Monitoring Data
      │
      ▼
Condition Evaluated
      │
      ├── Condition False
      │       ↓
      │    No Alert
      │
      └── Condition True
              ↓
         Alert Fired
              ↓
         Action Group
              ↓
        Notification
              │
              ▼
      Condition Clears
              │
              ▼
        Alert Resolved
```

---

# Alerts and Action Groups in VMSS

In the **VMSS Load Balancing & Autoscaling project** from Section 8.12, Azure Monitor alerts and Action Groups are used to notify administrators when monitoring conditions are met.

```text
VM Scale Set
      │
      ▼
Azure Monitor
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

The practical implementation is covered in that project rather than creating another duplicate lab here.

---

# Practical Lab

No separate lab is required for this topic because **alerts and Action Groups are already implemented practically in Section 8.12 — VMSS Load Balancing & Autoscaling**.

The concepts covered here should be applied there:

```text
VMSS
 │
 ▼
CPU Monitoring
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

---

# Key Points

- Azure Monitor Alerts detect important conditions in monitoring data.
- Alert rules define the scope, signal, condition, and evaluation configuration.
- Alerts can be based on metrics, logs, and Activity Log events.
- Metric alerts monitor numerical resource metrics.
- Log search alerts use KQL query results.
- Activity Log alerts monitor subscription-level management operations.
- Action Groups define notifications and actions when an alert fires.
- The same Action Group can be reused by multiple alert rules.
- Alert rules detect conditions; Action Groups respond to those conditions.
- Alerts can move between fired and resolved states.
- Alert severity helps prioritize monitoring events.
- Section 8.12 already provides the practical implementation of alerts and Action Groups.
