# 8.12 VMSS Load Balancing & Autoscaling

## What is VMSS Load Balancing & Autoscaling?

A **Virtual Machine Scale Set (VMSS)** can work with Azure Load Balancer and Azure Monitor autoscaling to provide a highly available and scalable application.

```text
                         Internet
                            │
                            ▼
                  Azure Load Balancer
                            │
                            ▼
                       VM Scale Set
                 ┌──────────┼──────────┐
                 │          │          │
               VM-1       VM-2       VM-3
                 │          │          │
                 └──────────┼──────────┘
                            │
                       Application
```

When the workload increases, autoscaling can add VM instances.

```text
High CPU / High Load
        ↓
   Autoscale Rule
        ↓
    Scale Out
        ↓
Add VM Instances
```

When the workload decreases:

```text
Low CPU / Low Load
        ↓
   Autoscale Rule
        ↓
     Scale In
        ↓
Remove VM Instances
```

---

# VMSS Load Balancing

VMSS can be integrated with **Azure Load Balancer** to distribute traffic across VMSS instances.

```text
                    Load Balancer
                          │
                          ▼
                     VM Scale Set
                          │
             ┌────────────┼────────────┐
             │            │            │
           VM-1         VM-2         VM-3
```

The Load Balancer uses health probes to determine which instances are available.

---

## Health Probes

A health probe checks whether a VMSS instance is healthy.

Example:

```text
Load Balancer
      │
      │ Health Probe
      ▼
 ┌────┼────┐
 │    │    │
VM-1 VM-2 VM-3
 ✓    ✓    ✗
```

Traffic is sent only to healthy instances.

---

# Autoscaling

**Autoscaling** automatically changes the number of VMSS instances based on defined rules and metrics.

```text
Azure Monitor
      │
      ▼
Autoscale
      │
 ┌────┴────┐
 │         │
Scale Out Scale In
```

Autoscaling can help:

- Handle increased traffic
- Maintain application performance
- Reduce unnecessary VM instances
- Control compute costs

---

# Scale Out

**Scale out** means increasing the number of VMSS instances.

Example:

```text
Before:

VMSS
 ├── VM-1
 └── VM-2

        ↓ Scale Out

VMSS
 ├── VM-1
 ├── VM-2
 ├── VM-3
 └── VM-4
```

A scale-out rule can be triggered when a metric reaches a defined threshold.

Example:

```text
Average CPU > 70%
        ↓
Add 2 VM instances
```

---

# Scale In

**Scale in** means decreasing the number of VMSS instances.

Example:

```text
Before:

VMSS
 ├── VM-1
 ├── VM-2
 ├── VM-3
 └── VM-4

        ↓ Scale In

VMSS
 ├── VM-1
 └── VM-2
```

Example rule:

```text
Average CPU < 30%
        ↓
Remove 1 VM instance
```

---

# Minimum and Maximum Instances

Autoscaling can define:

- Minimum instance count
- Maximum instance count
- Default instance count

Example:

```text
Minimum: 2
Default: 2
Maximum: 5
```

This means:

```text
Minimum instances → 2
Maximum instances → 5
```

The VMSS can scale between these limits according to the autoscale rules.

---

# Autoscale Metrics

Autoscaling can use metrics to determine when to scale.

A common metric is:

```text
Percentage CPU
```

Example:

```text
CPU > 70%
    ↓
Scale Out

CPU < 30%
    ↓
Scale In
```

Other supported metrics can also be used depending on the workload and autoscaling configuration.

---

# Autoscale Rules

An autoscale rule contains conditions that determine when scaling should occur.

Example:

```text
Condition:
Average CPU > 70%

Action:
Increase instance count by 1
```

Another rule:

```text
Condition:
Average CPU < 30%

Action:
Decrease instance count by 1
```

---

# Cooldown

A **cooldown period** prevents autoscaling from reacting too quickly to temporary changes.

Example:

```text
CPU > 70%
    ↓
Scale Out
    ↓
Cooldown
    ↓
Wait before evaluating another scaling action
```

