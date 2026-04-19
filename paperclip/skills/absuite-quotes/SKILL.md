---
name: absuite-quotes
description: >
  Manage sales quotes, quote lines, quote calculations, and quote-to-order conversion
  in the Alliance Business Suite (ABS) using the `absuite` CLI. Covers full quote
  lifecycle: create, calculate, close, reopen, convert to order, and email delivery.
  Requires an authenticated CLI session.
---

# Alliance Business Suite — Quotes Skill

Manage quotes through the `absuite` CLI's `quotes` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite quotes list-commands`

## Quotes

### List Quotes

```bash
absuite quotes list --TenantId $TENANT_ID
```

### List Extended Quotes (with related data)

```bash
absuite quotes list extended --TenantId $TENANT_ID
```

### Count Quotes

```bash
absuite quotes count --TenantId $TENANT_ID
```

### Get Quote by ID

```bash
absuite quotes get --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Create Quote

```bash
absuite quotes create --TenantId $TENANT_ID --QuoteCreateDto '{
  "Title": "Q-2026-042: Website Redesign",
  "Description": "Quote for complete website redesign project",
  "IndividualId": "<contact-guid>",
  "CurrencyId": "<currency-guid>",
  "PriceListId": "<price-list-guid>",
  "PaymentTermId": "<payment-term-guid>",
  "EffectiveFrom": "2026-04-19T00:00:00Z",
  "EffectiveTo": "2026-05-19T00:00:00Z",
  "FirstName": "Jane",
  "LastName": "Doe",
  "CompanyName": "Acme Corp",
  "BillingEmail": "jane.doe@acme.com",
  "CountryId": "USA"
}'
```

**Key QuoteCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `Title` | String | Quote title |
| `Description` | String | Description |
| `IndividualId` | String | Customer contact (individual) |
| `OrganizationId` | String | Customer contact (organization) |
| `CurrencyId` | String | Quote currency |
| `PriceListId` | String | Price list to use |
| `PaymentTermId` | String | Payment terms |
| `EffectiveFrom` / `EffectiveTo` | DateTime | Validity period |
| `DealUnitId` | String | Linked deal unit |
| `CartId` | String | Linked cart |
| `ReceiverTenantId` | String | Receiving tenant |
| `CostCalculationMethod` | String | Cost calculation method |
| `TaxCalculationMethod` | String | Tax calculation method |
| `QuoteStatus` | String | Status |

### Update Quote

```bash
absuite quotes update --TenantId $TENANT_ID --QuoteId <quote-guid> --QuoteUpdateDto '{...}'
```

### Delete Quote

```bash
absuite quotes delete --TenantId $TENANT_ID --QuoteId <quote-guid>
```

## Quote Lines

### List Quote Lines

```bash
absuite quotes list lines --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Count Quote Lines

```bash
absuite quotes count lines --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Get Quote Line

```bash
absuite quotes get line --TenantId $TENANT_ID --QuoteLineId <line-guid>
```

### Create Quote Line

```bash
absuite quotes create line --TenantId $TENANT_ID --QuoteLineCreateDto '{
  "QuoteId": "<quote-guid>",
  "ItemId": "<item-guid>",
  "Quantity": 5,
  "UnitPrice": 200.00,
  "Description": "Design consultation hours"
}'
```

### Upsert Quote Line (Create or Update)

```bash
absuite quotes upsert line --TenantId $TENANT_ID --QuoteLineUpsertDto '{
  "QuoteId": "<quote-guid>",
  "ItemId": "<item-guid>",
  "Quantity": 10
}'
```

### Update Quote Line

```bash
absuite quotes update line --TenantId $TENANT_ID --QuoteLineId <line-guid> --QuoteLineUpdateDto '{...}'
```

### Delete Quote Line

```bash
absuite quotes delete line --TenantId $TENANT_ID --QuoteLineId <line-guid>
```

### Check if Quote Line Exists

```bash
absuite quotes line-exists --TenantId $TENANT_ID --QuoteLineId <line-guid>
```

## Calculations

### Calculate Quote Totals

```bash
absuite quotes calculate --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Calculate a Single Line

```bash
absuite quotes calculate line --TenantId $TENANT_ID --QuoteLineId <line-guid>
```

## Quote Lifecycle

### Close Quote

```bash
absuite quotes close --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Reopen Quote

```bash
absuite quotes reopen --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Create Order from Quote

```bash
absuite quotes create order-from --TenantId $TENANT_ID --QuoteId <quote-guid>
```

## Email

### Preview Email Template

```bash
absuite quotes preview email-template --TenantId $TENANT_ID --QuoteId <quote-guid>
```

### Send Quote Email

```bash
absuite quotes send email --TenantId $TENANT_ID --QuoteId <quote-guid> --EmailDispatchRequest '{
  "title": "Your Quote from Acme Corp",
  "message": "Please find your quote attached.",
  "culture": "en-US"
}'
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List quotes | `absuite quotes list --TenantId <guid>` |
| Create quote | `absuite quotes create --TenantId <guid> --QuoteCreateDto '{...}'` |
| Add line | `absuite quotes create line --TenantId <guid> --QuoteLineCreateDto '{...}'` |
| Calculate | `absuite quotes calculate --TenantId <guid> --QuoteId <guid>` |
| Close | `absuite quotes close --TenantId <guid> --QuoteId <guid>` |
| Reopen | `absuite quotes reopen --TenantId <guid> --QuoteId <guid>` |
| Convert to order | `absuite quotes create order-from --TenantId <guid> --QuoteId <guid>` |
| Send email | `absuite quotes send email --TenantId <guid> --QuoteId <guid> --EmailDispatchRequest '{...}'` |

## Full Example: Quote-to-Order Flow

```bash
# 1. Create a quote
absuite quotes create --QuoteCreateDto '{
  "Title": "Q-2026-042",
  "IndividualId": "<contact-guid>",
  "CurrencyId": "<currency-guid>",
  "EffectiveFrom": "2026-04-19T00:00:00Z",
  "EffectiveTo": "2026-05-19T00:00:00Z"
}'

# 2. Add lines
absuite quotes create line --QuoteLineCreateDto '{
  "QuoteId": "<quote-guid>",
  "ItemId": "<item-guid>",
  "Quantity": 5,
  "UnitPrice": 200.00
}'

# 3. Calculate totals
absuite quotes calculate --QuoteId <quote-guid>

# 4. Send to customer
absuite quotes send email --QuoteId <quote-guid> --EmailDispatchRequest '{
  "title": "Your Quote",
  "message": "Please review the attached quote."
}'

# 5. Close and convert to order
absuite quotes close --QuoteId <quote-guid>
absuite quotes create order-from --QuoteId <quote-guid>
```

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Calculate after adding/modifying lines** to update totals.
- **Close before converting to order** — use the lifecycle commands in order.
- **Use `upsert line`** when you want to create-or-update a line in one call.
