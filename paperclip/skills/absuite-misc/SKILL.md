---
name: absuite-misc
description: >
  Minimal ABS CLI services with limited commands: marketplace, inventory, logistics,
  shipments, and sales. These services currently have very few functions but are
  documented here for completeness. Requires an authenticated CLI session.
---

# Alliance Business Suite — Miscellaneous Services Skill

These services have minimal CLI coverage. They are grouped here for reference.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant** where applicable.

## Inventory

```bash
# List inventory details
absuite inventory list details --TenantId $TENANT_ID
```

## Logistics

```bash
# List logistics contacts
absuite logistics list contacts --TenantId $TENANT_ID
```

## Shipments

```bash
# List shipments
absuite shipments list --TenantId $TENANT_ID
```

## Sales

```bash
# Get a sales quote
absuite sales get quote --TenantId $TENANT_ID --QuoteId <quote-guid>
```

## Marketplace

The marketplace service currently has **no commands available** via the CLI.

```bash
# Check for updates
absuite marketplace list-commands
```

## Critical Rules

- These services have limited CLI coverage — check `list-commands` periodically for new additions.
- Use dedicated services for full functionality (e.g., `absuite quotes` for quote management, `absuite catalog` for product catalog).
