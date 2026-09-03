# 18.2 Function Triggers and Bindings

Azure Functions use **triggers** to determine when a function executes and **bindings** to connect the function to other services for input and output.

The basic flow is:

```text
Event
  ↓
Trigger
  ↓
Azure Function
  ↓
Binding
  ↓
Azure Service
```

---

## What is a Trigger?

A **trigger** defines the event that causes an Azure Function to execute.

Every Azure Function must have **exactly one trigger**.

Examples:

- HTTP request
- Timer schedule
- Blob event
- Queue message
- Event Grid event

---

# HTTP Trigger

An **HTTP trigger** runs a function when an HTTP request is received.

```text
Client
  ↓
HTTP Request
  ↓
HTTP Trigger
  ↓
Azure Function
  ↓
HTTP Response
```

Example use cases:

- REST APIs
- Webhooks
- HTTP-based automation
- Simple backend endpoints

Example:

```text
GET /api/hello
       ↓
HTTP Trigger
       ↓
Function
       ↓
"Hello Azure"
```

---

# Timer Trigger

A **Timer trigger** runs a function according to a schedule.

```text
Schedule
   ↓
Timer Trigger
   ↓
Azure Function
   ↓
Task Executes
```

Example use cases:

- Scheduled cleanup
- Daily reports
- Periodic maintenance
- Scheduled data processing

Example:

```text
Every day at 08:00
        ↓
   Timer Trigger
        ↓
      Function
```

---

# Blob Trigger

A **Blob trigger** can execute a function when a blob is created or updated in a configured storage location.

```text
Blob Uploaded
      ↓
Blob Trigger
      ↓
Azure Function
      ↓
Process Blob
```

Example use cases:

- Image processing
- File processing
- Data transformation
- Document processing

---

# Queue Trigger

A **Queue trigger** executes a function when a message is available in an Azure Storage Queue.

```text
Application
    ↓
Queue Message
    ↓
Azure Storage Queue
    ↓
Queue Trigger
    ↓
Azure Function
```

Example use cases:

- Background processing
- Asynchronous workloads
- Order processing
- Task queues

---

# Event Grid Trigger

An **Event Grid trigger** allows a function to respond to events published through Azure Event Grid.

```text
Azure Resource
      ↓
Event
      ↓
Event Grid
      ↓
Function Trigger
      ↓
Azure Function
```

This is useful for event-driven architectures.

---

# Trigger Comparison

| Trigger | Executes When |
|---|---|
| HTTP | HTTP request is received |
| Timer | Scheduled time is reached |
| Blob | Blob-related event occurs |
| Queue | Queue message is available |
| Event Grid | Event is received through Event Grid |

---

# What is a Binding?

A **binding** provides a connection between an Azure Function and another service.

Bindings can provide data to a function or send data from a function.

There are two main types:

```text
Input Binding
     ↓
Azure Service → Function


Output Binding
     ↓
Function → Azure Service
```

---

# Input Binding

An **input binding** provides data to a function.

```text
Azure Service
      ↓
Input Binding
      ↓
Function
```

Example:

```text
Blob Storage
      ↓
Input Binding
      ↓
Azure Function
      ↓
Process Data
```

The function can access the required data without implementing all the service interaction itself.

---

# Output Binding

An **output binding** sends data from a function to another service.

```text
Function
    ↓
Output Binding
    ↓
Azure Service
```

Example:

```text
Azure Function
      ↓
Output Binding
      ↓
Storage Queue
      ↓
Queue Message
```

---

# Trigger vs Binding

A trigger and a binding have different purposes.

| Trigger | Binding |
|---|---|
| Starts the function | Connects the function to a service |
| Defines when execution occurs | Defines input/output data interaction |
| Exactly one trigger per function | A function can have multiple bindings |
| Required | Optional depending on the function |

Simple example:

```text
Blob Uploaded
      ↓
Blob Trigger
      ↓
Function
      ↓
Output Binding
      ↓
Queue
```

Here:

```text
Blob Trigger
     ↓
Starts the function

Output Binding
     ↓
Sends the result to the queue
```

---

# Input and Output Together

A function can use both input and output bindings.

Example:

```text
Blob Storage
     ↓
Input Binding
     ↓
Azure Function
     ↓
Output Binding
     ↓
Storage Queue
```

The function:

1. Receives data from Blob Storage
2. Processes the data
3. Sends the result to a queue

---

# Multiple Bindings

A function can have multiple bindings.

Example:

```text
             ┌── Input Binding → Blob
             │
Trigger → Function
             │
             ├── Output Binding → Queue
             │
             └── Output Binding → Database
```

The function still has only **one trigger**, but it can interact with multiple services through bindings.

---

# Trigger and Binding Example

Consider an image-processing application:

```text
User Uploads Image
        ↓
Blob Storage
        ↓
Blob Trigger
        ↓
Azure Function
        ↓
Process Image
        ↓
Output Binding
        ↓
Storage / Queue
```

The trigger starts the function, while the bindings provide connections to other services.

---

# Event-Driven Architecture

Triggers and bindings make Azure Functions suitable for event-driven applications.

Example:

```text
                    ┌── Blob Trigger
                    │
Blob Upload ────────┤
                    │
                    ▼
              Azure Function
                    │
                    ├── Output → Queue
                    │
                    └── Output → Storage
```

Instead of continuously running an application and checking for events, the function executes when the configured event occurs.

---

# Trigger vs Polling

Traditional application:

```text
Application
     ↓
Check for new data
     ↓
Wait
     ↓
Check again
     ↓
Process data
```

Event-driven function:

```text
Event
  ↓
Trigger
  ↓
Function
  ↓
Process data
```

This can reduce unnecessary processing and is well suited for event-driven workloads.

---

# Common Trigger and Binding Combinations

### HTTP Function

```text
HTTP Request
     ↓
HTTP Trigger
     ↓
Function
     ↓
HTTP Response
```

### Blob Processing

```text
Blob Upload
     ↓
Blob Trigger
     ↓
Function
     ↓
Output Binding
     ↓
Storage
```

### Queue Processing

```text
Queue Message
     ↓
Queue Trigger
     ↓
Function
     ↓
Output Binding
     ↓
Another Service
```

### Scheduled Task

```text
Timer
 ↓
Timer Trigger
 ↓
Function
 ↓
Output Binding
 ↓
Azure Service
```

---

# Practical Lab

## Lab — Create an HTTP-Triggered Azure Function

### Objective

Create an HTTP-triggered Azure Function and test it using a browser or HTTP client.

### Steps

1. Create an **Azure Function App**.
2. Select a supported runtime such as Python, .NET, or JavaScript.
3. Create a new function.
4. Select **HTTP trigger**.
5. Configure the authorization level.
6. Deploy or create the function.
7. Open the function's URL.
8. Send an HTTP request.
9. Verify that the function executes.
10. Check the function execution details in the Azure Portal.

Architecture:

```text
Browser / HTTP Client
        ↓
HTTP Request
        ↓
HTTP Trigger
        ↓
Azure Function
        ↓
HTTP Response
```

---

# Key Points

- A **trigger** determines when an Azure Function executes.
- Every function has **exactly one trigger**.
- Common triggers include HTTP, Timer, Blob, Queue, and Event Grid.
- **Bindings** connect functions to other Azure services.
- **Input bindings** provide data to functions.
- **Output bindings** send data from functions to other services.
- A function can have multiple bindings.
- Triggers and bindings simplify event-driven application development.
- Azure Functions can process events without continuously running a server.
