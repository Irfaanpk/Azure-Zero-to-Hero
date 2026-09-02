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

A company can have one Microsoft Entra tenant that contains all its organizational identities:

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

A production application can have a resource group containing:

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

## Azure Account Structure Examples

Here are some common ways organizations can structure their Azure environment.

### Example 1: Separate Environments

A company can use separate subscriptions for different environments.

**Development**

- Subscription: `Development`
- Resource Group: `Dev-RG`
- Resources:
  - Virtual Machine
  - Storage Account
  - Virtual Network

**Production**

- Subscription: `Production`
- Resource Group: `Prod-RG`
- Resources:
  - Virtual Machine
  - Storage Account
  - Azure SQL Database

This approach helps separate development and production environments and makes access and cost management easier.

---

### Example 2: Multiple Applications

A company can organize resources based on applications.

**E-Commerce Application**

- Resource Group: `Ecommerce-RG`
- Resources:
  - App Service
  - Azure SQL Database
  - Storage Account
  - Virtual Network

**Employee Portal**

- Resource Group: `EmployeePortal-RG`
- Resources:
  - App Service
  - Azure SQL Database
  - Key Vault
  - Virtual Network

This approach keeps resources related to each application organized together.

---

### Example 3: Multiple Teams

A company can also organize subscriptions and resource groups based on teams or business units.

**Development Team**

- Subscription: `Development-Subscription`
- Resource Groups:
  - `Frontend-RG`
  - `Backend-RG`
  - `Database-RG`

**Data Team**

- Subscription: `Data-Subscription`
- Resource Groups:
  - `Analytics-RG`
  - `DataWarehouse-RG`

This can help organizations manage access and responsibilities between different teams.

---

### Example 4: Simple Learning Environment

For learning and practice, you can keep the structure simple.

**Azure Subscription**

- Resource Group: `Learning-RG`
- Resources:
  - Virtual Machine
  - Storage Account
  - Virtual Network
  - Key Vault

This is a simple structure for beginners because resources related to learning can be managed from one resource group.

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

**Tenant → Subscription → Resource Group → Resources**

Each level serves a different purpose:

- **Tenant:** Identity and authentication
- **Subscription:** Billing, access, and resource boundary
- **Resource Group:** Resource organization and management
- **Resources:** Actual Azure services

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
