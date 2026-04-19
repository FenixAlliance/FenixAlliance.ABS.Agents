---
name: absuite-wallets
description: >
  Manage wallets, wallet orders, invoices (incoming/outgoing), payments
  (incoming/outgoing), and wallet locations in the Alliance Business Suite (ABS)
  using the `absuite` CLI. Requires an authenticated CLI session.
---

# Alliance Business Suite — Wallets Skill

Manage wallets through the `absuite` CLI's `wallets` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite wallets list-commands`

## Wallet Details

```bash
# Get wallet details
absuite wallets get details --TenantId $TENANT_ID --WalletId <wallet-guid>
```

## Orders

```bash
# List orders in wallet
absuite wallets list orders --TenantId $TENANT_ID --WalletId <wallet-guid>

# List extended orders (with related data)
absuite wallets list extended-orders --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count orders
absuite wallets count orders --TenantId $TENANT_ID --WalletId <wallet-guid>
```

## Invoices

### Incoming Invoices (received)

```bash
# List
absuite wallets list incoming-invoices --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count
absuite wallets count incoming-invoices --TenantId $TENANT_ID --WalletId <wallet-guid>
```

### Outgoing Invoices (sent)

```bash
# List
absuite wallets list outgoing-invoices --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count
absuite wallets count outgoing-invoices --TenantId $TENANT_ID --WalletId <wallet-guid>
```

## Payments

### Incoming Payments (received)

```bash
# List
absuite wallets list incoming-payments --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count
absuite wallets count incoming-payments --TenantId $TENANT_ID --WalletId <wallet-guid>
```

### Outgoing Payments (sent)

```bash
# List
absuite wallets list outgoing-payments --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count
absuite wallets count outgoing-payments --TenantId $TENANT_ID --WalletId <wallet-guid>
```

## Wallet Locations

```bash
# List
absuite wallets list locations --TenantId $TENANT_ID --WalletId <wallet-guid>

# Count
absuite wallets count locations --TenantId $TENANT_ID --WalletId <wallet-guid>

# Get by ID
absuite wallets get location --TenantId $TENANT_ID --WalletLocationId <location-guid>

# Create
absuite wallets create location --TenantId $TENANT_ID --WalletLocationCreateDto '{
  "WalletId": "<wallet-guid>",
  "Name": "Main Office",
  "Address": "123 Business Ave"
}'

# Update
absuite wallets update location --TenantId $TENANT_ID --WalletLocationId <location-guid> --WalletLocationUpdateDto '{...}'

# Delete
absuite wallets delete location --TenantId $TENANT_ID --WalletLocationId <location-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Wallet details | `absuite wallets get details --TenantId <guid> --WalletId <guid>` |
| List orders | `absuite wallets list orders --TenantId <guid> --WalletId <guid>` |
| Incoming invoices | `absuite wallets list incoming-invoices --TenantId <guid> --WalletId <guid>` |
| Outgoing invoices | `absuite wallets list outgoing-invoices --TenantId <guid> --WalletId <guid>` |
| Incoming payments | `absuite wallets list incoming-payments --TenantId <guid> --WalletId <guid>` |
| Outgoing payments | `absuite wallets list outgoing-payments --TenantId <guid> --WalletId <guid>` |
| List locations | `absuite wallets list locations --TenantId <guid> --WalletId <guid>` |
| Create location | `absuite wallets create location --TenantId <guid> --WalletLocationCreateDto '{...}'` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **WalletId is required** for most queries — get it from `absuite tenants get wallet` or `absuite users get wallet`.
- **Invoices and payments are split by direction** — use `incoming-*` for received, `outgoing-*` for sent.
