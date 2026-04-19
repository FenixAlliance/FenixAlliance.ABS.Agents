---
name: absuite-invoicing
description: >
  Create, read, update, delete, and manage invoices in the Alliance Business Suite
  (ABS) Invoicing Service using the `absuite` CLI. Covers invoices, invoice lines,
  line taxes, adjustments (discounts/surcharges), references (credit/debit notes),
  payments, calculations, aggregations, and transactional email notifications.
  Requires an authenticated CLI session (use the `absuite-login` skill to authenticate first).
---

# Alliance Business Suite — Invoicing Skill

Manage invoices through the `absuite` CLI's `invoicing` service. All invoicing operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant** — all invoicing operations require a tenant. Either set a default:
   ```bash
   absuite config set --tenant-id <tenant-guid>
   ```
   Or pass `--TenantId <guid>` on each call.
3. **Discover commands** — run `absuite invoicing list-commands` to see all invoicing commands, or use `--help` on any command for full parameter and output schemas.

## Command Discovery

```bash
# List all invoicing commands
absuite invoicing list-commands

# Get detailed help for any command
absuite invoicing create invoice --help
```

## Key Concepts

- **Invoice** — a formal billing document with header info (customer, currency, totals, status), line items, adjustments, and references.
- **Invoice Line** — an individual item/service billed, with quantity, pricing, and tax info.
- **Invoice Line Tax** — a tax applied to a specific invoice line (linked to a tax policy).
- **Invoice Adjustment** — a discount or surcharge applied at the invoice level (percentage or fixed amount).
- **Invoice Reference** — links between invoices (e.g., a credit note referencing the original invoice).
- **Invoice Payment** — payment records associated with an invoice.
- **InvoiceType** — `"Standard"`, `"CreditNote"`, `"DebitNote"`, `"Proforma"`.
- **DocumentType** — document classification for regulatory compliance.
- **InvoiceStatus** — lifecycle state: `"Draft"`, `"Sent"`, `"Paid"`, `"Overdue"`, `"Cancelled"`, `"Void"`.
- **Enumeration** — invoice number/sequence (auto-generated or from an enumeration range).

## Workflow: Creating an Invoice

1. **Create the invoice header** with customer, currency, and billing info
2. **Add invoice lines** for each billed item/service
3. **Add line taxes** to each line (link to tax policies)
4. **(Optional)** Add adjustments (global discounts/surcharges)
5. **Calculate totals** — let the server compute taxes, discounts, and grand total
6. **(Optional)** Add references to related invoices
7. **Send the invoice** via transactional email

## CRUD Operations

### Create Invoice

```bash
absuite invoicing create invoice --TenantId $TENANT_ID --InvoiceCreateDto '{
  "Title": "Invoice - Acme Corp Q2 2026",
  "Description": "Enterprise license and support services",
  "CurrencyId": "<currency-guid>",
  "IndividualId": "<contact-guid>",
  "OrganizationId": "<organization-guid>",
  "FirstName": "John",
  "LastName": "Doe",
  "CompanyName": "Acme Corp",
  "BillingEmail": "billing@acme.com",
  "AddressLine1": "123 Main St",
  "PostalCode": "10001",
  "CountryId": "<country-guid>",
  "StateId": "<state-guid>",
  "CityId": "<city-guid>",
  "InvoiceType": "Standard",
  "InvoiceStatus": "Draft",
  "CostCalculationMethod": "PerLine",
  "TaxCalculationMethod": "PerLine",
  "PaymentDue": "2026-07-15T00:00:00Z",
  "ValidFrom": "2026-04-19T00:00:00Z",
  "ValidTo": "2026-07-15T00:00:00Z",
  "Notes": "Net 30 payment terms apply.",
  "OrderId": "<order-guid>"
}'
```

**Required fields:**
- `Title` — invoice title
- `CurrencyId` — billing currency

**Recommended fields:**
- `IndividualId` or `OrganizationId` — CRM contact/organization
- `BillingEmail` — recipient for invoice emails
- Billing address — `FirstName`, `LastName`, `CompanyName`, `AddressLine1`, `CountryId`, etc.
- `InvoiceType` — `"Standard"`, `"CreditNote"`, `"DebitNote"`, `"Proforma"`
- `InvoiceStatus` — initial status (`"Draft"`)
- `PaymentDue` — payment due date
- `CostCalculationMethod` / `TaxCalculationMethod` — `"PerLine"` or `"PerInvoice"`

