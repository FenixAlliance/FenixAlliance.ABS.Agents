---
name: absuite-locations
description: >
  Manage physical and wallet locations in the Alliance Business Suite (ABS) using
  the `absuite` CLI. Covers location CRUD and wallet location management.
  Requires an authenticated CLI session.
---

# Alliance Business Suite — Locations Skill

Manage locations through the `absuite` CLI's `locations` service.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite locations list-commands`

## Locations

```bash
# List
absuite locations list --TenantId $TENANT_ID

# Count
absuite locations count --TenantId $TENANT_ID

# Get by ID
absuite locations get --TenantId $TENANT_ID --LocationId <location-guid>

# Create
absuite locations create --TenantId $TENANT_ID --LocationCreateDto '{
  "Name": "Main Office",
  "Description": "Headquarters",
  "AddressLine1": "123 Business Ave",
  "CountryId": "USA",
  "StateId": "<state-guid>",
  "CityId": "<city-guid>"
}'

# Update
absuite locations update --TenantId $TENANT_ID --LocationId <location-guid> --LocationUpdateDto '{...}'

# Delete
absuite locations delete --TenantId $TENANT_ID --LocationId <location-guid>
```

## Wallet Locations

Locations specifically associated with wallets/billing profiles.

```bash
# List
absuite locations list wallet --TenantId $TENANT_ID

# Count
absuite locations count wallet --TenantId $TENANT_ID

# Get by ID
absuite locations get wallet --TenantId $TENANT_ID --WalletLocationId <wallet-location-guid>

# Create
absuite locations create wallet --TenantId $TENANT_ID --WalletLocationCreateDto '{...}'

# Update
absuite locations update wallet --TenantId $TENANT_ID --WalletLocationId <wallet-location-guid> --WalletLocationUpdateDto '{...}'

# Delete
absuite locations delete wallet --TenantId $TENANT_ID --WalletLocationId <wallet-location-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List locations | `absuite locations list --TenantId <guid>` |
| Create location | `absuite locations create --TenantId <guid> --LocationCreateDto '{...}'` |
| List wallet locations | `absuite locations list wallet --TenantId <guid>` |
| Create wallet location | `absuite locations create wallet --TenantId <guid> --WalletLocationCreateDto '{...}'` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Use `absuite globe`** to look up valid `CountryId`, `StateId`, and `CityId` values.
