# 5.1 Introduction to Microsoft Entra ID

Microsoft Entra ID is Microsoft's cloud-based identity and access management service. It helps organizations manage identities and control access to Azure resources and applications.

Microsoft Entra ID was previously known as **Azure Active Directory (Azure AD)**.

---

## What is Microsoft Entra ID?

Microsoft Entra ID provides identity and access management capabilities for Azure and other Microsoft services.

It helps organizations manage:

- Users
- Groups
- Applications
- Authentication
- Access to Azure resources

---

## Microsoft Entra Tenant

A **Microsoft Entra tenant** is a dedicated instance of Microsoft Entra ID for an organization.

The tenant acts as the organization's identity boundary.

A tenant contains objects such as:

- Users
- Groups
- Applications
- Service Principals
- Managed Identities

### Example

```text
ABC Technologies
        │
        └── Microsoft Entra Tenant
                │
                ├── Users
                ├── Groups
                ├── Applications
                ├── Service Principals
                └── Managed Identities