This helps avoid unnecessary repeated scaling actions.

---

# Azure Monitor

**Azure Monitor** provides the metrics used to evaluate VMSS performance and can work with autoscale.

Example:

```text
VMSS
 │
 │ Metrics
 ▼
Azure Monitor
 │
 ▼
Autoscale
 │
 ├── Scale Out
 └── Scale In
```

Azure Monitor is covered in detail in the later **Azure Monitor** section.

---

# Alerts

An **Azure Monitor alert** can notify administrators when a defined condition occurs.

Example:

```text
VMSS CPU
   │
   ▼
CPU > 80%
   │
   ▼
Alert Rule
   │
   ▼
Action Group
```

Alerts and autoscaling are related but serve different purposes.

### Autoscaling

Automatically changes VMSS capacity.

```text
High CPU
   ↓
Add VM
```

### Alert

Notifies or triggers an action when a condition occurs.

```text
High CPU
   ↓
Alert
   ↓
Notification
```

---

# Action Groups

An **Action Group** defines what should happen when an Azure Monitor alert is triggered.

Actions can include:

- Email notification
- SMS
- Push notification
- Voice call
- Other supported notification or automation actions

Example:

```text
Metric
  ↓
Alert Rule
  ↓
Action Group
  ↓
Email Notification
```

---

# Email Notification

An Action Group can send an email notification when an alert is triggered.

Example:

```text
VMSS CPU > 80%
       ↓
   Alert Rule
       ↓
  Action Group
       ↓
 Email Notification
```

This allows administrators to be notified when the VMSS experiences a defined condition.

---

# Autoscaling vs Alerts

| Autoscaling | Alerts |
|---|---|
| Changes VMSS capacity | Notifies about a condition |
| Adds or removes instances | Sends notifications/actions |
| Used for workload management | Used for monitoring |
| Example: CPU > 70% → scale out | Example: CPU > 80% → send email |

Both can use Azure Monitor metrics.

---

# Complete Architecture

```text
                         Internet
                            │
                            ▼
                  Azure Load Balancer
                            │
                            ▼
                       VM Scale Set
                 ┌──────────┼──────────┐
                 │          │          │
               VM-1       VM-2       VM-3
                 │          │          │
                 └──────────┼──────────┘
                            │
                         Metrics
                            │
                            ▼
                       Azure Monitor
                            │
                     ┌──────┴──────┐
                     │             │
                 Autoscale        Alert
                     │             │
              ┌──────┴──────┐      ▼
              │             │  Action Group
          Scale Out       Scale In   │
                                      ▼
                                   Email
```

---

# Practical Lab

## Lab: VMSS Load Balancing, Autoscaling and Email Alert

### Objective

Create a complete VMSS environment with:

- VM Scale Set
- Azure Load Balancer
- Health probe
- Autoscaling
- Azure Monitor alert
- Action Group
- Email notification

### Architecture

```text
                         Internet
                            │
                            ▼
                  Azure Load Balancer
                            │
                            ▼
                       VM Scale Set
                 ┌──────────┼──────────┐
                 │          │          │
               VM-1       VM-2       VM-3
                            │
                            ▼
                       Azure Monitor
                       /            \
                 Autoscale          Alert
                    │                 │
              Scale Out/In       Action Group
                                      │
                                      ▼
                                    Email
```

---

## Step 1: Create a VM Scale Set

1. Open **Azure Portal**.
2. Search for **Virtual Machine Scale Sets**.
3. Select **Create**.
4. Select the subscription.
5. Select or create a resource group.
6. Enter a VMSS name.
7. Select the required region.
8. Select the appropriate orchestration mode.
9. Choose an Ubuntu image.
10. Select a suitable VM size.
11. Configure authentication.
12. Set the initial instance count to `2`.

---

## Step 2: Configure Networking

1. Select or create a VNet.
2. Select a subnet.
3. Configure the required network settings.
4. Configure the Load Balancer integration if available during VMSS creation.

