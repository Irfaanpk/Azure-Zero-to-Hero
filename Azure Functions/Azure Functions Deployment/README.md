# 18.4 Azure Functions Deployment

Azure Functions can be deployed using different tools and deployment methods. The deployment process transfers your function code and configuration to the **Function App** so it can run in Azure.

---

## Deployment Workflow

```text
Function Code
      ↓
Deployment Method
      ↓
Azure Function App
      ↓
Function
      ↓
Execute
```

---

# Common Deployment Methods

Azure Functions can be deployed using:

- Azure Portal
- Azure CLI
- Visual Studio Code
- GitHub Actions
- Azure DevOps
- Zip deployment
- CI/CD pipelines

---

# Azure Portal Deployment

The Azure Portal can be used to create and manage Function Apps and configure deployment options.

Basic workflow:

```text
Azure Portal
     ↓
Function App
     ↓
Deployment
     ↓
Function Code
```

The Portal is useful for learning and simple deployments.

---

# Azure CLI Deployment

Azure CLI can be used to automate Function App deployments.

Example:

```bash
az functionapp deployment source config-zip \
  --resource-group myResourceGroup \
  --name myFunctionApp \
  --src function.zip
```

The ZIP file contains the function application files.

```text
function.zip
     ↓
Azure CLI
     ↓
Function App
     ↓
Deployed Function
```

---

# Visual Studio Code

Visual Studio Code provides Azure Functions development and deployment support through the Azure Functions extension.

Basic workflow:

```text
Write Function
     ↓
Test Locally
     ↓
Sign in to Azure
     ↓
Select Function App
     ↓
Deploy
```

This is useful when developing functions locally.

---

# GitHub Actions

Azure Functions can be integrated with **GitHub Actions** for automated deployments.

Example:

```text
Developer
    ↓
Git Push
    ↓
GitHub Repository
    ↓
GitHub Actions
    ↓
Azure Function App
```

A CI/CD workflow can automatically deploy changes after code is pushed to a configured branch.

---

# Zip Deployment

With ZIP deployment, application files are packaged into a ZIP archive and deployed to the Function App.

Example:

```text
Function Files
     ↓
function.zip
     ↓
ZIP Deployment
     ↓
Function App
```

Example:

```bash
az functionapp deployment source config-zip \
  --resource-group myResourceGroup \
  --name myFunctionApp \
  --src function.zip
```

---

# Local Development

Functions can be developed and tested locally before deployment.

Typical workflow:

```text
Local Development
       ↓
Local Testing
       ↓
Package / Deploy
       ↓
Azure Function App
       ↓
Cloud Testing
```

The **Azure Functions Core Tools** can be used for local development and testing.

Example:

```bash
func start
```

This starts the Functions host locally.

---

# Function App Configuration During Deployment

Deployment involves more than just application code.

Important configuration can include:

- Runtime
- Application settings
- Environment variables
- Connection settings
- Hosting plan
- Networking configuration

Example:

```text
Function Code
      +
Application Configuration
      ↓
Function App
```

Application settings should be managed separately from source code when possible.

---

# Deployment Slots

Function Apps can use deployment slots when supported by the selected hosting configuration.

Example:

```text
Function App
     │
     ├── Production
     │
     └── Staging
```

A staging environment can be used to test a new version before moving it to production.

```text
New Version
     ↓
Staging
     ↓
Test
     ↓
Production
```

---

# Deployment Strategies

### Direct Deployment

Deploy directly to the production Function App.

```text
Code
 ↓
Production
```

Simple, but changes immediately affect production.

---

### Staged Deployment

Deploy to a staging environment first.

```text
Code
 ↓
Staging
 ↓
Testing
 ↓
Production
```

This reduces the risk of introducing untested changes directly into production.

---

# Deployment Verification

After deployment, verify that:

- Function App is running
- Function is available
- Trigger works
- Application settings are correct
- Logs show successful execution
- Expected response is returned

Example:

```text
Deploy
  ↓
Test Trigger
  ↓
Check Logs
  ↓
Verify Result
```

---

# Deployment Monitoring

Azure Monitor and Application Insights can be used to monitor deployed functions.

Example:

```text
Function App
     ↓
Function Execution
     ↓
Telemetry / Logs
     ↓
Azure Monitor
     ↓
Troubleshooting
```

Detailed monitoring is covered in the **Azure Monitor** section.

---

# Practical Lab

## Lab — Deploy an HTTP-Triggered Azure Function

### Objective

Create an HTTP-triggered Azure Function, deploy it to Azure, and verify that it works.

### Steps

1. Create a Function App in Azure.

2. Install Azure Functions Core Tools if developing locally.

3. Create a local Functions project.

4. Create an HTTP-triggered function.

5. Test the function locally:

```bash
func start
```

6. Verify the HTTP response locally.

7. Sign in to Azure:

```bash
az login
```

8. Deploy the function to the Azure Function App.

9. Verify the deployment.

10. Open the Function App and obtain the function URL.

11. Send an HTTP request to the deployed function.

12. Verify the response.

13. Check Function App logs to confirm execution.

Architecture:

```text
Local Function
      ↓
Test
      ↓
Deploy
      ↓
Azure Function App
      ↓
HTTP Trigger
      ↓
Function Execution
      ↓
HTTP Response
```

---

# Key Points

- Azure Functions supports multiple deployment methods.
- Functions can be deployed through Azure Portal, Azure CLI, VS Code, GitHub Actions, and CI/CD pipelines.
- **ZIP deployment** packages function files into a ZIP archive.
- Azure Functions Core Tools can be used for local development and testing.
- Application configuration should be managed separately from application code where possible.
- Deployment slots can provide staging and production environments when supported.
- Always verify the function after deployment.
- Azure Monitor and Application Insights can help monitor deployed functions.
