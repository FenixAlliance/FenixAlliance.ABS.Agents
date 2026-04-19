---
name: absuite-payments
description: >
  Manage payments, payment methods, payment modes, and payment terms in the Alliance
  Business Suite (ABS) using the `absuite` CLI. Covers payment CRUD and configuration
  of payment infrastructure. Requires an authenticated CLI session.
---

# Alliance Business Suite — Payments Skill

Manage payments through the `absuite` CLI's `payments` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite payments list-commands`

## Payments

### List Payments

```bash
absuite payments list --TenantId $TENANT_ID
```

### Get Payment by ID

```bash
absuite payments get async-v2 --TenantId $TENANT_ID --PaymentId <payment-guid>
```

### Create Payment

```bash
absuite payments create --TenantId $TENANT_ID --PaymentCreateDto '{
  "InvoiceId": "<invoice-guid>",
  "TotalCost": 1500.00,
  "TotalTaxes": 120.00,
  "CurrencyId": "<currency-guid>",
  "EmisorWalletId": "<sender-wallet-guid>",
  "ReceiverWalletId": "<receiver-wallet-guid>",
  "PaymentType": "CreditCard",
  "PaymentStatus": "Completed",
  "ReferenceCode": "PAY-2026-001",
  "CountryId": "USA",
  "BankId": "<bank-guid>",
  "BankAccountId": "<bank-account-guid>"
}'
```

**Key PaymentCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `InvoiceId` | String | Linked invoice |
| `OrderId` | String | Linked order |
| `TotalCost` | Double | Payment amount |
| `TotalTaxes` | Double | Tax amount |
| `CurrencyId` | String | Currency |
| `ForexRate` | Double | Exchange rate at time of payment |
| `EmisorWalletId` | String | Sender wallet |
| `ReceiverWalletId` | String | Receiver wallet |
| `PaymentType` | String | Type (e.g., CreditCard, BankTransfer, Cash) |
| `PaymentStatus` | String | Status (e.g., Pending, Completed, Failed) |
| `ReferenceCode` | String | External reference |
| `Authorization` | String | Auth code from gateway |
| `PaymentGatewayId` | String | Gateway used |
| `BankId` / `BankAccountId` | String | Bank details |
| `Closed` | Boolean | Whether payment is finalized |
| `IsExternal` | Boolean | External payment flag |

### Update Payment

```bash
absuite payments update --TenantId $TENANT_ID --PaymentId <payment-guid> --PaymentUpdateDto '{
  "PaymentStatus": "Completed",
  "Closed": true
}'
```

### Delete Payment

```bash
absuite payments delete --TenantId $TENANT_ID --PaymentId <payment-guid>
```

## Payment Methods

Configuration for accepted payment methods (e.g., Visa, MasterCard, PayPal).

```bash
# List
absuite payments list methods --TenantId $TENANT_ID

# Count
absuite payments count methods --TenantId $TENANT_ID

# Get by ID
absuite payments list method-details --TenantId $TENANT_ID --PaymentMethodId <method-guid>

# Create
absuite payments create method --TenantId $TENANT_ID --PaymentMethodCreateDto '{
  "Name": "Credit Card",
  "Description": "Visa/MasterCard payments"
}'

# Update
absuite payments update method --TenantId $TENANT_ID --PaymentMethodId <method-guid> --PaymentMethodUpdateDto '{...}'

# Delete
absuite payments delete method --TenantId $TENANT_ID --PaymentMethodId <method-guid>
```

## Payment Modes

Payment delivery mechanisms (e.g., Online, In-Person, Wire Transfer).

```bash
# List
absuite payments list modes --TenantId $TENANT_ID

# Count
absuite payments count modes --TenantId $TENANT_ID

# Get by ID
absuite payments list mode-details --TenantId $TENANT_ID --PaymentModeId <mode-guid>

# Create
absuite payments create mode --TenantId $TENANT_ID --PaymentModeCreateDto '{
  "Name": "Online Payment"
}'

# Update
absuite payments update mode --TenantId $TENANT_ID --PaymentModeId <mode-guid> --PaymentModeUpdateDto '{...}'

# Delete
absuite payments delete mode --TenantId $TENANT_ID --PaymentModeId <mode-guid>
```

## Payment Terms

Contractual payment conditions (e.g., Net 30, Net 60, COD).

```bash
# List
absuite payments list terms --TenantId $TENANT_ID

# Count
absuite payments count terms --TenantId $TENANT_ID

# Get by ID
absuite payments list term-details --TenantId $TENANT_ID --PaymentTermId <term-guid>

# Create
absuite payments create term --TenantId $TENANT_ID --PaymentTermCreateDto '{
  "Name": "Net 30",
  "Description": "Payment due within 30 days"
}'

# Update
absuite payments update term --TenantId $TENANT_ID --PaymentTermId <term-guid> --PaymentTermUpdateDto '{...}'

# Delete
absuite payments delete term --TenantId $TENANT_ID --PaymentTermId <term-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List payments | `absuite payments list --TenantId <guid>` |
| Get payment | `absuite payments get async-v2 --TenantId <guid> --PaymentId <guid>` |
| Create payment | `absuite payments create --TenantId <guid> --PaymentCreateDto '{...}'` |
| List methods | `absuite payments list methods --TenantId <guid>` |
| List modes | `absuite payments list modes --TenantId <guid>` |
| List terms | `absuite payments list terms --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Set up methods, modes, and terms** before creating payments.
- **Link payments to invoices/orders** via `InvoiceId`/`OrderId`.
- **Use `--help`** on any command for full DTO schemas.
