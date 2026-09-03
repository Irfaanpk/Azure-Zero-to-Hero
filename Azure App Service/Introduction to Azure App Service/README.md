# 10.1 Introduction to Azure App Service

## What is Azure App Service?

**Azure App Service** is a fully managed Platform as a Service (PaaS) offering that allows you to host web applications, APIs, and backend applications without managing the underlying virtual machines.

```text
                    Azure App Service
                           │
              ┌────────────┼────────────┐
              │            │            │
           Web App         API       Backend App
              │
              ▼
        Managed Platform
```

Azure manages the underlying infrastructure, while you focus on deploying and configuring your application.

---

# Why Use Azure App Service?

App Service provides:

- Managed application hosting
- Automatic scaling
- Built-in load balancing
- HTTPS support
- Custom domains
- Application configuration
- Deployment options
- Monitoring and diagnostics
- Integration with Azure services

You do not need to manually manage:

- Operating system
- VM patching
- Physical servers
- Basic infrastructure maintenance

---

# App Service Plan

An **App Service Plan** defines the compute resources used by App Service applications.

It determines:

- Region
- Operating system
- VM size
- Number of VM instances
- Pricing tier
- Available scaling features

```text
              App Service Plan
                     │
          ┌──────────┼──────────┐
          │          │          │
       Web App    Web App     API App
```

Multiple App Service apps can run within the same App Service Plan.

---

# App Service Architecture

```text
                       Internet
                          │
                          ▼
                  Azure App Service
                          │
              ┌───────────┼───────────┐
              │           │           │
           Web App      Web App     API App
              │           │           │
              └───────────┼───────────┘
                          │
                  App Service Plan
                          │
                    Azure Platform
```

---

# Web Apps

An **App Service Web App** provides a managed environment for hosting web applications.

Applications can be built using different programming languages and frameworks.

Examples include:

- .NET
- Java
- Node.js
- Python
- PHP

The available application stacks depend on the App Service configuration and operating system.

---

# Application Stacks

When creating a Web App, you select the required application stack.

Example:

```text
Runtime Stack
     │
     ├── .NET
     ├── Java
     ├── Node.js
     ├── Python
     └── PHP
```

The selected stack determines the runtime environment available to the application.

---

# App Service Deployment Options

Applications can be deployed to App Service using different methods.

Common options include:

- Azure Portal
- Azure CLI
- GitHub
- ZIP deployment
- Azure DevOps
- Visual Studio
- FTP

Basic deployment flow:

```text
Application
     ↓
Deployment Method
     ↓
Azure App Service
     ↓
Running Web Application
```

---

# App Service Configuration

App Service provides configuration settings that can be used by applications.

Common settings include:

- Application settings
- Environment variables
- Connection strings
- General application settings
- Startup configuration

Example:

```text
Application
     │
     ├── APP_ENV=production
     ├── DATABASE_URL=...
     └── API_KEY=...
```

> Sensitive values should be protected appropriately and should not be hard-coded into application source code.

---

# Application Settings

**Application settings** are environment variables provided to the application at runtime.

They are useful for storing configuration values that may differ between environments.

Example:

```text
Development
DATABASE_NAME = devdb

Production
DATABASE_NAME = proddb
```

This allows the same application code to be used in different environments.

---

# Custom Domains

By default, an App Service application receives an Azure-provided hostname.

Example:

```text
https://myapp.azurewebsites.net
```

You can configure a custom domain such as:

```text
https://www.example.com
```

The custom domain must be configured with the appropriate DNS records.

---

# HTTPS and TLS

App Service supports HTTPS to secure application traffic.

```text
Client
   │
   │ HTTPS
   ▼
Azure App Service
```

HTTPS protects communication between clients and the application.

App Service also supports TLS/SSL certificate configuration for custom domains.

---

# App Service Scaling

App Service supports both **vertical scaling** and **horizontal scaling**.

## Vertical Scaling

Vertical scaling means changing the App Service Plan pricing tier or compute resources.

```text
Smaller Tier
     ↓
Larger Tier
```

This provides more resources to the application.

---

## Horizontal Scaling

Horizontal scaling means increasing or decreasing the number of application instances.

```text
1 Instance
     ↓
3 Instances
```

