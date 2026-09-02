# 6.5 Static Website Hosting

**Azure Storage Static Website Hosting** allows you to host a static website directly from an Azure Storage Account.

A static website contains files such as:

- HTML
- CSS
- JavaScript
- Images

No web server is required to host the static content.

---

## How Static Website Hosting Works

Azure Storage provides a special container called:

```text
$web
```

When static website hosting is enabled, Azure serves the files stored inside this container as a website.

```text
Storage Account
└── $web
    ├── index.html
    ├── error.html
    ├── style.css
    ├── script.js
    └── images/
```

---

## `$web` Container

The `$web` container is automatically created when static website hosting is enabled.

Website files must be uploaded to:

```text
$web
```

Example:

```text
$web
├── index.html
├── error.html
└── style.css
```

---

## Index Document

The **index document** is the default page displayed when a user opens the static website.

Common example:

```text
index.html
```

Example:

```text
https://<storage-account>.zXX.web.core.windows.net/
```

The request loads:

```text
$web/index.html
```

---

## Error Document

The **error document** is displayed when a requested page cannot be found.

Example:

```text
404.html
```

You can configure:

```text
404.html
```

as the error document for the static website.

---

## Static Website Endpoint

When static website hosting is enabled, Azure provides a **web endpoint** for the website.

The endpoint follows a format similar to:

```text
https://<storage-account>.zXX.web.core.windows.net/
```

The exact endpoint is provided by Azure after static website hosting is enabled.

---

## Static Website Hosting vs Blob Access Level

These are different concepts.

| Feature | Purpose |
|---|---|
| **Blob Access Level** | Controls anonymous access to blobs |
| **Static Website Hosting** | Serves static website files through a web endpoint |

Static website hosting uses the `$web` container and provides a dedicated website endpoint.

---

## What Can Be Hosted?

Static Website Hosting can serve:

```text
HTML
CSS
JavaScript
Images
Fonts
Static JSON files
```

It is suitable for websites that do not require server-side processing.

Examples:

- Documentation websites
- Portfolio websites
- Simple company websites
- Static landing pages
- Front-end applications

---

## Limitations

Static Website Hosting is designed for **static content**.

It does not provide server-side application execution such as:

```text
PHP
ASP.NET server-side processing
Node.js server-side applications
Python server-side applications
```

For applications requiring server-side processing, use an appropriate Azure compute service such as Azure App Service or Azure Functions.

---

# 🧪 Lab: Host a Static Website Using Azure Blob Storage

### Step 1: Open Storage Account

Go to:

```text
https://portal.azure.com
```

Open:

```text
Storage accounts
```

Select your storage account.

---

### Step 2: Open Static Website Settings

In the storage account, go to:

```text
Data management → Static website
```

---

### Step 3: Enable Static Website

Set:

```text
Static website: Enabled
```

Enter:

```text
Index document name: index.html
```

Enter:

```text
Error document path: error.html
```

Click:

```text
Save
```

---

### Step 4: Open the `$web` Container

Go to:

```text
Data storage → Containers
```

You will see:

```text
$web
```

Open the container.

---

### Step 5: Create Website Files

Create the following files locally:

```text
index.html
error.html
style.css
```

Example `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Azure Static Website</title>
</head>
<body>
    <h1>Welcome to Azure Static Website Hosting</h1>
    <p>My website is hosted using Azure Blob Storage.</p>
</body>
</html>
```

Example `error.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Page Not Found</title>
</head>
<body>
    <h1>404 - Page Not Found</h1>
</body>
</html>
```

---

### Step 6: Upload Files

Inside the:

```text
$web
```

container, click:

```text
Upload
```

Upload:

```text
index.html
error.html
style.css
```

---

### Step 7: Open the Website

Return to:

```text
Data management → Static website
```

Copy the **Primary endpoint**.

Example:

```text
https://<storage-account>.zXX.web.core.windows.net/
```

Open it in your browser.

You should see:

```text
Welcome to Azure Static Website Hosting
```

---

### Step 8: Test the Error Page

Open a URL for a file that does not exist.

Example:

```text
https://<storage-account>.zXX.web.core.windows.net/test.html
```

Azure should display the configured:

```text
error.html
```

---

## Key Points

- Azure Storage can host **static websites**.
- Static Website Hosting uses the special **`$web` container**.
- Website files are stored inside `$web`.
- **Index document** defines the default page.
- **Error document** defines the page displayed for errors such as missing files.
- Azure provides a dedicated **static website endpoint**.
- Static websites support HTML, CSS, JavaScript, images, and other static files.
- Static Website Hosting does not provide server-side application execution.