**Optional fields:**
- `OrderId` — link to an originating order
- `Number` — explicit invoice number (auto-assigned if omitted)
- `Enumeration` — invoice numbering string
- `EnumerationRangeId` — numbering range reference
- `PaymentModeId` — preferred payment method
- `EmisorBillingProfileId`, `ReceiverBillingProfileId` — billing profile references
- `EmisorWalletAccountId`, `ReceiverWalletAccountId` — wallet accounts for payment
- `PriceListId`, `PaymentTermId` — pricing and terms
- `ValidFrom`, `ValidTo` — invoice validity dates
- `Notes` — internal notes
- `CustomerNotes` — customer-visible notes
- `DocumentType` — regulatory document classification
- `InvoiceLines` — inline array of invoice lines
- `InvoiceReferences` — inline array of invoice references
- `InvoiceAdjustments` — inline array of adjustments

### Create Invoice with Inline Lines

```bash
absuite invoicing create invoice --TenantId $TENANT_ID --InvoiceCreateDto '{
  "Title": "Invoice #1042 - Acme Corp",
  "CurrencyId": "<currency-guid>",
  "IndividualId": "<contact-guid>",
  "InvoiceType": "Standard",
  "InvoiceStatus": "Draft",
  "PaymentDue": "2026-07-15T00:00:00Z",
  "CostCalculationMethod": "PerLine",
  "TaxCalculationMethod": "PerLine",
  "InvoiceLines": [
    {
      "ItemId": "<item-guid-1>",
      "Quantity": 5,
      "CurrencyId": "<currency-guid>",
      "ItemPriceId": "<price-guid-1>",
      "Title": "ABS Enterprise License",
      "Description": "5 seats, annual"
    },
    {
      "ItemId": "<item-guid-2>",
      "Quantity": 1,
      "CurrencyId": "<currency-guid>",
      "ItemPriceId": "<price-guid-2>",
      "Title": "Premium Support",
      "Description": "Annual support plan"
    }
  ],
  "InvoiceAdjustments": [
    {
      "Description": "Volume discount",
      "DiscountPercent": 10,
      "Type": "Discount"
    }
  ]
}'
```

### List Invoices

```bash
absuite invoicing list invoices --TenantId $TENANT_ID
```

### List Extended Invoices (with related data)

```bash
absuite invoicing list extended-invoices --TenantId $TENANT_ID
```

### Count Invoices

```bash
absuite invoicing count invoices --TenantId $TENANT_ID
```

### Count Extended Invoices

```bash
absuite invoicing count extended-invoices --TenantId $TENANT_ID
```

### Get Invoice by ID

```bash
absuite invoicing get invoice --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Get Extended Invoice

```bash
absuite invoicing get extended-invoice --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Update Invoice

```bash
absuite invoicing update invoice --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceUpdateDto '{
  "InvoiceStatus": "Sent",
  "Notes": "Sent to client on 2026-04-19"
}'
```

### Delete Invoice

```bash
absuite invoicing delete invoice --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

## Invoice Lines

### Add a Line

```bash
absuite invoicing create invoice-line --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineCreateDto '{
  "Title": "ABS Enterprise License",
  "Description": "Annual platform license, 10 seats",
  "ItemId": "<item-guid>",
  "ItemPriceId": "<price-guid>",
  "Quantity": 10,
  "CurrencyId": "<currency-guid>",
  "CostCalculationMethod": "PerLine",
  "TaxCalculationMethod": "PerLine"
}'
```

### List Lines

```bash
absuite invoicing list invoice-lines --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Count Lines

```bash
absuite invoicing count invoice-lines --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Get Line by ID

```bash
absuite invoicing get invoice-line --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid>
```

### Update Line

```bash
absuite invoicing update invoice-line --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid> --InvoiceLineUpdateDto '{
  "Quantity": 15,
  "Description": "Increased to 15 seats"
}'
```

### Delete Line

```bash
absuite invoicing delete invoice-line --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid>
```

## Invoice Line Taxes

Apply tax policies to individual invoice lines.

### Add Tax to a Line

```bash
absuite invoicing create invoice-line-tax --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid> --InvoiceLineAppliedTaxCreateDto '{
  "TaxPolicyId": "<tax-policy-guid>",
  "InvoiceId": "<invoice-guid>"
}'
```

### List Taxes for a Line

```bash
absuite invoicing list invoice-line-taxes --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid>
```

### Count Taxes for a Line

```bash
absuite invoicing count invoice-line-taxes --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid>
```

### Update a Line Tax

```bash
absuite invoicing update invoice-line-tax --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid> --InvoiceLineAppliedTaxId <tax-guid> --InvoiceLineAppliedTaxUpdateDto '{
  "TaxPolicyId": "<new-tax-policy-guid>"
}'
```

### Remove a Line Tax

```bash
absuite invoicing delete invoice-line-tax --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid> --InvoiceLineAppliedTaxId <tax-guid>
```

## Adjustments (Discounts / Surcharges)

Apply invoice-level discounts or surcharges.

### Create Adjustment

```bash
# Percentage discount
absuite invoicing create invoice-adjustment --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceAdjustmentCreateDto '{
  "Description": "Early payment discount",
  "DiscountPercent": 5,
  "Type": "Discount",
  "CurrencyId": "<currency-guid>"
}'

