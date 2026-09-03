# 11.2 Azure Container Registry

## What is Azure Container Registry?

**Azure Container Registry (ACR)** is a private managed registry service in Azure used to store and manage container images and other OCI artifacts.

```text
Developer
    │
    ▼
Build Container Image
    │
    ▼
Azure Container Registry
    │
    ├── Repository 1
    │      ├── image:v1
    │      └── image:v2
    │
    └── Repository 2
           └── image:v1
```

---

# Why Use Azure Container Registry?

ACR provides:

- Private container image storage
- Image version management
- Authentication and access control
- Integration with Azure container services
- Image push and pull operations
- Geo-replication for supported configurations

---

# Registry

A **registry** is the top-level ACR resource that stores container repositories and images.

Example:

```text
myregistry.azurecr.io
```

The registry endpoint is used when pushing and pulling images.

---

# Repository

A **repository** stores related versions of a container image.

Example:

```text
myregistry.azurecr.io/nginx
```

A repository can contain multiple image versions.

```text
nginx
 ├── v1
 ├── v2
 └── v3
```

---

# Image Tags

Tags identify different versions of an image.

Example:

```text
myregistry.azurecr.io/myapp:v1
myregistry.azurecr.io/myapp:v2
myregistry.azurecr.io/myapp:latest
```

Using meaningful version tags makes image management easier.

---

# Push and Pull Container Images

### Push

Uploading an image from a local environment to ACR:

```text
Local Docker Image
       ↓
       Push
       ↓
Azure Container Registry
```

### Pull

Downloading an image from ACR:

```text
Azure Container Registry
       ↓
       Pull
       ↓
Container Service
```

---

# Authentication and Access

ACR supports authentication and access control for container image operations.

Access can be managed using Azure identity and role-based access control.

Common roles include:

- AcrPull
- AcrPush
- Owner
- Contributor

### AcrPull

Allows pulling images from the registry.

### AcrPush

Allows pushing and pulling images.

---

# ACR and Azure Container Services

ACR can provide private container images to Azure container services.

Example:

```text
                    Azure Container Registry
                              │
                         Private Image
                              │
                 ┌────────────┴────────────┐
                 │                         │
                ACI                 Azure Container Apps
```

This allows applications to use container images stored privately in Azure.

---

# ACR Image Workflow

A typical workflow is:

```text
Create Application
       ↓
Create Docker Image
       ↓
Create Azure Container Registry
       ↓
Login to ACR
       ↓
Tag Image
       ↓
Push Image
       ↓
Image Stored in ACR
       ↓
Deploy Image to Container Service
```

---

# Practical Lab

## Lab: Create ACR and Push a Container Image

### Objective

Create an Azure Container Registry, build or use a container image, push it to ACR, and verify the stored image.

### Step 1: Create Resource Group

```bash
az group create \
  --name rg-acr \
  --location eastus
```

### Step 2: Create Azure Container Registry

```bash
az acr create \
  --resource-group rg-acr \
  --name <unique-registry-name> \
  --sku Basic
```

> The registry name must be globally unique and contain only valid characters.

---

### Step 3: Login to ACR

```bash
az acr login \
  --name <unique-registry-name>
```

---

### Step 4: Get the Login Server

```bash
az acr show \
  --name <unique-registry-name> \
  --query loginServer \
  --output tsv
```

Example:

```text
myregistry.azurecr.io
```

---

### Step 5: Pull a Sample Image

```bash
docker pull nginx
```

---

### Step 6: Tag the Image

```bash
docker tag nginx <registry-name>.azurecr.io/nginx:v1
```

---

### Step 7: Push the Image

```bash
docker push <registry-name>.azurecr.io/nginx:v1
```

The image is now stored in ACR.

---

### Step 8: Verify the Repository

```bash
az acr repository list \
  --name <registry-name> \
  --output table
```

---

### Step 9: View Image Tags

```bash
az acr repository show-tags \
  --name <registry-name> \
  --repository nginx \
  --output table
```

You should see:

```text
v1
```

---

### Step 10: Clean Up

```bash
az group delete \
  --name rg-acr
```

---

# Key Points

- Azure Container Registry is a managed private container registry.
- A registry contains repositories.
- Repositories contain container images and their versions.
- Tags identify image versions.
- `docker push` uploads images to ACR.
- `docker pull` downloads images from a registry.
- `AcrPull` provides image pull access.
- `AcrPush` provides image push and pull access.
- ACR integrates with Azure Container Instances and Azure Container Apps.
- ACR can be used to store private container images for Azure workloads.
