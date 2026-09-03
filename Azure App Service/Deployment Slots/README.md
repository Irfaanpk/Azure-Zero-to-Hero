# 10.2 Deployment Slots

## What are Deployment Slots?

**Deployment Slots** are separate environments within an Azure App Service Web App.

They allow you to deploy and test a new version of an application in a separate slot before making it available to production users.

```text
                App Service
                     │
          ┌──────────┴──────────┐
          │                     │
     Production              Staging
          │                     │
     Version 1              Version 2
```

---

# Why Use Deployment Slots?

Deployment slots help reduce application downtime and deployment risk.

Common uses include:

- Testing new application versions
- Staging changes before production
- Blue-green deployments
- Zero-downtime deployments
- Quick rollback
- Testing configuration changes

---

# Production Slot

The **Production** slot is the default slot of an App Service.

Example:

```text
https://myapp.azurewebsites.net
```

This is normally the application accessed by production users.

---

# Staging Slot

A **Staging** slot is a separate environment used to deploy and test a new application version.

Example:

```text
Production
    ↓
Version 1

Staging
    ↓
Version 2
```

The staging slot has its own URL and can be tested before swapping it with production.

---

# Slot URL

Each deployment slot has its own hostname.

Example:

```text
Production:
https://myapp.azurewebsites.net

Staging:
https://myapp-staging.azurewebsites.net
```

This allows you to test the staging application without affecting production.

---

# Deployment Slot Workflow

A typical deployment process is:

```text
Developer
    ↓
Deploy New Version
    ↓
Staging Slot
    ↓
Test Application
    ↓
Swap
    ↓
Production
```

---

# Slot Swap

A **swap** exchanges the application content and configuration between two slots.

Example:

```text
Before Swap:

Production → Version 1
Staging    → Version 2

          ↓ Swap

After Swap:

Production → Version 2
Staging    → Version 1
```

This allows a tested application version to become the production version.

---

# Swap with Preview

**Swap with preview** allows you to test the configuration before completing the swap.

General workflow:

```text
Staging
   ↓
Swap with Preview
   ↓
Apply Production Configuration
   ↓
Validate Application
   ↓
Complete Swap
```

This can help identify configuration problems before the application is switched to production.

---

# Slot-Specific Settings

Some App Service configuration settings can be marked as **deployment slot settings**.

When a setting is marked as a slot setting, it remains with the slot during a swap.

Example:

```text
Production
DATABASE = ProductionDB

Staging
DATABASE = StagingDB
```

After swapping:

```text
Application Code → Swapped
Slot-specific Setting → Remains with Slot
```

This is useful when staging and production require different configuration values.

---

# Slot Swap and Application Settings

Consider:

```text
Production Slot
APP_ENV = production

Staging Slot
APP_ENV = staging
```

If the setting is configured as a **deployment slot setting**, it remains associated with its original slot during the swap.

This allows the same application code to use different environment-specific configuration.

---

# Rollback Using Slot Swap

Deployment slots can also provide a quick rollback mechanism.

Example:

```text
Production
    ↓
New Version
    ↓
Problem Detected
    ↓
Swap Back
    ↓
Previous Version
```

Before swapping, the previous production version remains available in the other slot.

---

# Traffic Routing

App Service can route a percentage of incoming traffic to a deployment slot.

Example:

```text
                 Application
                     │
              ┌──────┴──────┐
              │             │
         Production      Staging
            90%             10%
```

This can be used to gradually test a new version with a portion of users.

---

# Deployment Slots vs Separate App Services

| Deployment Slots | Separate App Services |
|---|---|
| Slots belong to the same App Service | Separate applications |
| Easy application swapping | No built-in slot swap |
| Useful for staging | More infrastructure |
| Supports slot-specific settings | Configuration managed separately |
| Useful for blue-green deployments | More independent environments |

---

# Practical Lab

## Lab: Deploy and Manage an App Service Deployment Slot

### Objective

Create a staging slot, deploy a different application version, test it, swap it with production, and perform a rollback.

---

### Step 1: Open the App Service

1. Open **Azure Portal**.
2. Open the Web App created in the previous lab.
3. Select **Deployment slots**.

---

### Step 2: Create a Staging Slot

1. Select **Add**.
2. Enter:

```text
Slot Name:
staging
```

3. Select the required configuration.
4. Create the slot.

You should now have:

```text
Production
Staging
```

---

### Step 3: Open the Staging Slot

Open the staging slot and copy its URL.

Example:

```text
https://myapp-staging.azurewebsites.net
```

Open the URL and verify the staging application.

---

### Step 4: Deploy a New Version

Deploy a new application version to the **staging slot**.

```text
Production
    ↓
Version 1

Staging
    ↓
Version 2
```

Verify the new version using the staging URL.

---

### Step 5: Configure a Slot Setting

Add an application setting to the staging slot.

Example:

```text
APP_ENV = staging
```

Mark the setting as:

```text
Deployment slot setting
```

Save the configuration.

---

### Step 6: Test the Staging Application

Open the staging URL and verify:

- Application is working
- New version is deployed
- Configuration is correct

Do not modify the production application during testing.

---

### Step 7: Swap Staging with Production

Go to:

```text
Deployment slots
       ↓
Swap
```

Select:

```text
Source: staging
Target: production
```

Perform the swap.

After the swap:

```text
Production → Version 2
Staging    → Version 1
```

---

### Step 8: Verify Production

Open the production URL:

```text
https://myapp.azurewebsites.net
```

Verify that the new application version is now running.

---

### Step 9: Roll Back

If the new version has a problem, perform another swap.

```text
Production
    ↓
Version 2
    ↓
Swap Back
    ↓
Version 1
```

Verify that the previous version is restored.

---

# Key Points

- Deployment slots provide separate environments within an App Service.
- Production is the default slot.
- Staging slots allow applications to be tested before production deployment.
- Each slot has its own URL.
- Slot swapping exchanges application versions between slots.
- Swap with preview allows additional validation before completing a swap.
- Slot-specific settings remain with their respective slots during a swap.
- Deployment slots support quick rollback.
- Traffic can be routed between production and staging slots.
- Deployment slots are useful for blue-green and low-downtime deployments.
