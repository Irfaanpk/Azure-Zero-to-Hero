<div align="center">

<img src="./assets/z2h.jpeg" alt="Azure Zero to Hero Banner">

<br><br>

<img src="https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white">
<img src="https://img.shields.io/badge/Level-Beginner%20to%20Advanced-purple?style=for-the-badge">
<img src="https://img.shields.io/badge/Entra%20ID-Identity-blue?style=for-the-badge&logo=microsoft">
<img src="https://img.shields.io/badge/Storage-Storage-569A31?style=for-the-badge&logo=microsoftazure&logoColor=white">
<img src="https://img.shields.io/badge/Networking-Networking-blue?style=for-the-badge&logo=microsoftazure&logoColor=white">

<br>

<img src="https://img.shields.io/badge/Contributions-Welcome-brightgreen?style=for-the-badge">

</div>

<div align="center">

# Azure — Zero to Hero

### ☁️ From cloud fundamentals to advanced Azure architecture — comprehensive explanations, hands-on labs, and real-world best practices.

</div>

---

## 📖 About This Repository

**Azure Zero to Hero** is a structured, project-based learning repository designed to take you from understanding the basics of cloud computing all the way through deploying secure, scalable, and highly available architectures on Microsoft Azure.

Every section is organized into its own folder with a dedicated `README.md` covering in-depth explanations, diagrams, Azure Portal walkthroughs, and hands-on labs. The content progresses logically — from foundational cloud concepts to advanced Azure networking and security.

By the end of this course, you will be able to:

- ✅ Explain cloud computing concepts, deployment models, and service models
- ✅ Navigate Azure global infrastructure — Regions, Availability Zones, and Azure edge locations
- ✅ Implement fine-grained access control using Microsoft Entra ID, Azure RBAC, roles, and policies
- ✅ Store, secure, and manage data at scale using Azure Storage
- ✅ Design isolated, secure cloud networks using Azure Virtual Network
- ✅ Connect and protect Azure networks using VNet Peering, VPN Gateway, Private Endpoints, and Azure Firewall

---

## 📚 Table of Contents

## 1. Introduction to Cloud Computing

This section introduces the fundamentals of cloud computing — what it is, why it exists, and how it differs from traditional on-premises IT infrastructure. It covers key characteristics, business benefits, and real-world use cases that form the foundation for understanding modern cloud platforms like Azure.

📂 **[Explore → Introduction to Cloud Computing](./Introduction%20to%20cloud%20computing/)**

---

## 2. Cloud Deployment and Service Models

This section explains how cloud environments are architected and consumed. It covers cloud **deployment models** — Public, Private, Hybrid, and Community — alongside cloud **service models** — IaaS, PaaS, and SaaS — helping you understand the architectural and usage choices available in cloud computing.

📂 **[Explore → Cloud Deployment and Service Models](./Cloud%20Deployment%20and%20Service%20Models/)**

---

## 3. Introduction to Azure

This section provides an overview of Microsoft Azure — its global infrastructure, Regions, Availability Zones, Azure cloud environments, and a high-level introduction to the essential Azure services that power modern cloud applications.

📂 **[Explore → Introduction to Azure](./Introduction%20to%20Azure/)**

---

## 4. Azure Account Structure

This section explains the fundamental structure of an Azure environment. It covers Microsoft Entra tenants, Azure subscriptions, resource groups, Azure resources, and how these components are organized for identity, billing, access control, and resource management.

📂 **[Explore → Azure Account Structure](./Azure%20Account%20Structure/)**

---

## 5. Microsoft Entra ID

This section provides a detailed understanding of Microsoft Entra ID — how identity and access management work in Azure, and how to manage users, groups, roles, authentication, and access to Azure resources.

