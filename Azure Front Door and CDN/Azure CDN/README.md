# 14.2 Azure CDN

## What is Azure CDN?

**Azure Content Delivery Network (CDN)** is a content delivery service that caches content at geographically distributed edge locations and delivers it to users from a location closer to them.

```text
User
  │
  ▼
Azure CDN Edge Location
  │
  ├── Cache Hit → Return Content
  │
  └── Cache Miss
          │
          ▼
        Origin
```

---

# Why Use Azure CDN?

Azure CDN can provide:

- Faster content delivery
- Reduced latency
- Reduced origin server load
- Global content distribution
- Content caching
- Improved application performance
- Static content delivery
- Cache management

---

# How Azure CDN Works

The basic flow is:

```text
                    User
                      │
                      ▼
               CDN Edge Location
                      │
                ┌─────┴─────┐
                │           │
            Cache Hit    Cache Miss
                │           │
                ▼           ▼
             Content      Origin
                            │
                            ▼
                       Application
```

### Cache Hit

If the requested content is already available at the edge:

```text
User
  │
  ▼
CDN Edge
  │
  ▼
Cached Content
```

The request can be served without contacting the origin.

### Cache Miss

If the content is not cached:

```text
User
  │
  ▼
CDN Edge
  │
  ▼
Origin
  │
  ▼
Content
  │
  ▼
CDN Edge
  │
  ▼
User
```

The CDN retrieves the content from the origin and can cache it for subsequent requests.

---

# CDN Profile

A **CDN profile** is the main resource that contains CDN configuration.

```text
CDN Profile
    │
    └── CDN Endpoint
            │
            └── Origin
```

---

# CDN Endpoint

A **CDN endpoint** provides the URL through which cached content can be accessed.

Example:

```text
https://example.azureedge.net
```

An endpoint is associated with an origin.

```text
CDN Endpoint
      │
      ▼
   Origin
```

---

# Origin

An **origin** is the source from which CDN retrieves content.

Examples:

- Azure Storage
- Azure App Service
- Public web server

Example:

```text
Azure CDN
    │
    ▼
Azure Storage Account
    │
    ▼
Static Content
```

---

# CDN with Azure Storage

Azure CDN can be used to deliver static content stored in Azure Blob Storage.

```text
User
  │
  ▼
Azure CDN
  │
  ▼
Blob Storage
  │
  ▼
Static Files
```

Examples of content:

- Images
- CSS
- JavaScript
- Videos
- Documents
- Static web content

---

# CDN Caching

Caching stores frequently requested content closer to users.

Example:

```text
Origin
  │
  ▼
CDN Edge Location
  │
  ├── User 1
  ├── User 2
  └── User 3
```

Instead of every user requesting the content from the origin, the CDN can serve cached content.

---

# Cache-Control

HTTP cache headers can control how content is cached.

Example:

```text
Cache-Control: max-age=3600
```

This indicates that the content can be cached for the specified period.

Caching behavior depends on the CDN configuration and HTTP caching headers.

---

# Cache Hit vs Cache Miss

| Cache Hit | Cache Miss |
|---|---|
| Content exists in CDN cache | Content is not in cache |
| Served from edge | Retrieved from origin |
| Lower latency | Higher latency |
| Origin may not receive request | Origin receives request |
| Faster response | Requires origin request |

---

# Cache Invalidation

**Cache invalidation** removes cached content so that updated content can be retrieved from the origin.

Example:

```text
Old Content
    │
    ▼
CDN Cache
    │
    X
    │
Purge Cache
    │
    ▼
New Content from Origin
```

This is useful when:

- Website content changes
- Images are replaced
- CSS or JavaScript files are updated
- Cached content needs to be refreshed

---

# CDN Rules

CDN configuration can control how requests and cached content are handled.

Examples include:

- Caching behavior
- URL-based caching
- Query string behavior
- HTTP headers
- Compression
- Redirects

The available rule capabilities depend on the CDN offering and endpoint configuration.

---

# CDN Compression

CDN can compress supported content before sending it to clients.

Example:

```text
Origin
  │
  ▼
CDN
  │
  │ Compressed Content
  ▼
User
```

Compression can reduce the amount of data transferred and improve delivery performance.

---

# Custom Domain

A custom domain can be configured for CDN delivery.

Instead of:

```text
https://example.azureedge.net
```

users can access content through a domain such as:

```text
https://cdn.example.com
```

---

# HTTPS

CDN endpoints can provide secure content delivery using HTTPS.

