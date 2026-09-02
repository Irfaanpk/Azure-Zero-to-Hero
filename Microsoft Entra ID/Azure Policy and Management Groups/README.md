## What is Azure Policy?

**Azure Policy** is a governance service that allows organizations to create rules and enforce requirements for Azure resources.

Azure Policy can be used to:

- Restrict resource locations
- Require specific configurations
- Audit resources
- Enforce organizational standards
- Prevent non-compliant resource deployments
- Apply governance across multiple resources and subscriptions

Example:

```text
Azure Policy

Allowed Locations:
Central India
South India
```

If a user tries to create a resource in a location that is not allowed by the policy, the deployment can be denied.

---

## Why Use Azure Policy?

Azure Policy helps organizations:

- Maintain consistent configurations
- Enforce organizational standards
- Prevent unwanted resource deployments
- Audit resource compliance
- Apply governance at scale

Example:

```text
Organization Requirement:

All storage accounts must use HTTPS
```

Azure Policy can be used to enforce or audit this requirement.

---

# Azure Policy Components

Azure Policy mainly works with the following concepts:

```text
Policy Definition
      │
      ▼
Policy Assignment
      │
      ▼
Scope
      │
      ▼
Resources
      │
      ▼
Compliance
```

---

## Policy Definition

A **Policy Definition** contains the rule that Azure should evaluate.

Example:

```text
Rule:

Resources must be deployed only in
Central India
```

Another example:

```text
Rule:

Storage accounts must require
secure transfer
```

---

## Policy Assignment

A **Policy Assignment** applies a policy definition to a specific scope.

Example:

```text
Policy Definition
"Allowed Locations"
        │
        ▼
Policy Assignment
        │
        ▼
Subscription
```

The policy then evaluates resources within that scope.

---

# Azure Policy Effects

Azure Policy supports different effects that determine what happens when a resource does not comply with a policy.

| Effect | Purpose |
|---|---|
| Deny | Prevent creation or update of a non-compliant resource |
| Audit | Allow the resource but record it as non-compliant |
| Modify | Modify or add properties during resource creation or update |
| Append | Add additional fields or properties |
| Disabled | Disable the policy effect |

---

## Deny

The **Deny** effect prevents a resource operation when it violates the policy.

Example:

```text
Policy:

Allowed Location = Central India

User:
Create VM in East US

        │
        ▼

Policy Evaluation

        │
        ▼

Denied
```

---

## Audit

The **Audit** effect allows the resource to exist but reports it as non-compliant.

Example:

```text
Resource
   │
   ▼
Policy Evaluation
   │
   ▼
Non-Compliant
   │
   ▼
Audit / Compliance Report
```

This is useful when an organization wants to identify existing issues without blocking deployments.

---

# Azure Policy vs Azure RBAC

Azure Policy and Azure RBAC solve different problems.

| Azure RBAC | Azure Policy |
|---|---|
| Controls who can perform actions | Controls what configurations are allowed or required |
| Authorization | Governance |
| Determines who can access resources | Evaluates resource compliance |
| Assigns roles | Assigns policies |

Example:

```text
Azure RBAC
    │
    ▼
Who can create a VM?
```

```text
Azure Policy
    │
    ▼
Where can the VM be created?
```

A user may have permission to create a resource through Azure RBAC, but Azure Policy can still prevent the resource from being created if it violates an assigned policy.

---

# Azure Management Groups

**Azure Management Groups** provide a way to organize multiple Azure subscriptions into a hierarchy.

They are useful for organizations that have multiple subscriptions.

Example:

```text
Management Group
       │
       ├── Production Subscription
       ├── Development Subscription
       └── Testing Subscription
```

Management Groups can be used with **Azure Policy** and **Azure RBAC** to apply governance and access controls across multiple subscriptions.

---

## Management Group Hierarchy

The Azure hierarchy can be represented as:

```text
Management Group
       │
       ▼
Subscription
       │
       ▼
Resource Group
       │
       ▼
Resource
```

A Management Group can contain multiple subscriptions.

---

## Why Use Management Groups?

Management Groups help organizations:

- Organize multiple subscriptions
- Apply Azure Policy across multiple subscriptions
- Apply RBAC at a higher level
- Manage governance centrally
- Organize subscriptions by business unit or environment

Example:

```text
Organization
     │
     ▼
Management Group
     │
     ├── Production
     │      ├── Subscription 1
     │      └── Subscription 2
     │
     └── Development
            ├── Subscription 3
            └── Subscription 4
```

---

# Management Groups and Azure Policy

Azure Policy can be assigned at the Management Group scope.

This allows a policy to apply across subscriptions underneath that Management Group.

Example:

```text
Management Group
       │
       │ Azure Policy
       ▼
Subscriptions
       │
       ├── Production Subscription
       ├── Development Subscription
       └── Testing Subscription
```

For example, an organization can assign a policy that allows resources to be created only in approved Azure regions.

The policy can then apply to multiple subscriptions without creating the same policy assignment separately for every subscription.

---

# Management Groups and RBAC

Azure RBAC can also be assigned at the Management Group scope.

Example:

```text
Management Group
       │
       │ Reader Role
       ▼
Subscriptions
       │
       ├── Subscription 1
       ├── Subscription 2
       └── Subscription 3
```

The role assignment can be inherited by resources under the management group according to Azure RBAC inheritance rules.

---

# Azure Policy Scope

Azure Policy can be assigned at different scopes.

Common scopes include:

```text
Management Group
       │
       ▼
Subscription
       │
       ▼
Resource Group
       │
       ▼
Resource
```

This allows organizations to apply governance at the level required.

---

## Key Points

- Azure Policy is used for governance and compliance.
- Policy definitions contain governance rules.
- Policy assignments apply policies to a specific scope.
- Common policy effects include **Deny, Audit, Modify, and Append**.
- **Deny** can prevent non-compliant deployments.
- **Audit** identifies non-compliant resources without blocking them.
- Azure Policy is different from Azure RBAC.
- Azure RBAC controls **who can access or manage resources**.
- Azure Policy controls **what configurations are allowed or required**.
- Management Groups organize multiple Azure subscriptions.
- Management Groups can be used with Azure Policy and Azure RBAC.
- Policies can be assigned at Management Group, Subscription, Resource Group, and Resource scopes.
- Management Groups are useful for applying governance across multiple subscriptions.
