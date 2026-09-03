# 15.7 Alert Processing Rules

## What are Alert Processing Rules?

**Alert Processing Rules** allow you to modify or suppress notifications from Azure Monitor alerts based on specific conditions.

They are useful when you want to control **when and how alert notifications are delivered** without changing the underlying alert rule.

```text
Alert Rule
    │
    ▼
Alert Fired
    │
    ▼
Alert Processing Rule
    │
    ├── Suppress Notifications
    └── Modify Action Groups
            │
            ▼
        Notification
```

---

# Why Use Alert Processing Rules?

Alert Processing Rules can be used to:

- Suppress notifications during planned maintenance
- Prevent unnecessary notifications
- Modify which Action Groups are triggered
- Apply notification behavior to multiple alerts
- Control alert notifications based on conditions

---

# Alert Rule vs Alert Processing Rule

These two concepts are different.

| Alert Rule | Alert Processing Rule |
|---|---|
| Defines when an alert should fire | Controls what happens after an alert fires |
| Monitors a condition | Processes the alert notification |
| Example: CPU > 80% | Example: Suppress email during maintenance |
| Detects a problem | Controls notification behavior |

Simple way to remember:

```text
Alert Rule
"What condition should trigger an alert?"

Alert Processing Rule
"What should happen with the notification?"
```

---

# Suppress Notifications

One of the main uses of an Alert Processing Rule is to temporarily suppress notifications.

Example:

```text
VM CPU > 80%
      │
      ▼
Alert Fired
      │
      ▼
Alert Processing Rule
      │
      ▼
Notification Suppressed
```

The alert can still be generated, but notifications can be suppressed according to the processing rule.

---

# Planned Maintenance Example

Suppose a VM will be under maintenance from:

```text
10:00 PM → 11:00 PM
```

You may not want to receive alerts during this period.

```text
Normal Time
     │
     ▼
Alert → Email ✅

Maintenance Window
     │
     ▼
Alert → Notification Suppressed

After Maintenance
     │
     ▼
Alert → Email ✅
```

This helps avoid unnecessary alert notifications during expected maintenance activities.

---

# Modify Action Groups

Alert Processing Rules can also modify the Action Groups associated with alerts.

Example:

```text
Alert
  │
  ▼
Alert Processing Rule
  │
  ▼
Different Action Group
  │
  ▼
Operations Team
```

This allows notification behavior to be changed without modifying the original alert rule.

---

# Scope

An Alert Processing Rule can be applied to a defined scope.

The scope determines which alerts the rule applies to.

Example:

```text
Subscription
     │
     └── Alert Processing Rule
             │
             ├── VM Alerts
             ├── Storage Alerts
             └── Network Alerts
```

The exact scope and filtering options depend on the alert processing rule configuration.

---

# Filters

Filters can be used to control which alerts the processing rule affects.

Examples include filtering based on:

- Alert severity
- Alert rule
- Resource
- Resource type
- Alert context

Example:

```text
All Alerts
    │
    ▼
Filter:
Severity = Sev 2
    │
    ▼
Processing Rule
```

Only matching alerts are processed by the rule.

---

# Alert Processing Flow

```text
Monitoring Data
      │
      ▼
Alert Rule
      │
      ▼
Condition Met
      │
      ▼
Alert Fired
      │
      ▼
Alert Processing Rule
      │
      ├── Suppress Notification
      │
      └── Modify Action Group
                  │
                  ▼
             Notification
```

---

# Alert Processing Rules and Action Groups

Alert Processing Rules work together with Action Groups.

```text
Alert Rule
    │
    ▼
Alert
    │
    ▼
Alert Processing Rule
    │
    ▼
Action Group
    │
    ▼
Email / SMS / Other Action
```

The Alert Processing Rule controls how the alert notification should be processed before the Action Group is used.

---

# Practical Example

Consider a production VM with this alert:

```text
CPU > 80%
```

Action Group:

```text
Email → Administrator
```

During planned maintenance, create an Alert Processing Rule:

```text
Scope:
Production VM

Condition:
Maintenance period

Action:
Suppress notifications
```

The alert can still be generated, but the administrator does not receive unnecessary notifications during the maintenance window.

---

# Practical Lab

No separate lab is required for this topic.

Alert Processing Rules can be understood as an extension of the **Azure Monitor Alerts and Action Groups** configuration covered in **15.6**.

The relationship is:

```text
Alert Rule
    ↓
Alert Processing Rule
    ↓
Action Group
    ↓
Notification
```

---

# Key Points

- Alert Processing Rules control how Azure Monitor alert notifications are handled.
- They operate after an alert is generated.
- They can suppress notifications.
- They can modify Action Groups associated with alerts.
- They can be useful during planned maintenance.
- Filters can limit which alerts a processing rule affects.
- Alert Processing Rules do not replace Alert Rules.
- Alert Rules detect conditions.
- Alert Processing Rules control notification behavior.
- Action Groups define the actual notifications or actions.
