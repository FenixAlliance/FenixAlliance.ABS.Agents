---
name: absuite-security
description: >
  Manage security roles, permissions, business applications, OAuth applications,
  OAuth authorizations, security certificates, security logs, and webhook requests
  in the Alliance Business Suite (ABS) using the `absuite` CLI. Covers RBAC
  assignment/revocation for enrollments, roles, and applications. Requires an
  authenticated CLI session.
---

# Alliance Business Suite — Security Skill

Manage security through the `absuite` CLI's `security` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite security list-commands`

## Roles

```bash
# List
absuite security list roles --TenantId $TENANT_ID

# Count
absuite security count roles --TenantId $TENANT_ID

# Get by ID
absuite security get role --TenantId $TENANT_ID --RoleId <role-guid>

# Create
absuite security create role --TenantId $TENANT_ID --SecurityRoleCreateDto '{
  "Name": "Sales Manager",
  "Description": "Can manage sales pipeline and customer data"
}'

# Update
absuite security update role --TenantId $TENANT_ID --RoleId <role-guid> --SecurityRoleUpdateDto '{...}'

# Delete
absuite security delete role --TenantId $TENANT_ID --RoleId <role-guid>
```

## Permissions

```bash
# List
absuite security list permissions --TenantId $TENANT_ID

# Count
absuite security count permissions --TenantId $TENANT_ID

# Get by ID
absuite security get permission --TenantId $TENANT_ID --PermissionId <perm-guid>

# Create
absuite security create permission --TenantId $TENANT_ID --SecurityPermissionCreateDto '{
  "Name": "orders.read",
  "Description": "Read access to orders"
}'

# Update
absuite security update permission --TenantId $TENANT_ID --PermissionId <perm-guid> --SecurityPermissionUpdateDto '{...}'

# Delete
absuite security delete permission --TenantId $TENANT_ID --PermissionId <perm-guid>
```

### Permissions by Role

```bash
absuite security list role-permissions --TenantId $TENANT_ID --RoleId <role-guid>
```

## RBAC: Assign & Revoke

### Permission ↔ Role

```bash
# Assign permission to role
absuite security set permission-to-role --TenantId $TENANT_ID --RoleId <role-guid> --PermissionId <perm-guid>

# Revoke permission from role
absuite security revoke-permission-from-role --TenantId $TENANT_ID --RoleId <role-guid> --PermissionId <perm-guid>

# Get roles that have a permission
absuite security get roles-by-permission --TenantId $TENANT_ID --PermissionId <perm-guid>

# Assign role to permission (reverse direction)
absuite security set role-to-permission --TenantId $TENANT_ID --PermissionId <perm-guid> --RoleId <role-guid>
absuite security revoke-role-from-permission --TenantId $TENANT_ID --PermissionId <perm-guid> --RoleId <role-guid>
```

### Role/Permission ↔ Enrollment

```bash
# Assign role to enrollment
absuite security set role-to-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --RoleId <role-guid>

# Revoke role from enrollment
absuite security revoke-role-from-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --RoleId <role-guid>

# Assign permission to enrollment
absuite security set permission-to-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --PermissionId <perm-guid>

# Revoke permission from enrollment
absuite security revoke-permission-from-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --PermissionId <perm-guid>

# Query
absuite security get roles-by-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>
absuite security get permissions-by-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>
absuite security get enrollments-by-role --TenantId $TENANT_ID --RoleId <role-guid>
absuite security get enrollments-by-permission --TenantId $TENANT_ID --PermissionId <perm-guid>
```

### Role/Permission ↔ Business Application

```bash
# Assign
absuite security set role-to-business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid> --RoleId <role-guid>
absuite security set permission-to-business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid> --PermissionId <perm-guid>

# Revoke
absuite security revoke-role-from-business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid> --RoleId <role-guid>
absuite security revoke-permission-from-business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid> --PermissionId <perm-guid>

# Query
absuite security get applications-by-role --TenantId $TENANT_ID --RoleId <role-guid>
absuite security get applications-by-permission --TenantId $TENANT_ID --PermissionId <perm-guid>
```

## Business Applications

```bash
absuite security list business-applications --TenantId $TENANT_ID
absuite security count business-applications --TenantId $TENANT_ID
absuite security get business-application-by-id --TenantId $TENANT_ID --BusinessApplicationId <app-guid>
absuite security create business-application --TenantId $TENANT_ID --BusinessApplicationCreateDto '{
  "Name": "CRM Portal",
  "Description": "Customer-facing CRM application"
}'
absuite security update business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid> --BusinessApplicationUpdateDto '{...}'
absuite security delete business-application --TenantId $TENANT_ID --BusinessApplicationId <app-guid>
```

## OAuth Applications & Authorizations

```bash
# OAuth applications
absuite security list o-auth-applications --TenantId $TENANT_ID
absuite security count o-auth-applications --TenantId $TENANT_ID
absuite security get o-auth-application-by-id --TenantId $TENANT_ID --OAuthApplicationId <oauth-app-guid>
absuite security create o-auth-application --TenantId $TENANT_ID --OAuthApplicationCreateDto '{...}'
absuite security update o-auth-application --TenantId $TENANT_ID --OAuthApplicationId <oauth-app-guid> --OAuthApplicationUpdateDto '{...}'
absuite security delete o-auth-application --TenantId $TENANT_ID --OAuthApplicationId <oauth-app-guid>

# OAuth authorizations
absuite security list o-auth-authorizations --TenantId $TENANT_ID
absuite security count o-auth-authorizations --TenantId $TENANT_ID
absuite security get o-auth-authorization-by-id --TenantId $TENANT_ID --OAuthAuthorizationId <auth-guid>
```

## Security Logs & Certificates

```bash
# Logs
absuite security list logs --TenantId $TENANT_ID
absuite security count logs --TenantId $TENANT_ID

# Certificates
absuite security list certificates --TenantId $TENANT_ID
absuite security count certificates --TenantId $TENANT_ID
```

## Webhook Requests

```bash
absuite security list webhook-requests --TenantId $TENANT_ID
absuite security count webhook-requests --TenantId $TENANT_ID
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List roles | `absuite security list roles --TenantId <guid>` |
| Create role | `absuite security create role --TenantId <guid> --SecurityRoleCreateDto '{...}'` |
| List permissions | `absuite security list permissions --TenantId <guid>` |
| Create permission | `absuite security create permission --TenantId <guid> --SecurityPermissionCreateDto '{...}'` |
| Assign perm to role | `absuite security set permission-to-role --TenantId <guid> --RoleId <guid> --PermissionId <guid>` |
| Assign role to enrollment | `absuite security set role-to-enrollment --TenantId <guid> --EnrollmentId <guid> --RoleId <guid>` |
| Revoke role from enrollment | `absuite security revoke-role-from-enrollment --TenantId <guid> --EnrollmentId <guid> --RoleId <guid>` |
| View security logs | `absuite security list logs --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Create roles and permissions first** before assigning them.
- **RBAC is bidirectional** — you can assign permission-to-role or role-to-permission.
- **Enrollments** are user memberships in a tenant — assign roles/permissions to enrollments to control access.
