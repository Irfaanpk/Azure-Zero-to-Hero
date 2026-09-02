# What is Azure?

<div align="center">

<img src="../assets/Microsoft-Azure-Logo.png" alt="Microsoft Azure Logo" width="300" />

</div>

Microsoft Azure is a comprehensive cloud computing platform provided by Microsoft. It offers a wide range of cloud services from data centers globally, including computing power, storage, databases, networking, analytics, artificial intelligence, machine learning, security, and more.

**Key Benefits:**

- Pay-as-you-go pricing model
- Scalability and flexibility
- Global infrastructure
- High reliability and security
- No upfront infrastructure costs
- Support for cloud, hybrid, and edge environments

---

## Core Azure Services

Azure offers services across multiple categories. Here are the essential ones:

**Compute Services:**

- **Azure Virtual Machines:** Virtual servers in the cloud that you can configure and scale
- **Azure Functions:** Serverless computing that runs code in response to events without managing servers
- **Azure App Service:** Platform for deploying and scaling web applications automatically

**Storage Services:**

- **Azure Blob Storage:** Object storage for any type of data with high durability and scalability
- **Azure Managed Disks:** Persistent block storage for Azure Virtual Machines
- **Azure Archive Storage:** Low-cost storage for data that is rarely accessed
- **Azure Files:** Scalable managed file storage accessible through SMB and NFS

**Database Services:**

- **Azure SQL Database:** Fully managed relational database service supporting SQL Server workloads
- **Azure Cosmos DB:** Fully managed NoSQL database with low latency and global distribution
- **Azure Cache for Redis:** In-memory caching service for improving application performance
- **Azure Synapse Analytics:** Analytics service for data warehousing, big data, and analytics

**Networking Services:**

- **Azure Virtual Network (VNet):** Isolated network environment within Azure
- **Azure DNS:** Scalable DNS hosting service for managing domain name resolution
- **Azure Front Door:** Global application delivery service for fast content distribution
- **Azure ExpressRoute:** Dedicated private network connection from your premises to Azure

**Security and Identity:**

- **Microsoft Entra ID:** Cloud-based identity and access management service for controlling access to Azure resources
- **Azure Key Vault:** Create and manage encryption keys, secrets, and certificates
- **Azure DDoS Protection:** Protection against distributed denial-of-service attacks
- **Azure Web Application Firewall (WAF):** Protect web applications from common web-based attacks

**Management and Monitoring:**

- **Azure Monitor:** Monitoring and observability service for Azure resources and applications
- **Azure Resource Manager (ARM):** Management layer for deploying and managing Azure resources
- **Azure Activity Log:** Log and monitor subscription-level account activity
- **Azure Automation:** Automate repetitive operational tasks and resource management

---

## Azure Cloud Partitions / Cloud Instances

Azure provides separate cloud environments for different geographic, regulatory, and compliance requirements. These environments are isolated from each other and can have different portals, endpoints, services, and compliance requirements.

### 1. **Azure Public Cloud**

The primary Azure cloud environment serving most commercial customers worldwide.

- **Cloud Name:** `AzureCloud`
- **Regions:** Includes standard commercial Azure regions worldwide
- **Examples:** East US, West Europe, Southeast Asia
- **Portal:** `portal.azure.com`
- **Endpoint Format:** Service-specific endpoints using Azure domains
- **Use Case:** General commercial and enterprise workloads globally

### 2. **Azure Government**

A separate Azure cloud environment designed for US government agencies, organizations, and their partners.

- **Cloud Name:** `AzureUSGovernment`
- **Regions:** Government regions such as US Gov Virginia and US Gov Texas
- **Portal:** `portal.azure.us`
- **Use Case:** US government and regulated workloads
- **Key Features:**
  - Designed for US government requirements
  - Supports government-specific compliance requirements
  - Physically isolated from Azure Public Cloud
  - Operated by screened US citizens
  - Separate identity and account environment
  - Service availability can differ from the Public Cloud