Example:

```text
VNet
 └── Subnet
      └── VMSS
```

---

## Step 3: Configure Load Balancer

If the Load Balancer is created as part of the VMSS deployment, verify:

```text
Frontend IP
Backend Pool
Health Probe
Load Balancing Rule
```

Example:

```text
Load Balancer
      │
      ▼
Backend Pool
      │
      ▼
VMSS Instances
```

---

## Step 4: Deploy the VMSS

1. Review the configuration.
2. Select **Create**.
3. Wait for the deployment to complete.
4. Open the VMSS.
5. Go to **Instances**.
6. Verify that the initial instances are running.

Example:

```text
Instance Count: 2

VMSS
 ├── Instance 1
 └── Instance 2
```

---

## Step 5: Configure Autoscaling

1. Open the VM Scale Set.
2. Go to **Scaling**.
3. Enable **Custom autoscale**.
4. Set:

```text
Minimum: 2
Default: 2
Maximum: 5
```

5. Add a scale-out rule.

Example:

```text
Metric:
Percentage CPU

Condition:
Greater than 70%

Action:
Increase count by 1
```

6. Add a scale-in rule.

Example:

```text
Metric:
Percentage CPU

Condition:
Less than 30%

Action:
Decrease count by 1
```

7. Configure an appropriate cooldown period.
8. Save the autoscale settings.

---

## Step 6: Create an Action Group

1. Open **Azure Monitor**.
2. Select **Alerts**.
3. Select **Action groups**.
4. Select **Create**.
5. Select the subscription.
6. Select the resource group.
7. Enter an Action Group name.
8. Add an email notification.
9. Enter the email address where notifications should be received.
10. Create the Action Group.

Example:

```text
Action Group
     │
     └── Email
          │
          ▼
     Administrator
```

---

## Step 7: Create an Azure Monitor Alert

1. Open **Azure Monitor**.
2. Go to **Alerts**.
3. Create a new alert rule.
4. Select the VMSS as the resource.
5. Select a metric such as:

```text
Percentage CPU
```

6. Configure a threshold.

Example:

```text
CPU > 80%
```

7. Select the Action Group created earlier.
8. Configure the alert details.
9. Create the alert rule.

---

## Step 8: Test Autoscaling

Generate workload on the VMSS instances.

For example, on a Linux VM, a CPU stress tool can be used for testing if available.

Monitor the CPU metric in Azure Monitor.

```text
CPU Usage
    ↓
Above Threshold
    ↓
Autoscale Rule
    ↓
Scale Out
    ↓
New VMSS Instance
```

Verify the VMSS instance count.

Example:

```text
Before:

2 Instances

      ↓

After Scale Out:

3 Instances
```

---

## Step 9: Test Scale In

Allow the workload to decrease.

When the scale-in condition is met:

```text
Low CPU
   ↓
Autoscale Rule
   ↓
Scale In
   ↓
Instance Removed
```

Verify that the VMSS returns toward the configured minimum capacity.

---

## Step 10: Test Alert and Email

Trigger the configured alert condition.

Example:

```text
CPU > 80%
     ↓
Alert Rule
     ↓
Action Group
     ↓
Email
```

Check the configured email inbox for the Azure Monitor alert notification.

---

# Key Points

- VMSS can be integrated with Azure Load Balancer.
- Load Balancer distributes traffic across healthy VMSS instances.
- Health probes determine whether instances are healthy.
- Autoscaling automatically changes VMSS capacity.
- **Scale out** adds VM instances.
- **Scale in** removes VM instances.
- Autoscaling can use Azure Monitor metrics such as CPU utilization.
- Minimum and maximum instance counts control the scaling boundaries.
- Cooldown periods help prevent rapid repeated scaling actions.
- Azure Monitor alerts notify administrators when defined conditions occur.
- Action Groups define the notification or action taken by an alert.
- Email notifications can be configured through Action Groups.
- Autoscaling changes capacity automatically, while alerts primarily notify administrators.