# Fixed surcharge
absuite invoicing create invoice-adjustment --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceAdjustmentCreateDto '{
  "Description": "Rush processing fee",
  "SurchargeAmount": 150.00,
  "Type": "Surcharge",
  "CurrencyId": "<currency-guid>"
}'
```

### List Adjustments

```bash
absuite invoicing list invoice-adjustments --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Count Adjustments

```bash
absuite invoicing count invoice-adjustments --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Get Adjustment by ID

```bash
absuite invoicing get invoice-adjustment --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceAdjustmentId <adjustment-guid>
```

### Update Adjustment

```bash
absuite invoicing update invoice-adjustment --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceAdjustmentId <adjustment-guid> --InvoiceAdjustmentUpdateDto '{
  "DiscountPercent": 8,
  "Description": "Loyalty discount increased"
}'
```

### Delete Adjustment

```bash
absuite invoicing delete invoice-adjustment --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceAdjustmentId <adjustment-guid>
```

## Invoice References

Link invoices together (e.g., credit note → original invoice).

### Create Reference

```bash
absuite invoicing create invoice-reference --TenantId $TENANT_ID --InvoiceId <credit-note-guid> --InvoiceReferenceCreateDto '{
  "ReferencedInvoiceId": "<original-invoice-guid>"
}'
```

### List References

```bash
absuite invoicing list invoice-references --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Count References

```bash
absuite invoicing count invoice-references --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Get Reference by ID

```bash
absuite invoicing get invoice-reference --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceReferenceId <reference-guid>
```

### Update Reference

```bash
absuite invoicing update invoice-reference --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceReferenceId <reference-guid> --InvoiceReferenceUpdateDto '{
  "ReferencedInvoiceId": "<different-invoice-guid>"
}'
```

### Delete Reference

```bash
absuite invoicing delete invoice-reference --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceReferenceId <reference-guid>
```

## Payments

### List Payments for an Invoice

```bash
absuite invoicing list invoice-payments --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Count Payments

```bash
absuite invoicing count invoice-payments --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

## Calculations

Always recalculate after adding/modifying lines, taxes, or adjustments.

### Calculate Full Invoice

```bash
absuite invoicing calculate invoice --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Calculate a Single Line

```bash
absuite invoicing calculate invoice-line --TenantId $TENANT_ID --InvoiceId <invoice-guid> --InvoiceLineId <line-guid>
```

## Aggregations

Get aggregated financial summaries across all invoices.

```bash
# Aggregate totals
absuite invoicing aggregate-invoice-totals --TenantId $TENANT_ID

# Aggregate taxes
absuite invoicing aggregate-invoice-taxes --TenantId $TENANT_ID

# Aggregate tax bases
absuite invoicing aggregate-invoice-tax-bases --TenantId $TENANT_ID

# Aggregate discounts
absuite invoicing aggregate-invoice-discounts --TenantId $TENANT_ID

# Aggregate global surcharges
absuite invoicing aggregate-invoice-global-surcharges --TenantId $TENANT_ID
```

## Email Notifications

### Preview Invoice Email

```bash
absuite invoicing preview invoice-email --TenantId $TENANT_ID --InvoiceId <invoice-guid>
```

### Send Invoice Email

```bash
absuite invoicing send invoice-email --TenantId $TENANT_ID --InvoiceId <invoice-guid> --EmailDispatchRequest '{
  "Title": "Invoice #1042 - Acme Corp",
  "Message": "Please find your invoice attached. Payment is due by July 15, 2026.",
  "Recipients": ["billing@acme.com"],
  "Culture": "en-US"
}'
```

**EmailDispatchRequest fields:**
- `Title` — email subject line
- `Message` — email body text
- `Recipients` — array of email addresses
- `ContactIds` — array of CRM contact GUIDs (sends to their email)
- `TenantIds` — array of tenant GUIDs
- `UserIds` — array of user GUIDs
- `Culture` / `UiCulture` — email locale
- `EmailTemplateId` — use a specific email template
- `ButtonLink`, `ButtonText` — optional CTA button
- `AlertMessage`, `AlertType` — optional alert banner

