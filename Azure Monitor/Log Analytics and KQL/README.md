# 15.3 Log Analytics and KQL

## What is Log Analytics?

**Azure Log Analytics** is a service used to collect, store, query, and analyze log data in a **Log Analytics workspace**.

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
      │
      ▼
Query Results
```

---

# Log Analytics Workspace

A **Log Analytics workspace** is a centralized location where monitoring logs are stored and analyzed.

A single workspace can receive logs from multiple Azure resources.

```text
VM ─────────────┐
                │
Storage ────────┤
                ▼
App Service ───► Log Analytics Workspace
                │
                ▼
               KQL
```

---

# Why Use Log Analytics?

Log Analytics helps administrators:

- Search large amounts of log data
- Troubleshoot Azure resources
- Analyze application errors
- Investigate resource activity
- Identify performance problems
- Create log-based alerts
- Build monitoring queries

---

# KQL

**Kusto Query Language (KQL)** is the query language used to analyze data in Azure Monitor Logs.

KQL is designed for:

- Filtering
- Searching
- Sorting
- Selecting columns
- Aggregating data
- Grouping data
- Time-based analysis

Example:

```kql
AzureActivity
| take 10
```

This returns 10 records from the `AzureActivity` table.

---

# KQL Query Structure

A basic KQL query starts with a table and then uses operators.

```kql
TableName
| Operator
| Operator
```

Example:

```kql
AzureActivity
| where ActivityStatus == "Succeeded"
| take 10
```

The pipe character `|` passes the result of one operation to the next operation.

---

# Common KQL Operators

| Operator | Purpose |
|---|---|
| `where` | Filter records |
| `project` | Select columns |
| `extend` | Create calculated columns |
| `summarize` | Aggregate data |
| `sort` | Sort results |
| `take` | Return a limited number of records |
| `top` | Return top records |
| `count` | Count records |
| `distinct` | Return unique values |
| `render` | Create visualizations |

---

# where

The `where` operator filters records based on a condition.

Example:

```kql
AzureActivity
| where ActivityStatus == "Succeeded"
```

Another example:

```kql
AzureActivity
| where ActivityStatus == "Failed"
```

---

# project

The `project` operator selects the columns you want to display.

Example:

```kql
AzureActivity
| project TimeGenerated, OperationNameValue, ActivityStatus
```

This returns only the selected columns.

---

# take

The `take` operator returns a specified number of records.

Example:

```kql
AzureActivity
| take 10
```

Returns 10 records.

---

# sort

The `sort` operator sorts query results.

Example:

```kql
AzureActivity
| sort by TimeGenerated desc
```

This displays the newest records first.

---

# top

The `top` operator returns the top records based on a specified column.

Example:

```kql
AzureActivity
| top 10 by TimeGenerated desc
```

---

# count

The `count` operator counts records.

Example:

```kql
AzureActivity
| count
```

Example result:

```text
Count
-----
1250
```

---

# summarize

The `summarize` operator performs aggregations.

Example:

```kql
AzureActivity
| summarize count()
```

You can also group results.

```kql
AzureActivity
| summarize count() by ActivityStatus
```

Example:

```text
ActivityStatus    Count
--------------    -----
Succeeded         1100
Failed             150
```

---

# distinct

The `distinct` operator returns unique values.

Example:

```kql
AzureActivity
| distinct ResourceGroup
```

This can help identify the resource groups represented in the log data.

---

# extend

The `extend` operator creates a calculated column.

Example:

```kql
AzureActivity
| extend Operation = OperationNameValue
| project TimeGenerated, Operation
```

---

# Time-Based Queries

KQL can filter data based on time.

Example:

```kql
AzureActivity
| where TimeGenerated > ago(1h)
```

This returns activity from the last hour.

Other examples:

```kql
ago(30m)
ago(6h)
ago(1d)
ago(7d)
```

---

# Searching Logs

You can search for specific text using `contains`.

Example:

```kql
AzureActivity
| where OperationNameValue contains "Delete"
```

This can help find deletion-related operations.

---

# Combining Conditions

Multiple conditions can be combined using `and` and `or`.

Example:

```kql
AzureActivity
| where ActivityStatus == "Failed"
| where TimeGenerated > ago(24h)
```

Or:

```kql
AzureActivity
| where ActivityStatus == "Failed"
    or ActivityStatus == "Canceled"
