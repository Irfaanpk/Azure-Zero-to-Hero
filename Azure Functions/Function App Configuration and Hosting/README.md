# 18.3 Function App Configuration and Hosting

An **Azure Function App** provides the environment where Azure Functions run. Its configuration determines how the application runs, connects to other services, and scales.

---

## Function App Architecture

```text
Function App
     │
     ├── Function 1
     ├── Function 2
     └── Function 3
            │
            ▼
      Hosting Environment
            │
            ├── Runtime
            ├── Application Settings
            ├── Networking
            └── Scaling
```

---

# Function App Configuration

Important Function App configuration areas include:

- Application settings
- Runtime settings
- Environment variables
- Connection settings
- Authentication settings
- Networking
- Hosting plan
- Scaling configuration

---

# Application Settings

**Application settings** store configuration values that your function can access at runtime.

Example:

```text
Application Settings

STORAGE_ACCOUNT
DATABASE_NAME
API_URL
ENVIRONMENT
```

Instead of putting configuration values directly inside the code:

```text
Function Code
     ↓
Application Settings
     ↓
Configuration Values
```

This helps separate application configuration from application code.

---

# Environment Variables

Application settings are exposed to the function as **environment variables**.

Example:

```text
Azure Portal
     ↓
Configuration
     ↓
Application Settings
     ↓
Environment Variables
     ↓
Function Code
```

Example value:

```text
ENVIRONMENT = production
```

The function can read the value at runtime.

---

# Connection Settings

Functions often need to communicate with Azure services such as:

- Azure Storage
- Azure Service Bus
- Azure SQL
- Other supported services

Connection information can be stored in application settings instead of hard-coding it in the function code.

Example:

```text
Function
   ↓
Application Setting
   ↓
Connection Configuration
   ↓
Azure Service
```

For production workloads, prefer secure identity-based authentication where supported instead of storing long-lived secrets.

---

# Runtime Configuration

A Function App uses a specific **Functions runtime** and programming language/runtime stack.

Examples include:

- .NET
- Java
- JavaScript / Node.js
- Python
- PowerShell

The selected runtime determines which programming model and application code can run in the Function App.

---

# Hosting Plans

Azure Functions can run using different hosting options.

The main hosting plans to understand are:

- Flex Consumption
- Consumption
- Premium
- Dedicated
- Container Apps hosting

The available features, scaling behavior, networking capabilities, and pricing differ between hosting options.

---

# Consumption-Based Hosting

Consumption-oriented hosting automatically manages compute resources based on workload.

Conceptually:

```text
Low Workload
     ↓
Fewer Instances

High Workload
     ↓
More Instances
```

This is suitable for workloads where demand changes over time and server management should be minimized.

---

# Flex Consumption

**Flex Consumption** is a newer serverless hosting option designed for flexible scaling and improved configuration options.

Conceptually:

```text
Function App
     ↓
Flex Consumption
     ↓
Automatic Scaling
     ↓
Function Instances
```

It provides more control over scaling and networking than the original Consumption plan.

---

# Premium Plan

The **Premium plan** provides additional capabilities for workloads that need more control or performance.

Common characteristics include:

- Prewarmed instances
- Avoiding cold starts for configured workloads
- More advanced networking capabilities
- Longer-running workloads
- Automatic scaling

Conceptually:

```text
Function App
     ↓
Premium Plan
     ↓
Prewarmed / Scaled Instances
```

---

# Dedicated Plan

Azure Functions can also run on a **Dedicated plan**, using App Service infrastructure.

```text
App Service Plan
       ↓
Function App
       ↓
Functions
```

This can be useful when predictable capacity or existing App Service infrastructure is required.

---

# Scaling

Azure Functions can scale based on workload and the selected hosting plan.

Example:

```text
100 Requests
      ↓
Function Instance
```

If demand increases:

```text
10,000 Requests
      ↓
Multiple Function Instances
```

This allows the application to handle changing workloads.

---

# Scale Out and Scale In

### Scale Out

More function instances are created.

```text
1 Instance
    ↓
5 Instances
    ↓
10 Instances
```

### Scale In

Instances are reduced when demand decreases.

```text
10 Instances
    ↓
5 Instances
    ↓
1 Instance
```

The exact scaling behavior depends on the hosting plan and workload.

---

# Networking

Function Apps can integrate with Azure networking capabilities.

Depending on the hosting plan and configuration, networking features can include:

- VNet integration
- Private endpoints
- Access restrictions
- Outbound connectivity
- Private access to Azure resources

Example:

```text
Function App
     │
     ▼
VNet Integration
     │
     ▼
Private Azure Resources
```

---

# Authentication and Authorization

Function Apps can use authentication and authorization features to control access to applications and HTTP endpoints.

For example:

```text
Client
  ↓
Authentication
  ↓
Function App
  ↓
HTTP Function
```

Authentication and authorization configuration can be managed through the Function App settings.

---

# Deployment Settings

Function Apps can be deployed using different methods, including:

- Azure Portal
- Azure CLI
- Visual Studio Code
- GitHub
- CI/CD pipelines
- Zip deployment
- Other supported deployment technologies

Example:

```text
Source Code
     ↓
Deployment
     ↓
Function App
     ↓
Function
```

Deployment is covered in detail in **18.4 — Azure Functions Deployment**.

---

# Function App Configuration Workflow

```text
Create Function App
        ↓
Select Runtime
        ↓
Select Hosting Plan
        ↓
Configure Application Settings
        ↓
Configure Networking
        ↓
Create Functions
        ↓
Deploy Code
        ↓
Monitor and Scale
```

---

# Hosting Plan Comparison

| Feature | Flex Consumption | Consumption | Premium | Dedicated |
|---|---|---|---|---|
| Serverless model | Yes | Yes | Yes | No |
| Automatic scaling | Yes | Yes | Yes | Depends on configuration |
| Pay-per-use model | Yes | Yes | No | No |
| Prewarmed instances | Supported | No | Yes | Available through App Service capacity |
| VNet integration | Supported | Limited / plan-dependent | Yes | Yes |
| Dedicated compute | No | No | Yes | Yes |

> **Note:** Azure Functions hosting capabilities and plan features can change. Always verify the current supported features when designing production workloads.

---

# Practical Lab

No separate lab is required for this topic.

Hosting-plan configuration and Function App settings are best reinforced while creating and deploying the function in:

**18.4 — Azure Functions Deployment**

---

# Key Points

- A **Function App** provides the environment where Azure Functions run.
- Application settings store configuration values for the Function App.
- Application settings are exposed to functions as environment variables.
- Function Apps use a selected runtime and programming language.
- Azure Functions supports multiple hosting options.
- Hosting plans differ in scaling, networking, performance, and pricing characteristics.
- Functions can automatically scale based on workload and hosting configuration.
- **Scale out** increases function instances.
- **Scale in** reduces function instances.
- Function Apps can integrate with Azure networking capabilities.
- Authentication and access controls can be configured for Function Apps.
- Function App configuration should be separated from application code where possible.
