# 15.9 Application Insights

## What is Application Insights?

**Application Insights** is an Azure Monitor feature used to monitor the performance, availability, and behavior of applications.

It helps developers and administrators identify:

- Application failures
- Slow requests
- Exceptions
- Availability problems
- Dependencies
- Application performance

```text
Application
     │
     ▼
Application Insights
     │
     ▼
Azure Monitor
     │
     ├── Metrics
     ├── Logs
     └── Alerts
```

---

# Why Use Application Insights?

Application Insights helps you:

- Monitor application performance
- Detect application failures
- Track requests
- Monitor response times
- Track exceptions
- Monitor dependencies
- Check application availability
- Troubleshoot application issues

---

# How Application Insights Works

Application Insights collects telemetry from an application and sends it to Azure Monitor.

```text
User
 │
 ▼
Application
 │
 ├── Requests
 ├── Dependencies
 ├── Exceptions
 └── Performance
        │
        ▼
Application Insights
        │
        ▼
Azure Monitor
```

---

# Application Telemetry

**Telemetry** is data collected about application behavior.

Important telemetry types include:

| Telemetry | Purpose |
|---|---|
| Requests | Track incoming application requests |
| Dependencies | Track calls to external services |
| Exceptions | Track application errors |
| Traces | Application diagnostic information |
| Availability | Check whether an application is reachable |
| Performance | Analyze application response and execution behavior |

---

# Requests

Application Insights can track requests received by an application.

Example:

```text
User
 │
 ▼
Web Application
 │
 ▼
Request
 │
 ├── Response Time
 ├── Success / Failure
 └── Result
```

This helps identify slow or failed requests.

---

# Dependencies

Application Insights can track dependencies used by an application.

Examples:

- Databases
- HTTP services
- Azure services
- External APIs

Example:

```text
Web Application
      │
      ├── SQL Database
      ├── REST API
      └── Storage Account
```

This helps identify whether a dependency is causing application performance problems.

---

# Exceptions

Application Insights can collect application exceptions.

Example:

```text
Application
     │
     ▼
Exception
     │
     ▼
Application Insights
     │
     ▼
Exception Details
```

This helps administrators identify application failures.

---

# Application Performance

Application Insights provides information that can help identify slow application operations.

Example:

```text
Request
  │
  ├── Application Processing
  │
  └── Dependency Calls
          │
          ▼
      Response Time
```

You can investigate which part of the application is contributing to slow responses.

---

# Availability Monitoring

Application Insights can monitor application availability using availability tests.

Example:

```text
Availability Test
       │
       ▼
Application URL
       │
       ├── Available ✅
       └── Unavailable ❌
```

This helps determine whether an application is reachable from configured test locations.

---

# Application Insights and Azure Monitor

Application Insights is part of the Azure Monitor ecosystem.

```text
                    Azure Monitor
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Metrics          Logs         Application
                                       Insights
                                          │
                         ┌────────────────┼───────────────┐
                         ▼                ▼               ▼
                      Requests       Exceptions      Dependencies
```

---

# Application Insights vs Azure Monitor

| Application Insights | Azure Monitor |
|---|---|
| Application-focused monitoring | Complete Azure monitoring platform |
| Requests | Metrics |
| Exceptions | Logs |
| Dependencies | Alerts |
| Application performance | Insights |
| Availability tests | Resource monitoring |

Application Insights is a **feature within Azure Monitor**, not a completely separate monitoring platform.

---

# Application Insights vs Log Analytics

| Application Insights | Log Analytics |
|---|---|
| Application monitoring | Log storage and analysis |
| Collects application telemetry | Queries monitoring data |
| Requests, exceptions, dependencies | KQL-based analysis |
| Application performance | Detailed log investigation |

They can work together:

```text
Application
     │
     ▼
Application Insights
     │
     ▼
Azure Monitor
     │
     ▼
Log Analytics
     │
     ▼
KQL
```

---

# Application Insights and Alerts

Application Insights data can be used with Azure Monitor alerts.

Example:

```text
Application
     │
     ▼
Application Insights
     │
     ▼
High Failure Rate
     │
     ▼
Azure Monitor Alert
     │
     ▼
Action Group
     │
     ▼
Email Notification
```

The general alert and Action Group concepts are covered in **15.6**.

---

# Practical Lab

## Lab: Monitor an Azure Web Application with Application Insights

### Objective

Enable Application Insights for an Azure App Service and view application requests, performance, and failures.

---

## Step 1: Open an App Service

1. Open **Azure Portal**.
2. Go to **App Services**.
3. Select an existing test Web App.

---

## Step 2: Open Application Insights

From the App Service menu:

```text
Monitoring
    ↓
Application Insights
```

Select the option to enable or configure Application Insights.

---

## Step 3: Configure Application Insights

Create or select an Application Insights resource.

Example:

```text
Application Insights:
appinsights-demo
```

Select the appropriate workspace and configuration options.

Save the configuration.

---

## Step 4: Generate Application Traffic

Open the Web App URL in a browser.

```text
https://<your-app>.azurewebsites.net
```

Refresh the application several times.

Generate normal application activity.

---

## Step 5: View Application Insights

Open:

```text
Application Insights
      ↓
Overview
```

Review the available application telemetry.

Look for:

```text
Requests
Failures
Response Time
Availability
Dependencies
```

---

## Step 6: View Application Failures

Open the application failure information and review any available exceptions or failed requests.

```text
Application
     ↓
Failed Request
     ↓
Application Insights
     ↓
Failure Details
```

---

## Step 7: Check Availability

Configure an availability test for the application's URL if supported by the current Application Insights experience.

Verify whether the application is reachable.

```text
Availability Test
       ↓
Application
       ↓
Available / Failed
```

---

# Key Points

- Application Insights is an **Azure Monitor application monitoring feature**.
- It monitors application performance and behavior.
- It collects telemetry such as requests, dependencies, exceptions, traces, and availability data.
- Requests help monitor application traffic and response times.
- Dependencies help identify problems with external services and databases.
- Exceptions help identify application failures.
- Availability tests help monitor application availability.
- Application Insights works with Azure Monitor, Log Analytics, and alerts.
- Application Insights is focused on **application-level monitoring**, while Azure Monitor provides broader monitoring across Azure resources.