### 3. **Microsoft Azure operated by 21Vianet**

A separate Azure cloud environment available in China and operated by 21Vianet.

- **Cloud Name:** `AzureChinaCloud`
- **Regions:** China North, China East, and other China regions
- **Portal:** `portal.azure.cn`
- **Operator:** 21Vianet
- **Use Case:** Azure services for customers operating in China
- **Key Features:**
  - Separate from Azure Public Cloud
  - Operated by 21Vianet
  - Separate account and subscription environment
  - Subject to Chinese regulatory requirements
  - Service availability can differ from the Public Cloud

### 4. **Azure Germany — Historical Cloud**

Azure Germany was a specialized Azure environment designed for German data residency and regulatory requirements.

- **Status:** Historical / Retired
- **Use Case:** Previously used for German regulatory and data residency requirements
- **Important:** Azure Germany was retired and should not be considered a current Azure cloud environment.

**Important Notes About Azure Cloud Environments:**

- Azure Public Cloud, Azure Government, and Azure operated by 21Vianet are separate cloud environments
- Resources and subscriptions are specific to their respective cloud environment
- Identity and account management can differ between environments
- Services and features may vary between cloud environments
- Endpoints and Azure portals can differ between environments
- Compliance and regulatory requirements can differ between environments
- Azure Government and Azure China are isolated from the Azure Public Cloud

### Azure CLI Cloud Environments

Azure CLI supports different cloud environments:

```bash
# List available Azure clouds
az cloud list

# Show the current cloud environment
az cloud show

# Use Azure Public Cloud
az cloud set --name AzureCloud

# Use Azure Government
az cloud set --name AzureUSGovernment

# Use Azure China
az cloud set --name AzureChinaCloud
```

---

## Core Azure Concepts (In-Depth)

### Regions

An **Azure Region** is a geographical area around the world that contains one or more Azure datacenters connected through a dedicated, low-latency network.

**Key Characteristics:**

- **Geographic Location:** Each Azure region is located within a specific geographic area
- **Resource Deployment:** Azure resources are deployed into specific regions
- **Service Availability:** Not all Azure services are available in every region
- **Compliance:** Regions help organizations meet data residency and regulatory requirements
- **Latency:** Choosing a region close to users can improve application performance

**Examples:**

- East US
- West Europe
- Central India
- Southeast Asia

**Factors for Choosing an Azure Region:**

1. **Latency:** Choose a region close to your users for better performance
2. **Cost:** Azure pricing can vary between regions
3. **Compliance:** Regulatory requirements may require data to remain within specific geographic boundaries
4. **Service Availability:** Some Azure services and features may be available in selected regions first
5. **Disaster Recovery:** Use multiple regions to improve availability and disaster recovery

---

### Availability Zones

An **Availability Zone** is a physically separate location within an Azure region. Each availability zone contains one or more datacenters with independent power, cooling, and networking.

**Key Characteristics:**

- **Physical Separation:** Availability Zones are physically separated within an Azure region
- **Low Latency:** Zones within a region are connected through a high-performance, low-latency network
- **Independent Infrastructure:** Each zone has independent power, cooling, and networking
- **Fault Isolation:** A failure in one zone is designed to have limited impact on resources in other zones

**Naming Convention:**

Azure Availability Zones are generally identified using numbers:

- Zone 1
- Zone 2
- Zone 3

**Best Practices:**

1. **Distribute Resources:** Deploy critical applications across multiple availability zones
2. **Load Balancing:** Use Azure Load Balancer or Application Gateway across zones
3. **Virtual Machines:** Deploy VM instances across multiple zones for high availability
4. **Database Redundancy:** Use zone-redundant database configurations where supported
5. **Data Redundancy:** Use Azure storage redundancy options such as ZRS when appropriate

**Use Cases:**

