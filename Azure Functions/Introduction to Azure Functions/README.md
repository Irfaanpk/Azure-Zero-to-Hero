# 18.1 Introduction to Azure Functions

**Azure Functions** is a serverless compute service that allows you to run code without managing the underlying servers.

You write the required code, define how it should be triggered, and Azure Functions handles the infrastructure, execution, and scaling.

---

## What is Serverless Computing?

In traditional computing:

```text
Application
    ↓
Virtual Machine
    ↓
Operating System
    ↓
Server Management
```

With Azure Functions:

```text
Event / Request
      ↓
Azure Function
      ↓
Your Code
      ↓
Result
```

You don't need to manage the underlying server infrastructure.

---

# What is an Azure Function?

An **Azure Function** is a small unit of code that performs a specific task.

For example:

```text
HTTP Request
     ↓
Azure Function
     ↓
Process Request
     ↓
Return Response
```

Another example:

```text
Blob Uploaded
      ↓
Azure Function
      ↓
Process Blob
      ↓
Save Result
```

Functions are commonly used for **event-driven applications**.

---

# Function App

A **Function App** is the Azure resource that hosts one or more functions.

The relationship is:

```text
Function App
     │
     ├── Function 1
     ├── Function 2
     └── Function 3
```

A Function App provides the environment in which your functions run.

---

# Function App vs Function

These are different concepts.

| Function App | Function |
|---|---|
| Azure resource | Individual piece of code |
| Hosts functions | Performs a specific task |
| Provides runtime environment | Executes when triggered |
| Contains one or more functions | Belongs to a Function App |

Example:

```text
Function App
      │
      ├── HTTP Function
      ├── Blob Function
      └── Timer Function
```

---

# Event-Driven Execution

Azure Functions are commonly triggered by events.

```text
Event
  ↓
Trigger
  ↓
Azure Function
  ↓
Code Executes
```

Examples of events:

- HTTP request
- Timer
- Blob upload
- Queue message
- Event-based message

For example:

```text
User sends HTTP request
          ↓
     HTTP Trigger
          ↓
    Azure Function
          ↓
      Code Runs
```

---

# Triggers

A **trigger** defines how a function starts executing.

Examples:

| Trigger | Function Starts When |
|---|---|
| HTTP | HTTP request is received |
| Timer | Scheduled time is reached |
| Blob | Blob-related event occurs |
| Queue | Queue message is received |

Example:

```text
HTTP Request
     ↓
HTTP Trigger
     ↓
Function
```

Triggers are covered in detail in **18.2 — Function Triggers and Bindings**.

---

# Bindings

**Bindings** provide a way for a function to connect to other Azure services without writing all the connection-handling code manually.

There are two common types:

### Input Binding

Provides data to the function.

```text
Azure Service
      ↓
Input Binding
      ↓
Function
```

### Output Binding

Sends data from the function to another service.

```text
Function
    ↓
Output Binding
    ↓
Azure Service
```

Example:

```text
Blob
 ↓
Input Binding
 ↓
Function
 ↓
Output Binding
 ↓
Storage Queue
```

---

# Supported Programming Languages

Azure Functions supports several programming languages and development models.

Common options include:

- C#
- Java
- JavaScript
- TypeScript
- Python
- PowerShell

The available language versions and programming models depend on the current Azure Functions runtime.

---

# Hosting and Scaling

Azure Functions can automatically scale based on workload and the selected hosting plan.

Example:

```text
Low Traffic
    ↓
Few Function Instances

High Traffic
    ↓
More Function Instances
```

This allows applications to handle changing workloads without manually creating and managing VM instances.

Hosting and scaling are covered in detail in **18.3 — Function App Configuration and Hosting**.

---

# Azure Functions vs Azure Virtual Machines

| Azure Functions | Azure Virtual Machines |
|---|---|
| Serverless | Infrastructure-based |
| No server management | You manage the VM |
| Event-driven | General-purpose computing |
| Automatic scaling options | Scaling must be configured |
| Pay for execution/hosting model | Pay for VM resources |
| Good for short-lived tasks and event processing | Good for full OS/application control |

---

# Azure Functions vs Azure App Service

Both can host applications, but they are designed for different scenarios.

| Azure Functions | Azure App Service |
|---|---|
| Serverless functions | Web application hosting |
| Event-driven | Application/web hosting |
| Function-based execution | Continuous application hosting |
| Trigger-based | HTTP/web application based |
| Automatic scaling options | App Service scaling |

Example:

```text
Azure Functions
      ↓
Event → Function → Result


Azure App Service
      ↓
Web Application
      ↓
Users
```

---

# Common Use Cases

Azure Functions can be used for:

### API Endpoints

```text
HTTP Request
      ↓
Azure Function
      ↓
API Response
```

### File Processing

```text
File Uploaded
      ↓
Blob Trigger
      ↓
Azure Function
      ↓
Process File
```

### Scheduled Tasks

```text
Timer
  ↓
Azure Function
  ↓
Scheduled Task
```

### Queue Processing

```text
Queue Message
      ↓
Azure Function
      ↓
Process Message
```

---

# Basic Architecture

```text
                  Azure Function App
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
     HTTP Trigger   Blob Trigger   Timer Trigger
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                   Function Code
                         │
                         ▼
                 Azure Services
```

---

# Key Points

- **Azure Functions** is a serverless compute service.
- You run code without managing the underlying servers.
- A **Function App** is the Azure resource that hosts functions.
- A Function App can contain multiple functions.
- Functions are commonly **event-driven**.
- A **trigger** determines when a function executes.
- **Bindings** connect functions to other services for input and output.
- Azure Functions supports multiple programming languages.
- Azure Functions can scale based on workload and hosting configuration.
- Common use cases include APIs, file processing, scheduled tasks, and queue processing.
- Azure Functions is different from Azure VMs and Azure App Service.

---

## Next

➡️ **18.2 — Function Triggers and Bindings**
