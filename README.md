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

---

## 9. Azure Managed Disks

This section covers Azure Managed Disks — disk types, performance, VM disk management, permanent disk mounting, and disk encryption using customer-managed keys.

📂 **[Explore → Azure Managed Disks](./Azure%20Managed%20Disks/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 9.1 | [Introduction to Azure Managed Disks](./Azure%20Managed%20Disks/Introduction%20to%20Azure%20Managed%20Disks/) | Managed disks, OS disks, data disks, temporary disks, disk types, IOPS, throughput, and disk selection |
| 9.2 | [Managing VM Disks](./Azure%20Managed%20Disks/Managing%20VM%20Disks/) | Attaching, detaching, initializing, partitioning, formatting, mounting, resizing, and permanently mounting Linux disks |
| 9.3 | [Disk Encryption Set](./Azure%20Managed%20Disks/Disk%20Encryption%20Set/) | Disk Encryption Sets, Azure Key Vault, customer-managed keys, managed identities, key rotation, and encryption at host |

---

## 10. Azure App Service

This section covers Azure App Service — web applications, App Service Plans, configuration, scaling, networking, security, and deployment slots.

📂 **[Explore → Azure App Service](./Azure%20App%20Service/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 10.1 | [Introduction to Azure App Service](./Azure%20App%20Service/Introduction%20to%20Azure%20App%20Service/) | App Service, App Service Plans, Web Apps, application stacks, configuration, scaling, networking, security, and deployment |
| 10.2 | [Deployment Slots](./Azure%20App%20Service/Deployment%20Slots/) | Production and staging slots, slot settings, deployment, testing, swapping, swap with preview, and rollback |

---

## 11. Azure Container Services

This section covers Azure container services — container deployment, container images, Azure Container Instances, Azure Container Registry, and containerized application hosting.

📂 **[Explore → Azure Container Services](./Azure%20Container%20Services/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 11.1 | [Azure Container Instances](./Azure%20Container%20Services/Azure%20Container%20Instances/) | Container basics, container images, container groups, networking, environment variables, restart policies, and container management |
| 11.2 | [Azure Container Registry](./Azure%20Container%20Services/Azure%20Container%20Registry/) | Container registries, repositories, images, tags, pushing and pulling images, authentication, and integration with Azure container services |
| 11.3 | [Azure Container Apps](./Azure%20Container%20Services/Azure%20Container%20Apps/) | Container Apps, environments, revisions, ingress, scaling, environment variables, and containerized application hosting |

---

## 12. Azure Database Services

This section covers Azure database services — managed relational and NoSQL databases, connectivity, configuration, scaling, and basic database management.

📂 **[Explore → Azure Database Services](./Azure%20Database%20Services/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 12.1 | [Azure SQL Database](./Azure%20Database%20Services/Azure%20SQL%20Database/) | Azure SQL Database, databases and servers, deployment options, service tiers, connectivity, firewall, authentication, scaling, and backup basics |
| 12.2 | [Azure Database for MySQL](./Azure%20Database%20Services/Azure%20Database%20for%20MySQL/) | MySQL Flexible Server, connectivity, authentication, firewall, networking, scaling, and database management |
| 12.3 | [Azure Cosmos DB](./Azure%20Database%20Services/Azure%20Cosmos%20DB/) | Cosmos DB, NoSQL concepts, databases, containers, partition keys, request units, and global distribution |

---

## 13. Azure DNS and Traffic Manager

This section covers Azure DNS and Traffic Manager — DNS zones, DNS records, private DNS, DNS resolution, traffic routing, endpoints, routing methods, health probes, and failover.

📂 **[Explore → Azure DNS and Traffic Manager](./Azure%20DNS%20and%20Traffic%20Manager/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 13.1 | [Azure DNS](./Azure%20DNS%20and%20Traffic%20Manager/Azure%20DNS/) | DNS zones, DNS records, public and private DNS zones, DNS resolution, and VNet integration |
| 13.2 | [Azure Traffic Manager](./Azure%20DNS%20and%20Traffic%20Manager/Azure%20Traffic%20Manager/) | Traffic Manager profiles, endpoints, routing methods, health probes, and traffic failover |
| 13.3 | [Azure DNS and Traffic Manager Comparison](./Azure%20DNS%20and%20Traffic%20Manager/Azure%20DNS%20and%20Traffic%20Manager%20Comparison/) | Azure DNS vs Traffic Manager, DNS hosting vs traffic routing, and when to use each service |

---

## 14. Azure Front Door and CDN

This section covers Azure Front Door and Azure CDN — global application delivery, content caching, routing, origins, endpoints, caching, and content delivery.

📂 **[Explore → Azure Front Door and CDN](./Azure%20Front%20Door%20and%20CDN/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 14.1 | [Azure Front Door](./Azure%20Front%20Door%20and%20CDN/Azure%20Front%20Door/) | Azure Front Door, profiles, endpoints, origins, origin groups, routing, health probes, caching, and global application delivery |
| 14.2 | [Azure CDN](./Azure%20Front%20Door%20and%20CDN/Azure%20CDN/) | CDN fundamentals, profiles, endpoints, origins, caching, cache rules, content delivery, and cache invalidation |
| 14.3 | [Azure Front Door vs Azure CDN](./Azure%20Front%20Door%20and%20CDN/Azure%20Front%20Door%20vs%20Azure%20CDN/) | Differences between Front Door and CDN, use cases, architecture, routing, caching, and when to use each |

---

## 15. Azure Monitor

This section covers Azure Monitor — metrics, logs, Log Analytics, diagnostic settings, data collection, alerts, action groups, insights, and application monitoring.

📂 **[Explore → Azure Monitor](./Azure%20Monitor/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 15.1 | [Introduction to Azure Monitor](./Azure%20Monitor/Introduction%20to%20Azure%20Monitor/) | Azure Monitor overview, monitoring architecture, metrics, logs, and monitoring data |
| 15.2 | [Azure Monitor Metrics and Logs](./Azure%20Monitor/Azure%20Monitor%20Metrics%20and%20Logs/) | Metrics, platform logs, resource logs, activity logs, and monitoring data types |
| 15.3 | [Log Analytics and KQL](./Azure%20Monitor/Log%20Analytics%20and%20KQL/) | Log Analytics workspaces, log queries, KQL basics, filtering, sorting, aggregation, and troubleshooting |
| 15.4 | [Diagnostic Settings](./Azure%20Monitor/Diagnostic%20Settings/) | Resource logs, diagnostic settings, and sending monitoring data to Log Analytics, Storage Accounts, and Event Hubs |
| 15.5 | [Azure Monitor Agent and Data Collection Rules](./Azure%20Monitor/Azure%20Monitor%20Agent%20and%20Data%20Collection%20Rules/) | Azure Monitor Agent, Data Collection Rules, data sources, and collecting guest OS data |
| 15.6 | [Azure Monitor Alerts and Action Groups](./Azure%20Monitor/Azure%20Monitor%20Alerts%20and%20Action%20Groups/) | Alert rules, metric and log alerts, alert conditions, Action Groups, and notifications |
| 15.7 | [Alert Processing Rules](./Azure%20Monitor/Alert%20Processing%20Rules/) | Suppressing, modifying, and controlling alert notifications using processing rules |
| 15.8 | [Azure Monitor Insights](./Azure%20Monitor/Azure%20Monitor%20Insights/) | VM Insights, Storage Insights, Network Insights, performance monitoring, and resource health information |
| 15.9 | [Application Insights](./Azure%20Monitor/Application%20Insights/) | Application monitoring, telemetry, availability, performance, failures, and application diagnostics |

---

## 16. Azure Backup and Site Recovery

This section covers Azure Backup and Azure Site Recovery — backup vaults, backup policies, backup and restore operations, disaster recovery, replication, and failover.

📂 **[Explore → Azure Backup and Site Recovery](./Azure%20Backup%20and%20Site%20Recovery/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 16.1 | [Azure Backup](./Azure%20Backup%20and%20Site%20Recovery/Azure%20Backup/) | Azure Backup overview, supported workloads, backup architecture, Recovery Services vaults, and Backup vaults |
| 16.2 | [Backup Policies](./Azure%20Backup%20and%20Site%20Recovery/Backup%20Policies/) | Backup policies, schedules, retention, recovery points, and backup configuration |
| 16.3 | [Backup and Restore](./Azure%20Backup%20and%20Site%20Recovery/Backup%20and%20Restore/) | Creating backups, on-demand backups, restore operations, restore options, and recovery |
| 16.4 | [Azure Site Recovery](./Azure%20Backup%20and%20Site%20Recovery/Azure%20Site%20Recovery/) | Disaster recovery, replication, recovery plans, failover, test failover, and failback |
| 16.5 | [Backup Monitoring and Alerts](./Azure%20Backup%20and%20Site%20Recovery/Backup%20Monitoring%20and%20Alerts/) | Backup monitoring, backup jobs, backup health, alerts, and backup reports |

---

## 17. Azure Resource Manager and Infrastructure as Code

This section covers Azure Resource Manager (ARM) and infrastructure as code — resource deployments, ARM templates, Bicep, and declarative Azure infrastructure management.

📂 **[Explore → Azure Resource Manager and Infrastructure as Code](./Azure%20Resource%20Manager%20and%20Infrastructure%20as%20Code/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 17.1 | [Azure Resource Manager](./Azure%20Resource%20Manager%20and%20Infrastructure%20as%20Code/Azure%20Resource%20Manager/) | ARM architecture, resource providers, resource types, deployments, templates, and resource management |
| 17.2 | [ARM Templates](./Azure%20Resource%20Manager%20and%20Infrastructure%20as%20Code/ARM%20Templates/) | ARM template structure, parameters, variables, resources, outputs, expressions, and deployments |
| 17.3 | [Bicep](./Azure%20Resource%20Manager%20and%20Infrastructure%20as%20Code/Bicep/) | Bicep syntax, resources, parameters, variables, modules, outputs, and Azure infrastructure deployment |
| 17.4 | [ARM Templates vs Bicep](./Azure%20Resource%20Manager%20and%20Infrastructure%20as%20Code/ARM%20Templates%20vs%20Bicep/) | Differences between ARM templates and Bicep and when to use each |

---

## 18. Azure Functions

This section covers Azure Functions — serverless computing, function apps, triggers, bindings, hosting, configuration, and scaling.

📂 **[Explore → Azure Functions](./Azure%20Functions/)**

| # | Sub-Topic | Description |
|---|-----------|-------------|
| 18.1 | [Introduction to Azure Functions](./Azure%20Functions/Introduction%20to%20Azure%20Functions/) | Serverless computing, Function Apps, functions, triggers, bindings, and core concepts |
| 18.2 | [Function Triggers and Bindings](./Azure%20Functions/Function%20Triggers%20and%20Bindings/) | HTTP, Timer, Blob, Queue triggers, input/output bindings, and event-driven execution |
| 18.3 | [Function App Configuration and Hosting](./Azure%20Functions/Function%20App%20Configuration%20and%20Hosting/) | Application settings, runtime configuration, hosting plans, scaling, and networking |
| 18.4 | [Azure Functions Deployment](./Azure%20Functions/Azure%20Functions%20Deployment/) | Deploying functions, deployment methods, configuration, and basic deployment management |

---

## 🛠️ Prerequisites

- Basic understanding of cloud computing concepts
- Basic understanding of networking concepts such as VNet, Subnets, NSGs, and IP addressing
- Familiarity with Linux commands and terminal usage
- Basic understanding of virtual machines and SSH/RDP access
- An active Microsoft Azure account with permission to create and manage Azure resources
- Basic knowledge of JSON and YAML is helpful for ARM templates and Bicep
- Basic Git and GitHub knowledge is recommended

Before diving in, make sure you have:

| **Requirement** | **Details** |
| ---------------- | ----------- |
| Azure Account | Azure account with an active subscription |
| Azure Portal | Access to [Azure Portal](https://portal.azure.com/) |
| Azure CLI | Installed and configured (`az login`) |
| Basic Linux | Comfortable with terminal commands |
| Git | Installed for cloning and managing the repository |
| GitHub Account | Required for cloning and contributing to the repository |
| Text Editor | VS Code or similar for editing configuration, JSON, and Bicep files |

---

# 🚦 Getting Started

```bash
# Clone this repository
git clone https://github.com/Irfaanpk/Azure-Zero-to-Hero.git

# Navigate into the project
cd Azure-Zero-to-Hero

# Start with the first section
cd "Introduction to cloud computing"
```

Each folder contains its own `README.md` with step-by-step explanations, diagrams, Azure Portal walkthroughs, CLI commands, and hands-on labs where applicable.

Work through the sections in order for the best learning experience.

---

## 🤝 Contributing

Contributions are welcome!

If you have suggestions for improvements, new examples, better explanations, or find any issues, feel free to:

- Open an issue
- Submit a pull request
- Improve existing documentation
- Add useful examples or diagrams

Please keep contributions beginner-friendly, accurate, and consistent with the structure of this repository.

---

**Happy Cloud Building! ☁️**

*If this repo helped you, please consider giving it a ⭐*



