---
name: absuite-identity
description: >
  Manage authentication, authorization, OpenID Connect, and user identity in the
  Alliance Business Suite (ABS) using the `absuite` CLI. Covers password sign-in,
  OAuth tokens, user info, permissions, enrolled roles, and OIDC configuration.
  Requires an authenticated CLI session for most operations.
---

# Alliance Business Suite — Identity Skill

Manage identity and authentication through the `absuite` CLI's `identity` service.

## Prerequisites

1. For sign-in operations, no prior authentication is needed.
2. For permission/role queries, **authenticate first** using `absuite login`.
3. **Discover commands**: `absuite identity list-commands`

## Authentication

### Password Sign-In

```bash
absuite identity password-sign-in --Email user@example.com --Password "YourPassword"
```

### Check Password Sign-In (Validate Credentials)

```bash
absuite identity check-password-sign-in --Email user@example.com --Password "YourPassword"
```

### Get OAuth Token

```bash
absuite identity token --GrantType password --Username user@example.com --Password "YourPassword"
```

### Check if Authenticated

```bash
absuite identity is-authenticated
```

### Get Current User Identity

```bash
absuite identity get-
```

### Get User Info (OpenID Connect)

```bash
absuite identity connect-userinfo-get
absuite identity connect-userinfo-post
```

## OpenID Connect Configuration

```bash
# Get OIDC discovery document
absuite identity get open-id-configuration

# Get JSON Web Key Set
absuite identity list jw-ks
```

## Permissions & Roles

### List User Permissions

```bash
absuite identity list permissions
```

### Get Resource Message (Authenticated Endpoint)

```bash
absuite identity get message
```

### Get Application by ID

```bash
absuite identity get application --ApplicationId <app-guid>
```

### Granted Permissions for an Enrollment

```bash
absuite identity list granted-enrollment-permissions --ApplicationId <app-guid> --RoleId <role-guid>
```

### Granted Tenant Permissions

```bash
absuite identity list granted-tenant-permissions --ApplicationId <app-guid>
```

### Granted Tenant Roles

```bash
absuite identity list granted-tenant-roles --ApplicationId <app-guid>
```

### Required Permissions for an Application

```bash
absuite identity list required-permissions --ApplicationId <app-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Sign in with password | `absuite identity password-sign-in --Email <email> --Password <pwd>` |
| Get OAuth token | `absuite identity token --GrantType password --Username <email> --Password <pwd>` |
| Check authentication | `absuite identity is-authenticated` |
| Get current user | `absuite identity get-` |
| List permissions | `absuite identity list permissions` |
| Get OIDC config | `absuite identity get open-id-configuration` |
| Get JWKS | `absuite identity list jw-ks` |

## Critical Rules

- **For CLI sessions, prefer `absuite login`** over direct `identity password-sign-in` — the login skill handles token storage.
- **`identity` is lower-level** — use it when you need raw OAuth tokens, OIDC discovery, or permission queries.
- **Passwords are sensitive.** Never log or store them in plain text.
