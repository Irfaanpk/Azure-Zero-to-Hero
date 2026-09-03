# 11.3 Azure Container Apps

## What is Azure Container Apps?

**Azure Container Apps** is a serverless container hosting service that allows you to run containerized applications without managing virtual machines or Kubernetes clusters.

```text
Container Image
      ↓
Azure Container Apps
      ↓
Running Application
```

Azure manages the underlying infrastructure while you focus on your application and containers.

---

# Why Use Azure Container Apps?

Azure Container Apps provides:

- Serverless container hosting
- Automatic scaling
- Application ingress
- Revisions
- Traffic splitting
- Environment variables
- Managed identity integration
- Integration with Azure Container Registry

---

# Container Apps Environment

A **Container Apps Environment** provides a secure boundary for one or more container apps.

```text
        Container Apps Environment
                   │
          ┌────────┼────────┐
          │        │        │
       App 1     App 2     App 3
```

Applications within the same environment can communicate with each other.

---

# Container App

A **Container App** runs a containerized application inside a Container Apps environment.

Example:

```text
Container Image
      ↓
Container App
      ↓
Application
```

A Container App can be configured with:

- Container image
- CPU and memory
- Environment variables
- Ingress
- Scaling rules
- Identity

---

# Ingress

**Ingress** controls how network traffic reaches the Container App.

```text
Internet
    ↓
Ingress
    ↓
Container App
    ↓
Container
```

A Container App can expose an application through HTTP/HTTPS ingress.

---

# Environment Variables

Environment variables provide configuration values to the container at runtime.

Example:

```text
APP_ENV=production
PORT=8080
```

This allows configuration to be changed without modifying the container image.

---

# Revisions

A **revision** is an immutable version of a Container App.

Example:

```text
Container App
     │
     ├── Revision 1
     ├── Revision 2
     └── Revision 3
```

Revisions are useful for:

- Deploying new versions
- Testing changes
- Rollback
- Traffic management

---

# Traffic Splitting

Azure Container Apps can distribute traffic between revisions.

Example:

```text
                 Incoming Traffic
                       │
                 ┌─────┴─────┐
                 │           │
              Revision 1  Revision 2
                 80%         20%
```

This can be useful for gradual application releases and testing.

---

# Scaling

Container Apps can automatically scale based on application workload.

```text
Low Workload
     ↓
Fewer Replicas
```

```text
High Workload
     ↓
More Replicas
```

Scaling can be configured using supported scale rules and workload metrics.

---

# Managed Identity

Container Apps can use **managed identities** to access Azure resources without storing credentials inside the application.

Example:

```text
Container App
      │
      │ Managed Identity
      ▼
Azure Resource
```

This can be useful when accessing services such as Azure Container Registry or other Azure resources.

---

# Azure Container Registry Integration

Container Apps can use container images stored in **Azure Container Registry**.

```text
Azure Container Registry
          │
          │ Container Image
          ▼
   Azure Container App
          │
          ▼
      Application
```

This allows private container images to be used for application deployment.

---

# Container Apps vs Azure Container Instances

| Container Apps | Container Instances |
|---|---|
| Designed for application hosting | Designed for simple container execution |
| Supports revisions | No application revisions |
| Supports application scaling | Basic container scaling model |
| Supports traffic splitting | No built-in traffic splitting |
| Supports ingress | Supports networking |
| Suitable for long-running applications | Suitable for simple or short-lived workloads |

---

# Container Apps vs Virtual Machines

| Container Apps | Azure VM |
|---|---|
| Serverless container platform | Virtual machine |
| No OS management | Customer manages OS |
| Container-based | Full operating system |
| Automatic scaling features | Scaling requires additional configuration |
| Less infrastructure management | Greater infrastructure control |

---

# Practical Lab

## Lab: Deploy a Containerized Application

### Objective

Create an Azure Container Apps environment, deploy a containerized application, configure ingress, and test the application.

### Step 1: Create Resource Group

```bash
az group create \
  --name rg-container-app \
  --location eastus
```

### Step 2: Create Container Apps Environment

```bash
az containerapp env create \
  --name container-env \
  --resource-group rg-container-app \
  --location eastus
```

### Step 3: Create Container App

Deploy a sample Nginx container:

```bash
az containerapp create \
  --name my-container-app \
  --resource-group rg-container-app \
  --environment container-env \
  --image nginx:latest \
  --target-port 80 \
  --ingress external \
  --query properties.configuration.ingress.fqdn
```

The command returns the application's FQDN.

---

### Step 4: Test the Application

Open the returned URL in a browser:

```text
https://<CONTAINER-APP-FQDN>
```

You should see the Nginx welcome page.

---

### Step 5: View the Container App

```bash
az containerapp show \
  --name my-container-app \
  --resource-group rg-container-app
```

---

### Step 6: Clean Up

```bash
az group delete \
  --name rg-container-app
```

---

# Key Points

- Azure Container Apps is a serverless container hosting platform.
- It allows applications to run without managing VMs or Kubernetes clusters.
- Container Apps run inside Container Apps environments.
- Ingress provides HTTP/HTTPS access to applications.
- Environment variables provide runtime configuration.
- Revisions represent immutable versions of a Container App.
- Traffic can be split between revisions.
- Container Apps supports automatic scaling.
- Managed identities can provide secure access to Azure resources.
- Azure Container Registry can provide private container images.
- Container Apps is more application-focused than Azure Container Instances.
