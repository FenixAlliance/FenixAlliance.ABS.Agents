---
name: absuite-pricing
description: >
  Manage price lists, price list entries, discount lists, and discount list entries
  in the Alliance Business Suite (ABS) using the `absuite` CLI. Includes price
  calculation and savings/tax estimation. Requires an authenticated CLI session.
---

# Alliance Business Suite — Pricing Skill

Manage pricing through the `absuite` CLI's `pricing` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite pricing list-commands`

## Price Lists

```bash
# List
absuite pricing list price-lists --TenantId $TENANT_ID

# Count
absuite pricing count price-lists --TenantId $TENANT_ID

# Get by ID
absuite pricing get price-list --TenantId $TENANT_ID --PriceListId <list-guid>

# Create
absuite pricing create price-list --TenantId $TENANT_ID --PriceListCreateDto '{
  "Name": "Retail Price List 2026",
  "Description": "Standard retail pricing",
  "StartDate": "2026-01-01T00:00:00Z",
  "EndDate": "2026-12-31T23:59:59Z",
  "CurrencyId": "<currency-guid>"
}'

# Update
absuite pricing update price-list --TenantId $TENANT_ID --PriceListId <list-guid> --PriceListUpdateDto '{...}'

# Delete
absuite pricing delete price-list --TenantId $TENANT_ID --PriceListId <list-guid>
```

### Price List Entries (Prices)

```bash
# List entries in a price list
absuite pricing list price-list-prices --TenantId $TENANT_ID --PriceListId <list-guid>

# Get a specific entry
absuite pricing get price-list-price --TenantId $TENANT_ID --PriceListPriceId <entry-guid>

# Create an entry (set price for an item in a price list)
absuite pricing create price-list-prices --TenantId $TENANT_ID --PriceListPriceCreateDto '{
  "PriceListId": "<list-guid>",
  "ItemId": "<item-guid>",
  "Amount": 49.99
}'

# Update
absuite pricing update price-list-price --TenantId $TENANT_ID --PriceListPriceId <entry-guid> --PriceListPriceUpdateDto '{...}'

# Delete
absuite pricing delete price-list-price --TenantId $TENANT_ID --PriceListPriceId <entry-guid>
```

## Discount Lists

```bash
# List
absuite pricing list discount-lists --TenantId $TENANT_ID

# Count
absuite pricing count discount-lists --TenantId $TENANT_ID

# Get by ID
absuite pricing get discount-list --TenantId $TENANT_ID --DiscountListId <list-guid>

# Create
absuite pricing create discount-list --TenantId $TENANT_ID --DiscountListCreateDto '{
  "Name": "VIP Discounts",
  "Description": "Discounts for VIP customers"
}'

# Update
absuite pricing update discount-list --TenantId $TENANT_ID --DiscountListId <list-guid> --DiscountListUpdateDto '{...}'

# Delete
absuite pricing delete discount-list --TenantId $TENANT_ID --DiscountListId <list-guid>
```

### Discount List Entries

```bash
# List entries
absuite pricing list discount-list-entries --TenantId $TENANT_ID --DiscountListId <list-guid>

# Count entries
absuite pricing count discount-list-entries --TenantId $TENANT_ID --DiscountListId <list-guid>

# Get specific entry
absuite pricing get discount-list-entry --TenantId $TENANT_ID --DiscountListEntryId <entry-guid>

# Create entry
absuite pricing create discount-list-entry --TenantId $TENANT_ID --DiscountListEntryCreateDto '{
  "DiscountListId": "<list-guid>",
  "ItemId": "<item-guid>",
  "DiscountPercentage": 15.0
}'

# Update entry
absuite pricing update discount-list-entry --TenantId $TENANT_ID --DiscountListEntryId <entry-guid> --DiscountListEntryUpdateDto '{...}'

# Delete entry
absuite pricing delete discount-list-entry --TenantId $TENANT_ID --DiscountListEntryId <entry-guid>
```

## Price Calculation

### Get Calculated Price for an Item

```bash
absuite pricing get price --TenantId $TENANT_ID --ItemId <item-guid>
```

### Get Final Price (after discounts and taxes)

```bash
absuite pricing get final-price --TenantId $TENANT_ID --ItemId <item-guid>
```

### Get Total Savings (in USD)

```bash
absuite pricing get total-savings-in-usd --TenantId $TENANT_ID --ItemId <item-guid>
```

### Get Total Taxes (in USD)

```bash
absuite pricing get total-taxes-in-usd --TenantId $TENANT_ID --ItemId <item-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List price lists | `absuite pricing list price-lists --TenantId <guid>` |
| Create price list | `absuite pricing create price-list --TenantId <guid> --PriceListCreateDto '{...}'` |
| Set item price | `absuite pricing create price-list-prices --TenantId <guid> --PriceListPriceCreateDto '{...}'` |
| List discount lists | `absuite pricing list discount-lists --TenantId <guid>` |
| Create discount | `absuite pricing create discount-list-entry --TenantId <guid> --DiscountListEntryCreateDto '{...}'` |
| Get final price | `absuite pricing get final-price --TenantId <guid> --ItemId <guid>` |
| Get total taxes | `absuite pricing get total-taxes-in-usd --TenantId <guid> --ItemId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Create price lists before price entries.** Entries reference a price list ID.
- **Create discount lists before discount entries.**
- **Price calculations are dynamic** — they apply active price lists, discount lists, and tax policies.
