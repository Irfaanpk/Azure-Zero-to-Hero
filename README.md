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

This section covers Azure Storage, Storage Accounts, Blob Storage, data protection, access control, networking, encryption, and Azure Files.

📂 **[Explore → Azure Storage Account](./Azure%20Storage%20Account/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 6.1 | [Introduction to Azure Storage](./Azure%20Storage%20Account/Introduction%20to%20Azure%20Storage/) | Azure Storage overview, storage services, storage access, and core concepts |
| 6.2 | [Storage Accounts](./Azure%20Storage%20Account/Storage%20Accounts/) | Storage account types, performance options, endpoints, storage redundancy, and minimum TLS version |
| 6.3 | [Azure Blob Storage](./Azure%20Storage%20Account/Azure%20Blob%20Storage/) | Blob Storage, containers, blob types, blob properties, metadata, and storage tiers |
| 6.4 | [Blob Access Levels](./Azure%20Storage%20Account/Blob%20Access%20Levels/) | Private, Blob, and Container anonymous access levels |
| 6.5 | [Static Website Hosting](./Azure%20Storage%20Account/Static%20Website%20Hosting/) | Hosting static websites using Azure Blob Storage |
| 6.6 | [Blob Versioning and Immutability](./Azure%20Storage%20Account/Blob%20Versioning%20and%20Immutability/) | Blob versioning, previous versions, immutable blobs, retention policies, and legal holds |
| 6.7 | [Blob Soft Delete and Snapshots](./Azure%20Storage%20Account/Blob%20Soft%20Delete%20and%20Snapshots/) | Blob recovery, soft delete, and blob snapshots |
| 6.8 | [Blob Lifecycle Management](./Azure%20Storage%20Account/Blob%20Lifecycle%20Management/) | Automatically moving and deleting blobs based on lifecycle rules |
| 6.9 | [Blob Object Replication](./Azure%20Storage%20Account/Blob%20Object%20Replication/) | Replicating block blobs between storage accounts |
| 6.10 | [SAS and Storage Account Access Keys](./Azure%20Storage%20Account/SAS%20and%20Storage%20Account%20Access%20Keys/) | Shared Access Signatures, access keys, permissions, and controlled storage access |
| 6.11 | [Azure Storage Explorer](./Azure%20Storage%20Account/Azure%20Storage%20Explorer/) | Managing Azure Storage data using Azure Storage Explorer |
| 6.12 | [Storage Account Firewall and Network Access](./Azure%20Storage%20Account/Storage%20Account%20Firewall%20and%20Network%20Access/) | Firewall rules, IP rules, virtual network rules, public access, and private endpoints |
| 6.13 | [Blob Storage Security and Encryption](./Azure%20Storage%20Account/Blob%20Storage%20Security%20and%20Encryption/) | Encryption at rest, Microsoft-managed keys, customer-managed keys, infrastructure encryption, and secure transfer |
| 6.14 | [Azure Files](./Azure%20Storage%20Account/Azure%20Files/) | Azure Files overview, file shares, storage options, and use cases |
| 6.15 | [Azure File Shares and SMB NFS](./Azure%20Storage%20Account/Azure%20File%20Shares%20and%20SMB%20NFS/) | File shares, SMB, NFS, authentication, permissions, mounting, and Azure Files configuration |
| 6.16 | [Azure File Sync](./Azure%20Storage%20Account/Azure%20File%20Sync/) | Synchronizing on-premises file servers with Azure file shares and cloud tiering |

---

## 7. Azure Virtual Network

This section covers Azure Virtual Network — VNet architecture, address spaces, subnets, networking interfaces, security, routing, connectivity, private access, and network diagnostics.

📂 **[Explore → Azure Virtual Network](./Azure%20Virtual%20Network/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 7.1 | [Introduction to Azure Virtual Network](./Azure%20Virtual%20Network/Introduction%20to%20Azure%20Virtual%20Network/) | Azure VNet fundamentals, architecture, components, and core networking concepts |
| 7.2 | [VNet Address Space and Subnets](./Azure%20Virtual%20Network/VNet%20Address%20Space%20and%20Subnets/) | Address spaces, CIDR, IPv4, IPv6, subnetting, and IP address planning |
| 7.3 | [Network Interfaces and IP Addressing](./Azure%20Virtual%20Network/Network%20Interfaces%20and%20IP%20Addressing/) | NICs, private and public IP addresses, dynamic and static IPs, and IP configurations |
| 7.4 | [Network Security Groups and Application Security Groups](./Azure%20Virtual%20Network/Network%20Security%20Groups%20and%20Application%20Security%20Groups/) | NSGs, ASGs, inbound and outbound rules, priorities, and network traffic filtering |
| 7.5 | [Route Tables and User-Defined Routes](./Azure%20Virtual%20Network/Route%20Tables%20and%20User-Defined%20Routes/) | System routes, route tables, UDRs, next-hop types, and custom routing |
| 7.6 | [VNet Peering](./Azure%20Virtual%20Network/VNet%20Peering/) | VNet-to-VNet connectivity, regional and global peering, Gateway Transit, and Service Chaining |
| 7.7 | [VPN Gateway](./Azure%20Virtual%20Network/VPN%20Gateway/) | Site-to-Site VPN, Point-to-Site VPN, VPN gateways, connections, and local network gateways |
| 7.8 | [Azure Virtual WAN](./Azure%20Virtual%20Network/Azure%20Virtual%20WAN/) | Virtual WAN, virtual hubs, VNet connections, hub-to-hub connectivity, and centralized networking |
| 7.9 | [Service Endpoints](./Azure%20Virtual%20Network/Service%20Endpoints/) | Secure VNet access to supported Azure services through service endpoints |
| 7.10 | [Private Endpoints and Private Link](./Azure%20Virtual%20Network/Private%20Endpoints%20and%20Private%20Link/) | Private endpoints, private IP access, Private Link, and private connectivity to Azure services |
| 7.11 | [NAT Gateway](./Azure%20Virtual%20Network/NAT%20Gateway/) | Outbound internet connectivity, SNAT, public IP association, and NAT Gateway configuration |
| 7.12 | [Azure Bastion](./Azure%20Virtual%20Network/Azure%20Bastion/) | Secure RDP and SSH access to Azure VMs without exposing management ports to the internet |
| 7.13 | [Azure Network Watcher](./Azure%20Virtual%20Network/Azure%20Network%20Watcher/) | Network diagnostics, IP Flow Verify, Next Hop, Connection Troubleshoot, and network topology |

---

## 8. Azure Virtual Machines

This section covers Azure Virtual Machines — VM fundamentals, lifecycle management, sizing, access, availability, web hosting, load balancing, and scaling.

📂 **[Explore → Azure Virtual Machines](./Azure%20Virtual%20Machines/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 8.1 | [Introduction to Azure Virtual Machines](./Azure%20Virtual%20Machines/Introduction%20to%20Azure%20Virtual%20Machines/) | Azure VM fundamentals, architecture, images, components, lifecycle, and VM creation |
| 8.2 | [VM States and Actions](./Azure%20Virtual%20Machines/VM%20States%20and%20Actions/) | VM states, start, stop, deallocate, restart, redeploy, delete, and billing implications |
| 8.3 | [VM Sizes and Pricing Options](./Azure%20Virtual%20Machines/VM%20Sizes%20and%20Pricing%20Options/) | VM sizes, size families, resizing, Pay-as-you-go, Reservations, Savings Plan, and Spot VMs |
| 8.4 | [SSH Keys and VM Access](./Azure%20Virtual%20Machines/SSH%20Keys%20and%20VM%20Access/) | SSH keys, PuTTY, PuTTYgen, SSH, RDP, authentication, Bastion, and Run Command |
| 8.5 | [VM Availability and Placement](./Azure%20Virtual%20Machines/VM%20Availability%20and%20Placement/) | Availability Sets, Fault Domains, Update Domains, Availability Zones, and Dedicated Host |
| 8.6 | [Web Servers on Azure VMs](./Azure%20Virtual%20Machines/Web%20Servers%20on%20Azure%20VMs/) | Installing and configuring Apache, Nginx, and IIS on Azure Virtual Machines |
| 8.7 | [Multiple Websites on a Single VM](./Azure%20Virtual%20Machines/Multiple%20Websites%20on%20a%20Single%20VM/) | Hosting multiple websites using hostnames, ports, virtual hosts, and web-server configuration |
| 8.8 | [VM Extensions and Custom Script](./Azure%20Virtual%20Machines/VM%20Extensions%20and%20Custom%20Script/) | VM Extensions, Custom Script Extension, Run Command, and automated software installation |
| 8.9 | [Azure Load Balancer](./Azure%20Virtual%20Machines/Azure%20Load%20Balancer/) | Load balancing, frontend IP, backend pools, health probes, rules, inbound NAT, and outbound rules |
| 8.10 | [Azure Application Gateway](./Azure%20Virtual%20Machines/Azure%20Application%20Gateway/) | Layer 7 load balancing, listeners, routing rules, backend pools, health probes, HTTPS, and WAF basics |
| 8.11 | [VM Scale Sets](./Azure%20Virtual%20Machines/VM%20Scale%20Sets/) | VMSS architecture, instances, scaling, instance management, and flexible orchestration |
| 8.12 | [VMSS Load Balancing & Autoscaling](./Azure%20Virtual%20Machines/VMSS%20Load%20Balancing%20and%20Autoscaling/) | VMSS load balancing, health probes, autoscaling, scale-out, scale-in, alerts, Action Groups, and email notifications |
