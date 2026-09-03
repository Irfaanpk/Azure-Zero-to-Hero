# 8.12 VMSS Load Balancing & Autoscaling

## Project Overview

In this project, we will build a highly available and scalable web application using:

- Azure Virtual Machine Scale Set
- Azure Load Balancer
- Health Probe
- Autoscaling
- Azure Monitor
- Alert Rule
- Action Group
- Email Notification

The project will demonstrate how multiple VMSS instances can serve application traffic and automatically scale based on workload.

---

# Project Architecture

```text
                         Internet
                            │
                            ▼
                  Azure Load Balancer
                            │
                            ▼
                       VM Scale Set
                  ┌─────────┼─────────┐
                  │         │         │
                VM-1      VM-2      VM-3
                  │         │         │
                  └─────────┼─────────┘
                            │
                       Azure Monitor
                       /            \
                      ↓              ↓
                 Autoscaling        Alert
                  /      \            │
                 ↓        ↓           ↓
            Scale Out  Scale In   Action Group
                                      │
                                      ▼
                               Email Notification
```

---

# Project Requirements

Before starting, make sure you have:

- An active Azure subscription
- Access to Azure Portal
- Permission to create Azure resources
- A valid email address for alert notifications

---

# Step 1: Create a Resource Group

1. Open **Azure Portal**.
2. Search for **Resource Groups**.
3. Select **Create**.
4. Select your subscription.
5. Create a resource group.

Example:

```text
Resource Group:
rg-vmss-project
```

6. Select the required region.
7. Select **Review + create**.
8. Select **Create**.

---

# Step 2: Create a VM Scale Set

1. Search for **Virtual Machine Scale Sets**.
2. Select **Create**.
3. Select the subscription.
4. Select the resource group:

```text
rg-vmss-project
```

5. Enter a VMSS name.

Example:

```text
vmss-web-project
```

6. Select the required region.
7. Select the appropriate orchestration mode.
8. Select an Ubuntu image.
9. Select a suitable VM size.
10. Configure SSH authentication.
11. Set the initial instance count:

```text
2
```

---

# Step 3: Configure Networking

Configure:

```text
Virtual Network
      ↓
Subnet
      ↓
VM Scale Set
```

During VMSS creation:

1. Create or select a VNet.
2. Select a subnet.
3. Configure the required networking options.
4. Configure the Load Balancer integration.

---

# Step 4: Configure Azure Load Balancer

Create or configure a **Standard Public Load Balancer** for the VMSS.

The architecture should be:

```text
Internet
    │
    ▼
Public IP
    │
    ▼
Azure Load Balancer
    │
    ▼
VMSS Backend Pool
```

Configure:

- Frontend public IP
- Backend pool
- Health probe
- Load balancing rule

---

# Step 5: Configure Health Probe

Create an HTTP health probe.

Example:

```text
Protocol: HTTP
Port: 80
Path: /
```

The health probe checks whether the VMSS instances are available.

```text
Load Balancer
      │
      ▼
Health Probe
      │
 ┌────┼────┐
 │    │    │
VM-1 VM-2 VM-3
 ✓    ✓    ✓
```

---

# Step 6: Configure Load Balancing Rule

Create an HTTP load balancing rule.

Example:

```text
Protocol: TCP
Frontend Port: 80
Backend Port: 80
```

Associate:

```text
Frontend IP
     ↓
Backend Pool
     ↓
Health Probe
```

---

# Step 7: Install a Web Server on VMSS Instances

Connect to one of the VMSS instances using SSH.

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Start Nginx:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

---

# Step 8: Create a Test Web Page

Edit the default Nginx page:

```bash
sudo nano /var/www/html/index.html
```

Add:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Azure VMSS Project</title>
</head>
<body>
    <h1>Azure VMSS Load Balancing Project</h1>
    <p>Application is running successfully.</p>
</body>
</html>
```

Save the file.

Make sure the same web server configuration is available on the VMSS instances.

---

# Step 9: Test Load Balancing

Copy the **Load Balancer public IP address**.

Open:

```text
http://LOAD_BALANCER_PUBLIC_IP
```

The request should reach one of the healthy VMSS instances.

```text
Client
  ↓
Load Balancer
  ↓
VMSS
  ├── VM-1
  └── VM-2
```

---

# Step 10: Configure Autoscaling

Open the VM Scale Set.

Go to:

```text
Scaling
```

Select:

```text
Custom autoscale
```

Configure:

```text
Minimum instances: 2
Default instances: 2
Maximum instances: 5
```

---

# Step 11: Configure Scale-Out Rule

Create a scale-out rule using CPU utilization.

Example:

```text
Metric:
Percentage CPU

Condition:
Greater than 70%

Action:
Increase instance count by 1
```

Architecture:

```text
CPU > 70%
     ↓
Scale Out
     ↓
Add VM Instance
```

---

# Step 12: Configure Scale-In Rule

Create a scale-in rule.

Example:

```text
Metric:
Percentage CPU

Condition:
Less than 30%