📂 **[Explore → Microsoft Entra ID](./Microsoft%20Entra%20ID/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 5.1 | [Introduction to Microsoft Entra ID](./Microsoft%20Entra%20ID/Introduction%20to%20Microsoft%20Entra%20ID/) | Entra ID overview, tenants, directories, and foundational identity concepts |
| 5.2 | [Users in Microsoft Entra ID](./Microsoft%20Entra%20ID/Users%20in%20Microsoft%20Entra%20ID/) | Creating and managing users, member users, guest users, and user properties |
| 5.3 | [Groups in Microsoft Entra ID](./Microsoft%20Entra%20ID/Groups%20in%20Microsoft%20Entra%20ID/) | Security groups, Microsoft 365 groups, membership, and group-based access |
| 5.4 | [Microsoft Entra Roles](./Microsoft%20Entra%20ID/Microsoft%20Entra%20Roles/) | Administrative roles, built-in roles, role assignments, and delegated administration |
| 5.5 | [Authentication in Microsoft Entra ID](./Microsoft%20Entra%20ID/Authentication%20in%20Microsoft%20Entra%20ID/) | Authentication concepts, authentication methods, and multi-factor authentication |
| 5.6 | [Azure RBAC](./Microsoft%20Entra%20ID/Azure%20RBAC/) | Azure RBAC, built-in roles, custom roles, and role assignments |
| 5.7 | [RBAC Scopes](./Microsoft%20Entra%20ID/RBAC%20Scopes/) | Management group, subscription, resource group, and resource scopes |
| 5.8 | [Managed Identities](./Microsoft%20Entra%20ID/Managed%20Identities/) | System-assigned and user-assigned managed identities for Azure resource access |
| 5.9 | [Azure Policy and Management Groups](./Microsoft%20Entra%20ID/Azure%20Policy%20and%20Management%20Groups/) | Azure Policy, policy effects, governance, management groups, and subscription-level governance |

---

## 6. Azure Storage Account

This section covers Azure Storage services, storage accounts, Blob Storage, access control, data protection, storage security, and Azure Files.

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 6.1 | [Introduction to Azure Storage](./Azure%20Storage%20Account/Introduction%20to%20Azure%20Storage/) | Azure Storage overview, storage services, storage access, and core concepts |
| 6.2 | [Storage Accounts](./Azure%20Storage%20Account/Storage%20Accounts/) | Storage account types, performance tiers, endpoints, and storage redundancy |
| 6.3 | [Azure Blob Storage](./Azure%20Storage%20Account/Azure%20Blob%20Storage/) | Blob Storage, containers, blob types, blob properties, metadata, and storage tiers |
| 6.4 | [Blob Access Levels](./Azure%20Storage%20Account/Blob%20Access%20Levels/) | Private, Blob, and Container anonymous access levels |
| 6.5 | [Static Website Hosting](./Azure%20Storage%20Account/Static%20Website%20Hosting/) | Hosting static websites using Azure Blob Storage |
| 6.6 | [Blob Versioning and Immutability](./Azure%20Storage%20Account/Blob%20Versioning%20and%20Immutability/) | Blob versioning, previous versions, immutable blobs, retention policies, and legal holds |
| 6.7 | [Blob Soft Delete and Snapshots](./Azure%20Storage%20Account/Blob%20Soft%20Delete%20and%20Snapshots/) | Blob recovery, soft delete, and blob snapshots |
| 6.8 | [Blob Lifecycle Management](./Azure%20Storage%20Account/Blob%20Lifecycle%20Management/) | Automatically moving and deleting blobs based on lifecycle rules |
| 6.9 | [Blob Object Replication](./Azure%20Storage%20Account/Blob%20Object%20Replication/) | Replicating blob objects between storage accounts |
| 6.10 | [SAS and Storage Account Access Keys](./Azure%20Storage%20Account/SAS%20and%20Storage%20Account%20Access%20Keys/) | Shared Access Signatures, access keys, permissions, and controlled storage access |
| 6.11 | [Azure Storage Explorer](./Azure%20Storage%20Account/Azure%20Storage%20Explorer/) | Managing Azure Storage data using Storage Explorer |
| 6.12 | [Storage Account Firewall and Network Access](./Azure%20Storage%20Account/Storage%20Account%20Firewall%20and%20Network%20Access/) | Network access rules, firewall configuration, virtual network rules, and private endpoints |
| 6.13 | [Blob Storage Security and Encryption](./Azure%20Storage%20Account/Blob%20Storage%20Security%20and%20Encryption/) | Storage encryption, Microsoft-managed keys, customer-managed keys, and data protection |
| 6.14 | [Azure Files](./Azure%20Storage%20Account/Azure%20Files/) | Azure Files overview, file shares, storage options, and use cases |
| 6.15 | [Azure File Shares and SMB/NFS](./Azure%20Storage%20Account/Azure%20File%20Shares%20and%20SMB%20NFS/) | File shares, SMB, NFS, access, and Azure Files configuration |
