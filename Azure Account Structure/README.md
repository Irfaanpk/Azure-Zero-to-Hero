# Azure Account Structure

Understanding the Azure account structure is important before creating and managing Azure resources.

Azure organizes resources using a hierarchy that helps with identity, billing, access control, management, and resource organization.

---

## Azure Resource Hierarchy

The basic Azure structure consists of:

**Microsoft Entra Tenant → Subscription → Resource Group → Resources**

Each level has a different purpose in managing an Azure environment.

---

## 1. Microsoft Entra Tenant

A **Microsoft Entra Tenant** is a dedicated identity boundary for an organization.

It contains and manages identities such as:

- Users
- Groups
- Applications
- Service Principals
- Managed Identities

The tenant is responsible for identity and authentication.

### Example

**Microsoft Entra Tenant**

- Users
- Groups
- Applications
- Service Principals
- Managed Identities

---

## 2. Azure Subscription

An **Azure Subscription** is a logical container used to organize Azure resources and manage billing and access.

A subscription is associated with a Microsoft Entra tenant.

### Why Do We Need a Subscription?

A subscription provides:

- Billing boundary
- Resource organization
- Access control boundary
- Usage and cost tracking
- Resource limits and quotas

### Multiple Subscriptions

An organization can have multiple subscriptions for different environments, teams, or business requirements.

For example:

- Development Subscription
- Testing Subscription
- Production Subscription

---

## 3. Resource Group

A **Resource Group** is a logical container used to organize and manage Azure resources.

Every Azure resource belongs to a resource group.

A resource group can contain resources from different Azure services.

### Example

**Production Resource Group**

- Virtual Machine
- Storage Account
- Virtual Network
- Azure SQL Database

### Important Points

- A resource group belongs to one subscription.
- A resource can belong to only one resource group.
- A resource group can contain resources from different Azure services.
- Access control can be assigned at the resource group level.
- Resources can be organized based on an application, environment, or team.
- Resources in a resource group can be managed together.

---

## 4. Azure Resources

An **Azure Resource** is an individual service or component that you create and manage in Azure.

Examples include:

- Virtual Machine
- Storage Account
- Virtual Network
- Network Interface
- Public IP Address
- Azure SQL Database
- Key Vault
- App Service

Resources are created inside resource groups and belong to a subscription.

---

## Azure Account Structure Example

Consider a company with separate development and production environments.

**Microsoft Entra Tenant**

→ **Development Subscription**

→ **Development Resource Group**

→ Virtual Machine  
→ Storage Account  
→ Virtual Network

→ **Production Subscription**

→ **Production Resource Group**

→ Virtual Machine  
→ Storage Account  
→ Azure SQL Database

This structure allows organizations to separate environments and manage access, resources, and costs more effectively.

---

## Resource Group Lifecycle

Resource groups are also useful for managing the lifecycle of an application.

For example:

**Application Resource Group**

- Virtual Machine
- Storage Account
- Virtual Network
- Database

When the application is no longer required, the resource group can be deleted.

> **Warning:** Deleting a resource group can also delete the resources contained within it. Always verify the resources before deleting a resource group.

---

## Azure Hierarchy

The overall Azure structure can be remembered as:

**Tenant**

→ **Subscription**

→ **Resource Group**

→ **Resources**

Each level serves a different purpose:

- **Tenant:** Identity and authentication
- **Subscription:** Billing, access, and resource boundary
- **Resource Group:** Resource organization and management
- **Resources:** Actual Azure services

---

## Azure vs AWS

| Azure | AWS |
|---|---|
| Microsoft Entra Tenant | AWS Identity environment |
| Azure Subscription | AWS Account |
| Resource Group | No direct equivalent |
| Azure Resource | AWS Resource |
| Azure RBAC | IAM permissions |

These are conceptual comparisons and are not exact one-to-one equivalents.

---

## Key Points

- A **Microsoft Entra Tenant** manages identities and authentication.
- An **Azure Subscription** provides a billing and management boundary.
- A **Resource Group** organizes Azure resources.
- **Resources** are the actual Azure services you create.
- A tenant can have multiple subscriptions.
- A subscription can contain multiple resource groups.
- A resource group can contain multiple Azure resources.
- Azure RBAC can be assigned at different scopes, including subscription and resource group levels.