```text
              App Service
                   │
        ┌──────────┼──────────┐
        │          │          │
     Instance 1 Instance 2 Instance 3
```

Horizontal scaling helps applications handle increased traffic.

---

# Autoscaling

App Service can automatically adjust the number of instances based on workload conditions.

Example:

```text
High Application Load
        ↓
    Scale Out
        ↓
More Instances
```

```text
Low Application Load
        ↓
     Scale In
        ↓
Fewer Instances
```

Autoscaling can help maintain application performance while controlling resource usage.

---

# App Service Networking

App Service provides networking features for controlling application connectivity.

Important concepts include:

- Inbound access restrictions
- Outbound connectivity
- Virtual network integration
- Private access options

### Access Restrictions

Access restrictions can control which IP addresses or networks are allowed to access the application.

```text
Internet
   │
   ▼
Access Restrictions
   │
   ├── Allowed
   └── Denied
```

### VNet Integration

App Service can integrate with an Azure Virtual Network to provide access from the application to resources inside the VNet.

Example:

```text
App Service
     │
     ▼
VNet Integration
     │
     ▼
Azure VNet
     │
     ▼
Private Resources
```

---

# App Service Backup

App Service supports application backup for supported configurations and tiers.

Backups can help protect:

- Application content
- Configuration
- Databases supported by the backup configuration

Backup configuration can include a storage account for storing backup data.

---

# App Service Security

Important security features include:

- HTTPS
- TLS
- Access restrictions
- Authentication and authorization
- Managed identities
- Private connectivity options

These features help secure applications and their access to Azure resources.

---

# App Service vs Azure Virtual Machine

| App Service | Azure Virtual Machine |
|---|---|
| PaaS | IaaS |
| Azure manages the OS | Customer manages the OS |
| Less infrastructure management | More infrastructure management |
| Designed for applications and APIs | Full server control |
| Built-in application features | More manual configuration |
| Easy scaling | Scaling requires more management |

---

# App Service Use Cases

Azure App Service is commonly used for:

- Websites
- Web APIs
- Business applications
- Backend applications
- Mobile application backends
- Public web applications

---

# Practical Lab

## Lab: Deploy and Configure an App Service

### Objective

Create an Azure App Service, deploy a web application, configure application settings, and practice basic scaling and access configuration.

### Steps

1. Open **Azure Portal**.
2. Search for **App Services**.
3. Select **Create → Web App**.
4. Select the subscription.
5. Create or select a resource group.
6. Enter a unique Web App name.
7. Select the required region.
8. Select an application stack.
9. Create or select an **App Service Plan**.
10. Select the required pricing tier.
11. Review the configuration.
12. Select **Create**.

---

### Deploy the Application

After deployment:

1. Open the Web App.
2. Copy the default application URL.
3. Verify that the application is running.
4. Deploy a sample application using a supported deployment method.
5. Verify the deployed application.

---

### Configure Application Settings

Go to:

```text
App Service
    ↓
Settings
    ↓
Environment variables
```

Add an application setting.

Example:

```text
APP_ENV = production
```

Save the configuration.

---

### Test Scaling

Open:

```text
App Service
    ↓
Scale up
```

Review the available pricing tiers.

Then open:

```text
App Service
    ↓
Scale out
```

Configure the required number of instances.

Verify the scaling configuration.

---

### Configure Access Restrictions

Open:

```text
App Service
    ↓
Networking
    ↓
Access Restrictions
```

Add an access rule and configure the required IP address or network.

Verify the access behavior.

---

# Key Points

- Azure App Service is a managed PaaS platform.
- It can host web applications, APIs, and backend applications.
- An App Service Plan provides the compute resources for App Service apps.
- Multiple apps can share the same App Service Plan.
- App Service supports multiple application stacks.
- Applications can be deployed using Portal, CLI, GitHub, ZIP deployment, and other methods.
- Application settings provide runtime configuration.
- App Service supports custom domains and HTTPS/TLS.
- App Service supports vertical and horizontal scaling.
- Autoscaling can automatically adjust application instances.
- Access restrictions control inbound application access.
- VNet integration allows App Service applications to access resources in an Azure VNet.
- App Service reduces the need to manage operating systems and underlying infrastructure.