```

---

# Log Tables

Log Analytics stores different types of data in different tables.

Examples include:

```text
AzureActivity
Heartbeat
Perf
Syslog
```

The available tables depend on the data sources configured for the workspace.

---

# AzureActivity

The `AzureActivity` table contains Azure Activity Log data.

Example query:

```kql
AzureActivity
| take 10
```

You can investigate administrative operations such as:

```text
Create
Update
Delete
Start
Stop
```

---

# Heartbeat

The `Heartbeat` table can be used to determine whether monitored agents are reporting data.

Example:

```kql
Heartbeat
| take 10
```

You can use it to investigate whether monitored machines are communicating with Azure Monitor.

---

# Perf

The `Perf` table can contain performance data collected from monitored machines.

Example:

```kql
Perf
| take 10
```

Performance data can include information such as CPU and memory counters depending on the configured data collection.

---

# KQL Query Example: Failed Activities

```kql
AzureActivity
| where ActivityStatus == "Failed"
| project TimeGenerated, OperationNameValue, ResourceGroup
| sort by TimeGenerated desc
```

This query:

1. Filters failed activities.
2. Selects relevant columns.
3. Sorts the results by time.

---

# KQL Query Example: Recent Activities

```kql
AzureActivity
| where TimeGenerated > ago(1h)
| project TimeGenerated, OperationNameValue, ActivityStatus
| sort by TimeGenerated desc
```

This shows recent Azure activity from the last hour.

---

# KQL Query Example: Activity Count

```kql
AzureActivity
| summarize count() by ActivityStatus
```

This groups activities by status and counts them.

---

# KQL Query Example: Activities by Resource Group

```kql
AzureActivity
| summarize count() by ResourceGroup
| sort by count_ desc
```

This shows the number of activities for each resource group.

---

# Log Analytics and Azure Monitor

Log Analytics is part of the Azure Monitor ecosystem.

```text
Azure Monitor
      │
      ├── Metrics
      │
      ├── Logs
      │     │
      │     ▼
      │  Log Analytics
      │     │
      │     ▼
      │    KQL
      │
      ├── Alerts
      └── Insights
```

---

# Practical Lab

## Lab: Query Azure Activity Logs Using Log Analytics and KQL

### Objective

Create a Log Analytics workspace, send Activity Log data to it, and use KQL to analyze Azure activity.

---

## Step 1: Create a Log Analytics Workspace

1. Open **Azure Portal**.
2. Search for **Log Analytics workspaces**.
3. Select **Create**.
4. Select your subscription.
5. Select or create a resource group.
6. Enter a workspace name.

Example:

```text
law-demo
```

7. Select the appropriate region.
8. Select **Review + create**.
9. Select **Create**.

---

## Step 2: Configure Activity Log

1. Open your Azure subscription.
2. Go to **Activity log**.
3. Select **Export Activity Logs**.
4. Create a diagnostic setting.
5. Select **Audit** or the required Activity Log categories.
6. Select **Send to Log Analytics workspace**.
7. Select:

```text
Workspace:
law-demo
```

8. Save the diagnostic setting.

---

## Step 3: Generate Activity

Perform a simple management operation in Azure.

For example:

```text
Create a resource
Update a resource
Delete a test resource
```

This generates Activity Log data.

---

## Step 4: Open Log Analytics

Open:

```text
Log Analytics Workspace
        ↓
Logs
```

Run:

```kql
AzureActivity
| take 10
```

Verify that activity records are returned.

---

## Step 5: Find Failed Operations

Run:

```kql
AzureActivity
| where ActivityStatus == "Failed"
| project TimeGenerated, OperationNameValue, ResourceGroup
| sort by TimeGenerated desc
```

Review any failed operations.

---

## Step 6: Analyze Activity by Status

Run:

```kql
AzureActivity
| summarize count() by ActivityStatus
```

Review the number of successful and failed operations.

---

## Step 7: Analyze Recent Activity

Run:

```kql
AzureActivity
| where TimeGenerated > ago(1h)
| project TimeGenerated, OperationNameValue, ActivityStatus
| sort by TimeGenerated desc
```

Verify the recent activity generated during the lab.

---

# Key Points

- **Log Analytics** is used to store and analyze log data.
- A **Log Analytics workspace** provides centralized log storage and querying.
- **KQL (Kusto Query Language)** is used to query Azure Monitor Logs.
- `where` filters records.
- `project` selects columns.
- `summarize` performs aggregation.
- `sort` orders results.
- `take` limits the number of returned records.
- `count` counts records.
- `distinct` returns unique values.
- `extend` creates calculated columns.
- `ago()` is useful for time-based queries.
- `AzureActivity` contains Azure Activity Log data.
- KQL is useful for troubleshooting, investigation, reporting, and log-based monitoring.