- **High Availability:** If one availability zone fails, applications can continue running in another zone
- **Disaster Protection:** Protect applications from datacenter-level failures
- **Load Distribution:** Distribute application workloads across multiple zones
- **Compliance:** Meet requirements for infrastructure redundancy

---

### Azure Edge Locations / Points of Presence

Azure provides a global network of **Points of Presence (PoPs)** and edge locations that bring Azure services and content closer to end users.

These locations are primarily used by services such as **Azure Front Door**, **Azure CDN**, and other Azure networking services to improve application performance and reduce latency.

**Key Characteristics:**

- **Global Distribution:** Azure has a large global network of edge locations and Points of Presence
- **Content Caching:** Content can be cached closer to users
- **Low Latency:** Requests can enter Microsoft's global network closer to the end user
- **Global Network:** Azure uses Microsoft's global network to connect users to applications and services

**Services Using Azure Edge / Global Network:**

1. **Azure Front Door:** Global application delivery service
   - Provides global HTTP/HTTPS application delivery
   - Uses Microsoft's global edge network
   - Provides caching and acceleration capabilities
   - Supports global traffic routing

2. **Azure CDN:** Content Delivery Network
   - Caches static content closer to users
   - Reduces latency
   - Improves website and application performance
   - Reduces load on origin servers

3. **Azure Traffic Manager:** DNS-based traffic routing service
   - Routes users to appropriate application endpoints
   - Supports geographic and performance-based routing
   - Helps improve application availability

4. **Azure Web Application Firewall (WAF):**
   - Helps protect web applications from common attacks
   - Can be integrated with services such as Azure Front Door and Application Gateway
   - Filters malicious HTTP/HTTPS traffic

**Benefits:**

- **Reduced Latency:** Users can connect through Microsoft's global network closer to their location
- **Improved Performance:** Faster delivery of applications and content
- **Global Reach:** Serve users around the world without deploying application infrastructure in every location
- **High Availability:** Global traffic routing can help direct users to healthy endpoints
- **Security:** Azure networking and security services can help protect applications at the edge

---

### Comparison Table

| **Feature** | **Region** | **Availability Zone** | **Edge Location / PoP** |
|---|---|---|---|
| **Purpose** | Deploy Azure resources | High availability and fault isolation | Global content and application delivery |
| **Scope** | Geographic area | Isolated datacenter location within a region | Global network location |
| **Resources** | Hosts Azure services and resources | Hosts zonal resources | Primarily handles network traffic and content delivery |
| **Connectivity** | Connected through Azure/Microsoft global network | Low-latency connection to other zones | Connected through Microsoft's global network |
| **Main Use** | Resource deployment | High availability | Low latency and global delivery |

---

## Summary

Azure provides a globally distributed cloud infrastructure that enables organizations to deploy applications with high availability, low latency, scalability, and compliance with regulatory requirements.

Understanding the relationship between **Regions, Availability Zones, and Edge Locations / Points of Presence** is fundamental to designing reliable and highly available Azure architectures.

**Key Takeaways:**

- Choose Azure regions based on latency, compliance, cost, and service availability
- Use Availability Zones for high availability and fault isolation
- Use multiple regions for disaster recovery and geographic redundancy
- Use Azure Front Door and other edge services for global application delivery
- Understand Azure cloud environments such as Public Cloud, Government, and China
- Azure services and features can vary by region

---

# Azure Regions List

## Azure Public Cloud

### North America

- **East US** - Virginia
- **East US 2** - Virginia
- **West US** - California
- **West US 2** - Washington
- **West US 3** - Arizona
- **Central US** - Iowa
- **North Central US** - Illinois
- **South Central US** - Texas
- **West Central US** - Wyoming
- **Canada Central** - Toronto
- **Canada East** - Quebec
- **Mexico Central** - Queretaro

### South America

- **Brazil South** - Sao Paulo
- **Brazil Southeast** - Rio de Janeiro