## Creating a Credit Note

```bash
# 1. Create a credit note invoice referencing the original
absuite invoicing create invoice --TenantId $TENANT_ID --InvoiceCreateDto '{
  "Title": "Credit Note for Invoice #1042",
  "CurrencyId": "<currency-guid>",
  "IndividualId": "<contact-guid>",
  "InvoiceType": "CreditNote",
  "InvoiceStatus": "Draft",
  "InvoiceReferences": [
    { "ReferencedInvoiceId": "<original-invoice-guid>" }
  ]
}'

# 2. Add lines reflecting the credited items
absuite invoicing create invoice-line --TenantId $TENANT_ID --InvoiceId <credit-note-guid> --InvoiceLineCreateDto '{
  "Title": "ABS Enterprise License (Credit)",
  "ItemId": "<item-guid>",
  "Quantity": -2,
  "CurrencyId": "<currency-guid>",
  "ItemPriceId": "<price-guid>"
}'

# 3. Calculate
absuite invoicing calculate invoice --InvoiceId <credit-note-guid>

# 4. Send
absuite invoicing send invoice-email --InvoiceId <credit-note-guid> --EmailDispatchRequest '{
  "Title": "Credit Note for Invoice #1042",
  "Message": "A credit note has been issued for your account.",
  "Recipients": ["billing@acme.com"]
}'
```

## Full Example: End-to-End Invoice Creation

```bash
# 1. Authenticate
absuite login --email billing@company.com

# 2. Set tenant
absuite config set --tenant-id 00000000-0000-0000-0000-000000000000

# 3. Create the invoice
absuite invoicing create invoice --InvoiceCreateDto '{
  "Title": "Invoice #1042 - Acme Corp",
  "Description": "Q2 2026 Enterprise License",
  "CurrencyId": "<currency-guid>",
  "IndividualId": "<contact-guid>",
  "CompanyName": "Acme Corp",
  "BillingEmail": "billing@acme.com",
  "FirstName": "John",
  "LastName": "Doe",
  "InvoiceType": "Standard",
  "InvoiceStatus": "Draft",
  "PaymentDue": "2026-07-15T00:00:00Z",
  "CostCalculationMethod": "PerLine",
  "TaxCalculationMethod": "PerLine",
  "OrderId": "<order-guid>"
}'
# Note the returned invoice ID

# 4. Add line items
absuite invoicing create invoice-line --InvoiceId <invoice-id> --InvoiceLineCreateDto '{
  "Title": "ABS Enterprise License",
  "ItemId": "<item-guid>",
  "Quantity": 10,
  "CurrencyId": "<currency-guid>",
  "ItemPriceId": "<price-guid>"
}'

absuite invoicing create invoice-line --InvoiceId <invoice-id> --InvoiceLineCreateDto '{
  "Title": "Premium Support (Annual)",
  "ItemId": "<support-item-guid>",
  "Quantity": 1,
  "CurrencyId": "<currency-guid>",
  "ItemPriceId": "<support-price-guid>"
}'

# 5. Apply taxes to each line
absuite invoicing create invoice-line-tax --InvoiceId <invoice-id> --InvoiceLineId <line-1-guid> --InvoiceLineAppliedTaxCreateDto '{
  "TaxPolicyId": "<vat-policy-guid>",
  "InvoiceId": "<invoice-id>"
}'

# 6. Add a volume discount
absuite invoicing create invoice-adjustment --InvoiceId <invoice-id> --InvoiceAdjustmentCreateDto '{
  "Description": "Volume discount (10+ seats)",
  "DiscountPercent": 10,
  "Type": "Discount",
  "CurrencyId": "<currency-guid>"
}'

# 7. Calculate totals
absuite invoicing calculate invoice --InvoiceId <invoice-id>

# 8. Update status to Sent
absuite invoicing update invoice --InvoiceId <invoice-id> --InvoiceUpdateDto '{
  "InvoiceStatus": "Sent"
}'

# 9. Send invoice email
absuite invoicing send invoice-email --InvoiceId <invoice-id> --EmailDispatchRequest '{
  "Title": "Invoice #1042 - Acme Corp",
  "Message": "Please find your invoice below. Payment due: July 15, 2026.",
  "Recipients": ["billing@acme.com"],
  "Culture": "en-US"
}'

# 10. Verify
absuite invoicing get invoice --InvoiceId <invoice-id>
```