Action:
Decrease instance count by 1
```

Architecture:

```text
CPU < 30%
     ↓
Scale In
     ↓
Remove VM Instance
```

---

# Step 13: Configure Cooldown

Configure an appropriate cooldown period between scaling actions.

Example:

```text
Scale Action
     ↓
Cooldown Period
     ↓
Evaluate Again
```

This prevents the autoscale system from reacting too quickly to temporary workload changes.

---

# Step 14: Create an Azure Monitor Alert

Open:

```text
Azure Monitor
    ↓
Alerts
    ↓
Create
    ↓
Alert Rule
```

Select the VMSS as the resource.

Choose a metric such as:

```text
Percentage CPU
```

Configure an alert condition.

Example:

```text
CPU > 80%
```

---

# Step 15: Create an Action Group

Create an Action Group for the alert.

Go to:

```text
Azure Monitor
    ↓
Alerts
    ↓
Action Groups
```

Select **Create**.

Configure:

```text
Action Group Name:
vmss-email-action

Notification Type:
Email

Email Address:
your-email@example.com
```

Save the Action Group.

---

# Step 16: Connect Action Group to Alert

Return to the alert rule.

Under **Actions**, select the Action Group:

```text
vmss-email-action
```

The flow becomes:

```text
CPU > 80%
     ↓
Alert Rule
     ↓
Action Group
     ↓
Email
```

---

# Step 17: Generate CPU Load

Connect to a VMSS instance using SSH.

Install a CPU stress utility if required:

```bash
sudo apt update
sudo apt install stress -y
```

Generate CPU load:

```bash
stress --cpu 2 --timeout 300
```

This generates CPU activity for testing.

> Use a suitable number of CPU workers for the VM size you selected.

---

# Step 18: Verify Autoscaling

Monitor the VMSS instances.

Initially:

```text
VMSS
 ├── VM-1
 └── VM-2
```

When the configured CPU threshold is reached:

```text
High CPU
   ↓
Autoscale Rule
   ↓
Scale Out
   ↓
VMSS
 ├── VM-1
 ├── VM-2
 └── VM-3
```

Verify that the instance count increases.

---

# Step 19: Verify Load Balancing

After the new VMSS instance is created:

```text
Load Balancer
      ↓
Backend Pool
      ↓
VM-1
VM-2
VM-3
```

Verify that the new instance becomes healthy through the health probe.

Access:

```text
http://LOAD_BALANCER_PUBLIC_IP
```

The application should remain available.

---

# Step 20: Verify Scale-In

Stop the CPU workload and allow CPU utilization to decrease.

When the scale-in condition is met:

```text
Low CPU
   ↓
Autoscale Rule
   ↓
Scale In
   ↓
VMSS Instance Removed
```

Verify that the VMSS instance count decreases toward the configured minimum.

---

# Step 21: Verify Azure Monitor Alert

Trigger the configured alert condition.

Example:

```text
CPU > 80%
     ↓
Alert Rule Triggered
     ↓
Action Group
     ↓
Email Notification
```

Check the configured email inbox.

You should receive an Azure Monitor alert notification.

---

# Step 22: Review Autoscale Activity

Open the VM Scale Set and review the scaling activity.

Verify:

```text
Scale Out
    ↓
New Instance Created

Scale In
    ↓
Instance Removed
```

Review the autoscale activity and Azure Monitor metrics to understand when the scaling actions occurred.

---

# Final Project Flow

```text
                    Internet
                       │
                       ▼
              Azure Load Balancer
                       │
                       ▼
                  Health Probe
                       │
                       ▼
                  VM Scale Set
             ┌─────────┼─────────┐
             │         │         │
           VM-1      VM-2      VM-3
             │         │         │
             └─────────┼─────────┘
                       │
                       ▼
                 Azure Monitor
                       │
                ┌──────┴──────┐
                │             │
            Autoscale        Alert
                │             │
          ┌─────┴─────┐       ▼
          │           │   Action Group
      Scale Out    Scale In     │
                                ▼
                         Email Notification
```

---

# What You Learned

By completing this project, you practiced:

- Creating a VM Scale Set
- Configuring Azure Load Balancer
- Configuring health probes
- Connecting VMSS to a Load Balancer
- Configuring autoscaling
- Testing scale-out
- Testing scale-in
- Monitoring VMSS metrics
- Creating Azure Monitor alerts
- Creating Action Groups
- Configuring email notifications
- Verifying the complete VMSS scaling workflow

---

# Cleanup

To avoid unnecessary Azure charges:

1. Open **Resource Groups**.
2. Select:

```text
rg-vmss-project
```

3. Select **Delete resource group**.
4. Confirm the deletion.

This removes the resources created for the project.

---

# Key Project Architecture

```text
Load Balancing
      ↓
Azure Load Balancer
      ↓
VM Scale Set
      ↓
Multiple VM Instances

Autoscaling
      ↓
Azure Monitor
      ↓
Scale Out / Scale In

Monitoring
      ↓
Alert Rule
      ↓
Action Group
      ↓
Email Notification
```
