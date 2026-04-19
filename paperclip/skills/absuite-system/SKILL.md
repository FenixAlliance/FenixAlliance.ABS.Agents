---
name: absuite-system
description: >
  Manage system administration in the Alliance Business Suite (ABS) using the
  `absuite` CLI. Covers user management, tenant management, system options, licensing,
  database migrations, health checks, admin emails, and antiforgery tokens. Requires
  an authenticated CLI session with admin privileges for most operations.
---

# Alliance Business Suite — System Skill

Manage system administration through the `absuite` CLI's `system` service. Most operations require admin-level access.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite system list-commands`

## Health & Version

```bash
# Health check
absuite system get health

# Platform version
absuite system get version

# Hello / ping
absuite system get hello
```

## Users (Admin)

```bash
# List users
absuite system list users

# Count users
absuite system count users

# Get user by ID
absuite system get user --UserId <user-guid>

# List extended users
absuite system list extended-users
absuite system count extended-users

# Get extended user
absuite system get extended-account-holder --UserId <user-guid>

# Create user
absuite system create account-holder --AccountHolderCreateDto '{
  "Email": "newuser@example.com",
  "Password": "SecureP@ss123"
}'

# Update user
absuite system update account-holder --UserId <user-guid> --AccountHolderUpdateDto '{...}'

# Delete user
absuite system delete account-holder --UserId <user-guid>
```

## Tenants (Admin)

```bash
# List tenants
absuite system list all-tenants
absuite system count tenants

# List extended tenants
absuite system list all-extended-tenants
absuite system count extended-tenants

# Get tenant by ID
absuite system get tenant --TenantId <tenant-guid>

# Create tenant
absuite system create tenant --TenantCreateDto '{
  "Name": "New Business",
  "Email": "admin@newbusiness.com"
}'

# Update tenant
absuite system update tenant --TenantId <tenant-guid> --TenantUpdateDto '{...}'

# Delete tenant
absuite system delete tenant --TenantId <tenant-guid>
```

## System Options (Key-Value Configuration)

```bash
# List
absuite system list options
absuite system count options

# Get by ID
absuite system get option-by-id --OptionId <option-guid>

# Get by key
absuite system get option-by-key --Key "smtp.host"

# Create
absuite system create option --SystemOptionCreateDto '{
  "Key": "app.maintenance-mode",
  "Value": "false"
}'

# Update
absuite system update option --OptionId <option-guid> --SystemOptionUpdateDto '{
  "Value": "true"
}'

# Upsert (create or update by key)
absuite system upsert option --Key "app.maintenance-mode" --SystemOptionUpdateDto '{
  "Value": "true"
}'

# Delete
absuite system delete option --OptionId <option-guid>
```

## Licensing

```bash
# List licenses
absuite system list licenses

# Get license by ID
absuite system get license-by-id --LicenseId <license-guid>

# Get license quota
absuite system get license-records-quota

# List license assignments / attributes / features
absuite system list license-assignments --LicenseId <license-guid>
absuite system list license-attributes --LicenseId <license-guid>
absuite system list license-features --LicenseId <license-guid>

# Validate a license
absuite system confirm-license --LicenseKey <key>

# Redeem a license
absuite system redeem-license --LicenseKey <key>
```

## Modules

```bash
# List all modules on the server
absuite system list all-modules

# List modules available to current tenant user
absuite system list available-modules --TenantId $TENANT_ID
```

## Database Migrations

```bash
# List migrations
absuite system migrations

# Apply pending migrations
absuite system move-
```

## Admin Email Operations

```bash
# Preview basic email template
absuite system admin-preview-basic-email-template --EmailTemplateId <template-guid>

# Send basic email
absuite system admin-send-basic-email --EmailDispatchRequest '{
  "title": "System Notification",
  "message": "Scheduled maintenance tonight.",
  "recipients": ["admin@example.com"]
}'

# Preview/send user-specific email
absuite system admin-preview-user-email-template --UserId <user-guid>
absuite system admin-send-user-email --UserId <user-guid> --EmailDispatchRequest '{...}'

# Preview/send tenant-specific email
absuite system admin-preview-tenant-email --TenantId <tenant-guid>
absuite system admin-send-tenant-email --TenantId <tenant-guid> --EmailDispatchRequest '{...}'
```

## Authentication & Security

```bash
# Validate antiforgery request
absuite system is-request-valid

# Get and store antiforgery tokens
absuite system list and-store-tokens
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Health check | `absuite system get health` |
| Platform version | `absuite system get version` |
| List users | `absuite system list users` |
| Create user | `absuite system create account-holder --AccountHolderCreateDto '{...}'` |
| List tenants | `absuite system list all-tenants` |
| Create tenant | `absuite system create tenant --TenantCreateDto '{...}'` |
| List options | `absuite system list options` |
| Upsert option | `absuite system upsert option --Key <key> --SystemOptionUpdateDto '{...}'` |
| List licenses | `absuite system list licenses` |
| List modules | `absuite system list all-modules` |
| Check migrations | `absuite system migrations` |

## Critical Rules

- **Admin access required** for most system operations.
- **Be careful with user/tenant deletion** — these are destructive operations.
- **Use `upsert option`** for idempotent configuration updates.
- **Database migrations (`move-`)** should be used with caution in production.