```text
User
  │
  │ HTTPS
  ▼
Azure CDN
  │
  ▼
Origin
```

HTTPS helps protect data while it is transmitted between the client and CDN.

---

# Azure CDN Use Cases

Azure CDN is useful for:

- Static websites
- Images and media
- JavaScript and CSS
- Video delivery
- Downloadable files
- Frequently accessed static content
- Reducing origin server load
- Global content delivery

---

# Azure CDN vs Azure Front Door

| Azure CDN | Azure Front Door |
|---|---|
| Primarily content delivery and caching | Global application delivery |
| Focuses on content caching | Focuses on application routing and delivery |
| Static content delivery | Dynamic and static application delivery |
| Edge caching | Edge delivery and routing |
| Origin-based content delivery | Origin groups and routing |
| Suitable for CDN workloads | Suitable for global web applications |

---

# Practical Lab

## Lab: Configure Azure CDN with Azure Storage

### Objective

Create an Azure Storage account, upload static content, configure a CDN endpoint, and verify that the content is delivered through the CDN.

### Architecture

```text
                         Internet
                            │
                            ▼
                        Azure CDN
                            │
                     CDN Endpoint
                            │
                            ▼
                    Azure Storage
                            │
                            ▼
                       Blob Content
```

---

## Step 1: Create Storage Account

1. Open **Azure Portal**.
2. Search for **Storage accounts**.
3. Select **Create**.
4. Create a storage account.
5. Use the appropriate redundancy and performance options for the lab.
6. Create the storage account.

---

## Step 2: Create a Container

Open the storage account.

1. Go to **Data storage → Containers**.
2. Select **+ Container**.
3. Enter:

```text
cdn-content
```

4. Create the container.

---

## Step 3: Upload a File

Open the container and upload a test file.

Example:

```text
index.html
```

or:

```text
image.jpg
```

Verify that the file exists in the container.

---

## Step 4: Create CDN Profile

1. Search for **Front Door and CDN profiles**.
2. Select **Create**.
3. Choose **Azure CDN**.
4. Create a CDN profile.

Example:

```text
cdn-demo-profile
```

---

## Step 5: Create CDN Endpoint

Configure the CDN endpoint.

Example:

```text
Endpoint name:
cdn-demo-endpoint
```

Configure the origin using the storage account.

```text
Origin:
Azure Storage

Origin Hostname:
<storage-account-endpoint>
```

Create the endpoint.

---

## Step 6: Access the CDN Endpoint

Azure provides a CDN endpoint hostname similar to:

```text
https://cdn-demo-endpoint.azureedge.net
```

Open the CDN endpoint and request the uploaded content.

Example:

```text
https://cdn-demo-endpoint.azureedge.net/index.html
```

---

## Step 7: Verify Content Delivery

Verify that the file is accessible through the CDN endpoint.

```text
User
  │
  ▼
CDN Endpoint
  │
  ▼
Cached Content
```

---

## Step 8: Test Cache Behavior

Request the same file multiple times.

```text
First Request
     ↓
CDN
     ↓
Origin
     ↓
Content
     ↓
CDN Cache

Later Requests
     ↓
CDN Cache
     ↓
Content
```

Use browser developer tools or response headers to inspect caching behavior where available.

---

## Step 9: Update the Origin Content

Modify the content stored in the origin.

For example:

```text
Version 1
   ↓
Upload Version 2
```

Depending on the caching configuration, the CDN may continue serving the cached version until the cache expires or is purged.

---

## Step 10: Purge Cached Content

Open the CDN endpoint.

Use the **Purge** operation to remove the cached content.

Select the appropriate path, for example:

```text
/index.html
```

After the purge, request the file again.

```text
User
  │
  ▼
CDN
  │
  │ Cache Miss
  ▼
Origin
  │
  ▼
New Content
```

Verify that the updated content is returned.

---

# Key Points

- Azure CDN delivers content through geographically distributed edge locations.
- CDN primarily improves the delivery of static and cacheable content.
- A CDN profile contains CDN configuration.
- A CDN endpoint provides the CDN access URL.
- An origin is the source of the content.
- CDN caching reduces requests to the origin.
- A cache hit serves content from the edge.
- A cache miss retrieves content from the origin.
- Cache invalidation or purging removes cached content.
- Azure Storage can be used as a CDN origin.
- CDN can support custom domains and HTTPS.
- Azure CDN focuses primarily on content delivery and caching.
- Azure Front Door provides broader global application delivery and routing capabilities.
