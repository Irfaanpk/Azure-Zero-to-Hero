# 11.1 Azure Container Instances

## What is Azure Container Instances?

**Azure Container Instances (ACI)** is a serverless container service that allows you to run containers directly in Azure without managing virtual machines.

```text
Container Image
      ↓
Azure Container Instances
      ↓
Running Container
```

Azure manages the underlying infrastructure while you manage the container.

---

# Why Use Azure Container Instances?

ACI is useful when you need to:

- Run containers quickly
- Run short-lived workloads
- Run simple containerized applications
- Perform testing and development
- Run batch jobs
- Avoid managing virtual machines

---

# Containers vs Virtual Machines

| Containers | Virtual Machines |
|---|---|
| Lightweight | More resource intensive |
| Share the host OS kernel | Have their own guest OS |
| Start quickly | Usually take longer to start |
| Package application and dependencies | Package complete operating system |
| Suitable for containerized workloads | Suitable for full OS workloads |

```text
Virtual Machine

VM
 ├── Guest OS
 ├── Application
 └── Dependencies


Container

Container
 ├── Application
 └── Dependencies
```

---

# Container Image

A **container image** contains the application and everything required to run it.

Example:

```text
Container Image
 ├── Application
 ├── Dependencies
 ├── Libraries
 └── Configuration
```

ACI creates a running container from the image.

```text
Container Image
      ↓
     ACI
      ↓
Running Container
```

Images can come from:

- Azure Container Registry
- Docker Hub
- Other container registries

---

# Container Group

A **container group** is a collection of containers that share the same lifecycle, network, and resources.

```text
          Container Group
                │
        ┌───────┴───────┐
        │               │
   Container 1      Container 2
```

A simple application may use one container, while more complex workloads can use multiple containers.

---

# ACI Networking

ACI containers can be accessed through networking configurations such as:

- Public IP
- Private IP
- DNS name label
- Virtual Network integration

Example:

```text
Internet
    ↓
Public IP
    ↓
ACI Container
    ↓
Application
```

ACI can also be deployed into an Azure Virtual Network for private connectivity.

---

# Environment Variables

Environment variables allow configuration values to be passed to a container at runtime.

Example:

```text
APP_ENV=production
PORT=8080
```

The application can read these values without modifying the container image.

```text
ACI
 │
 └── Environment Variables
          │
          ▼
      Application
```

---

# Restart Policies

ACI provides restart policies that control how containers behave after they exit.

Common policies include:

| Policy | Behavior |
|---|---|
| Always | Container is restarted when it stops |
| Never | Container is not automatically restarted |
| OnFailure | Container is restarted when it exits with a failure |

The appropriate policy depends on the workload.

---

# Container Lifecycle

A basic ACI container lifecycle is:

```text
Create
  ↓
Starting
  ↓
Running
  ↓
Stopping
  ↓
Stopped
```

You can view the container status and logs from Azure Portal or Azure CLI.

---

# Managing Containers

Common management operations include:

- Create container
- Start container
- Stop container
- Restart container
- Delete container
- View logs
- View container status
- View configuration

Azure CLI example:

```bash
az container show \
  --resource-group rg-container \
  --name my-container
```

View logs:

```bash
az container logs \
  --resource-group rg-container \
  --name my-container
```

---

# ACI Use Cases

Azure Container Instances are commonly used for:

- Development and testing
- Short-lived jobs
- Batch processing
- Simple web applications
- Automation tasks
- Temporary workloads
- Container experimentation

---

# ACI vs Azure Container Apps

| ACI | Azure Container Apps |
|---|---|
| Simple container execution | Managed application hosting |
| Quick container deployment | Designed for applications and APIs |
| Basic container management | Supports revisions and application scaling |
| Suitable for short-lived workloads | Suitable for long-running applications |
| Less application-level functionality | More application-level features |

---

# Practical Lab

## Lab: Deploy a Container using Azure Container Instances

### Objective

Create an Azure Container Instance using a public container image and access the running application.

### Step 1: Create Resource Group

```bash
az group create \
  --name rg-container \
  --location eastus
```

### Step 2: Create Container Instance

Deploy an Nginx container:

```bash
az container create \
  --resource-group rg-container \
  --name my-nginx \
  --image nginx \
  --dns-name-label my-nginx-demo \
  --ports 80
```

### Step 3: Verify Container

```bash
az container show \
  --resource-group rg-container \
  --name my-nginx \
  --output table
```

Verify that the container is running.

### Step 4: Get the IP Address

```bash
az container show \
  --resource-group rg-container \
  --name my-nginx \
  --query ipAddress.ip \
  --output tsv
```

Open the returned IP address in a browser:

```text
http://<PUBLIC-IP>
```

You should see the Nginx welcome page.

### Step 5: View Logs

```bash
az container logs \
  --resource-group rg-container \
  --name my-nginx
```

### Step 6: Restart the Container

```bash
az container restart \
  --resource-group rg-container \
  --name my-nginx
```

### Step 7: Clean Up

```bash
az group delete \
  --name rg-container
```

---

# Key Points

- Azure Container Instances provides serverless container execution.
- You do not need to manage virtual machines.
- Containers are created from container images.
- A container group can contain multiple containers.
- ACI supports public and private networking.
- Environment variables can provide runtime configuration.
- Restart policies control container behavior after exit.
- ACI is suitable for simple, short-lived, and containerized workloads.
- Azure Container Registry can be used to store private container images.
- Azure Container Apps provides additional application-level features for more advanced containerized applications.
