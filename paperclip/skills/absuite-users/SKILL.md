---
name: absuite-users
description: >
  Manage the current user's profile, avatar, settings, addresses, enrollments,
  tenants, social profile, cart, wallet, followers, follows, notifications, and
  user options in the Alliance Business Suite (ABS) using the `absuite` CLI.
  Requires an authenticated CLI session.
---

# Alliance Business Suite — Users Skill

Manage the current user's account through the `absuite` CLI's `users` service. Most operations apply to the currently authenticated user.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite users list-commands`

## Current User Profile

```bash
# Get my profile
absuite users get current-user

# Get my extended profile (with related data)
absuite users get current-extended-user

# Update my profile
absuite users update current-user --AccountHolderUpdateDto '{
  "FirstName": "Jane",
  "LastName": "Doe",
  "PhoneNumber": "+1-555-0199"
}'

# Patch (partial update)
absuite users patch current-user --AccountHolderPatchDto '{
  "PhoneNumber": "+1-555-0200"
}'
```

## Avatar

```bash
# Get my avatar
absuite users get avatar

# Update my avatar
absuite users update avatar --Avatar @/path/to/avatar.png
```

## Cart, Wallet, Social Profile

```bash
# Get my cart
absuite users get cart

# Get my wallet
absuite users get wallet

# Get my social profile
absuite users get social-profile
```

## Tenant Memberships

```bash
# List my tenants
absuite users list tenants

# Count my tenants
absuite users count tenants
```

## Enrollments

```bash
# List my enrollments
absuite users list enrollments

# Count my enrollments
absuite users count enrollments
```

## Followers & Follows

```bash
# My followers
absuite users list followers
absuite users count followers

# Who I follow
absuite users list follows
absuite users count follows
```

## Notifications

```bash
absuite users list notifications
absuite users count notifications
```

## Addresses

```bash
absuite users list addresses
absuite users count addresses
absuite users get address --AddressId <address-guid>
absuite users create address --AddressCreateDto '{
  "Line1": "123 Main St",
  "City": "Portland",
  "State": "OR",
  "PostalCode": "97201",
  "CountryId": "USA"
}'
absuite users update address --AddressId <address-guid> --AddressUpdateDto '{...}'
absuite users delete address --AddressId <address-guid>
```

## User Settings

```bash
absuite users get settings
absuite users update settings --UserSettingsUpdateDto '{...}'
```

## User Options (Key-Value)

```bash
absuite users list options
absuite users count options
absuite users get option-by-id --OptionId <option-guid>
absuite users get option-by-key --Key "ui.theme"
absuite users create option --UserOptionCreateDto '{
  "Key": "ui.theme",
  "Value": "dark"
}'
absuite users update option --OptionId <option-guid> --UserOptionUpdateDto '{
  "Value": "light"
}'
absuite users upsert option --Key "ui.theme" --UserOptionUpdateDto '{"Value": "dark"}'
absuite users delete option --OptionId <option-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Get my profile | `absuite users get current-user` |
| Update my profile | `absuite users update current-user --AccountHolderUpdateDto '{...}'` |
| Get my avatar | `absuite users get avatar` |
| Update avatar | `absuite users update avatar --Avatar @/path/to/avatar.png` |
| My tenants | `absuite users list tenants` |
| My enrollments | `absuite users list enrollments` |
| My followers | `absuite users list followers` |
| My notifications | `absuite users list notifications` |
| My addresses | `absuite users list addresses` |
| My settings | `absuite users get settings` |
| Upsert option | `absuite users upsert option --Key <key> --UserOptionUpdateDto '{...}'` |

## Critical Rules

- **Authenticate first.**
- **User service is self-scoped** — all operations apply to the current user (not arbitrary users; use `system` service for admin user management).
- **Use `upsert option`** for idempotent key-value preference updates.