### Europe

- **North Europe** - Ireland
- **West Europe** - Netherlands
- **UK South** - London
- **UK West** - Cardiff
- **France Central** - Paris
- **France South** - Marseille
- **Germany West Central** - Frankfurt
- **Germany North** - Berlin
- **Switzerland North** - Zurich
- **Switzerland West** - Geneva
- **Sweden Central** - Gävle
- **Sweden South** - Staffanstorp
- **Norway East** - Oslo
- **Norway West** - Stavanger
- **Italy North** - Milan
- **Poland Central** - Warsaw
- **Spain Central** - Madrid

### Asia Pacific

- **Central India** - Pune
- **South India** - Chennai
- **West India** - Mumbai
- **East Asia** - Hong Kong
- **Southeast Asia** - Singapore
- **East Japan** - Tokyo
- **West Japan** - Osaka
- **Korea Central** - Seoul
- **Korea South** - Busan
- **Australia East** - New South Wales
- **Australia Southeast** - Victoria
- **Australia Central** - Canberra
- **Australia Central 2** - Canberra
- **New Zealand North** - Auckland
- **Malaysia West** - Kuala Lumpur
- **Indonesia Central** - Jakarta

### Middle East

- **UAE North** - Dubai
- **UAE Central** - Abu Dhabi
- **Qatar Central** - Doha
- **Israel Central** - Israel
- **Saudi Arabia East** - Dammam

### Africa

- **South Africa North** - Johannesburg
- **South Africa West** - Cape Town

---

## Azure Government

Azure Government provides isolated cloud environments designed for US government agencies and organizations.

- **US Gov Virginia**
- **US Gov Texas**
- **US DoD East**
- **US DoD Central**
- **US Gov Arizona**
- **US Gov Iowa**

---

## Microsoft Azure operated by 21Vianet

Azure operated by 21Vianet provides Azure services within China through a separate cloud environment.

- **China North**
- **China East**
- Additional China regions operated by 21Vianet

---

## Announced / Upcoming Regions

Microsoft regularly announces new Azure regions and expands its global infrastructure.

New regions can be introduced in different countries and geographic areas to provide additional capacity, lower latency, and support local regulatory requirements.

---

## Quick Reference Summary

**Azure Cloud Environments:**

- **Azure Public Cloud:** Global commercial Azure environment
- **Azure Government:** Isolated cloud for US government requirements
- **Azure China:** Azure operated by 21Vianet

**Core Infrastructure Concepts:**

- **Region:** Geographic area containing Azure datacenters
- **Availability Zone:** Physically separated datacenter location within a region
- **Edge Location / PoP:** Global network location used to bring applications and content closer to users

---

## Notes

- Region availability and service offerings may vary
- New Azure regions are added regularly
- Not all Azure services are available in every region
- Availability Zone support varies by region
- Pricing varies by region
- Some Azure services have region-specific limitations
- Azure Government and Azure China are separate cloud environments

**To check the most current list of Azure regions**, you can:

1. Visit the Azure Global Infrastructure documentation
2. Use Azure CLI:

```bash
az account list-locations
```

---

# Connecting Your Local Machine to Azure (Installing Azure CLI)

## Prerequisites

Before starting, ensure you have:

- An active Azure account
- An active Azure subscription
- Administrator access to your local machine
- Basic command line knowledge
- Active internet connection

---

## Step 1: Install Azure CLI

Azure CLI is a command-line tool used to create, manage, and interact with Azure resources from your local machine.

### For Windows

**Using MSI Installer:**

1. Visit the [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows)
2. Download the Windows MSI installer
3. Run the installer and complete the installation
4. Open **Command Prompt** or **PowerShell**
5. Verify the installation:

```bash
az --version
```

---

## Step 2: Sign in to Azure

After installing Azure CLI, authenticate your local machine with your Azure account.

### Using `az login`

Run the following command:

```bash
az login

