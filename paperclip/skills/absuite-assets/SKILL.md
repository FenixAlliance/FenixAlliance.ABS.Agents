---
name: absuite-assets
description: >
  Manage fixed and moveable assets in the Alliance Business Suite (ABS) using the
  `absuite` CLI. Covers asset CRUD, categories, types, depreciation records,
  repairs, value amendments, and asset transfers. Requires an authenticated CLI
  session (use the `absuite-login` skill to authenticate first).
---

# Alliance Business Suite — Assets Skill

Manage assets through the `absuite` CLI's `assets` service. All operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite assets list-commands`

## Asset CRUD

### List Assets

```bash
absuite assets list --TenantId $TENANT_ID
```

### Count Assets

```bash
absuite assets count --TenantId $TENANT_ID
```

### Get Asset by ID

```bash
absuite assets get --TenantId $TENANT_ID --AssetId <asset-guid>
```

### Create Asset

```bash
absuite assets create --TenantId $TENANT_ID --AssetCreateDto '{
  "Name": "Office Laptop - Dell XPS 15",
  "Description": "Employee workstation",
  "AssetClass": "Equipment",
  "AssetOwner": "IT Department",
  "PurchaseDate": "2026-01-15T00:00:00Z",
  "PurchasePrice": 2500.00,
  "CurrencyId": "<currency-guid>",
  "AssetCategoryId": "<category-guid>",
  "CalculateDepreciation": true,
  "IsExistingAsset": false,
  "ContactId": "<assigned-contact-guid>",
  "OrganizationDepartmentId": "<dept-guid>"
}'
```

**Key AssetCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `Name` | String | Asset name |
| `Description` | String | Asset description |
| `AssetClass` | String | Classification (e.g., Equipment, Vehicle, Furniture) |
| `AssetOwner` | String | Owner description |
| `PurchaseDate` | DateTime | Purchase date |
| `PurchasePrice` | Double | Purchase price |
| `CurrencyId` | String | Currency |
| `AssetCategoryId` | String | Link to asset category |
| `ItemId` | String | Link to catalog stock item |
| `CalculateDepreciation` | Boolean | Enable depreciation |
| `AllowMonthlyDepreciation` | Boolean | Monthly depreciation |
| `OpeningDepreciation` | Double | Initial depreciation value |
| `AssetLocationId` | String | Physical location |
| `ContactId` | String | Responsible contact |
| `OrganizationDepartmentId` | String | Department |
| `PurchaseInvoiceId` | String | Linked purchase invoice |
| `PurchaseReceiptId` | String | Linked purchase receipt |

### Update Asset

```bash
absuite assets update --TenantId $TENANT_ID --AssetId <asset-guid> --AssetUpdateDto '{
  "Name": "Office Laptop - Dell XPS 15 (Upgraded)",
  "Description": "Upgraded RAM to 32GB"
}'
```

### Delete Asset

```bash
absuite assets delete --TenantId $TENANT_ID --AssetId <asset-guid>
```

## Asset Categories

```bash
# List
absuite assets list categories --TenantId $TENANT_ID

# Count
absuite assets count categories --TenantId $TENANT_ID

# Get by ID
absuite assets get category --TenantId $TENANT_ID --AssetCategoryId <category-guid>

# Create
absuite assets create category --TenantId $TENANT_ID --AssetCategoryCreateDto '{
  "Name": "IT Equipment",
  "Description": "Laptops, desktops, servers, peripherals"
}'

# Update
absuite assets update category --TenantId $TENANT_ID --AssetCategoryId <category-guid> --AssetCategoryUpdateDto '{...}'

# Delete
absuite assets delete category --TenantId $TENANT_ID --AssetCategoryId <category-guid>
```

## Asset Types

```bash
# List
absuite assets list types --TenantId $TENANT_ID

# Count
absuite assets count types --TenantId $TENANT_ID

# Get by ID
absuite assets get type --TenantId $TENANT_ID --AssetTypeId <type-guid>

# Create
absuite assets create type --TenantId $TENANT_ID --AssetTypeCreateDto '{
  "Name": "Tangible Fixed Asset"
}'

# Update
absuite assets update type --TenantId $TENANT_ID --AssetTypeId <type-guid> --AssetTypeUpdateDto '{...}'

# Delete
absuite assets delete type --TenantId $TENANT_ID --AssetTypeId <type-guid>
```

## Depreciation Records

Track depreciation over time for an asset.

```bash
# List
absuite assets list depreciation-records --TenantId $TENANT_ID --AssetId <asset-guid>

# Count
absuite assets count depreciation-records --TenantId $TENANT_ID --AssetId <asset-guid>

# Get
absuite assets get depreciation-record --TenantId $TENANT_ID --AssetId <asset-guid> --DepreciationRecordId <record-guid>

# Create
absuite assets create depreciation-record --TenantId $TENANT_ID --AssetId <asset-guid> --DepreciationRecordCreateDto '{
  "Description": "Annual depreciation 2026",
  "Amount": 500.00,
  "Date": "2026-12-31T00:00:00Z"
}'

# Update
absuite assets update depreciation-record --TenantId $TENANT_ID --AssetId <asset-guid> --DepreciationRecordId <record-guid> --DepreciationRecordUpdateDto '{...}'

# Delete
absuite assets delete depreciation-record --TenantId $TENANT_ID --AssetId <asset-guid> --DepreciationRecordId <record-guid>
```

## Repairs

Track maintenance and repair history for an asset.

```bash
# List
absuite assets list repairs --TenantId $TENANT_ID --AssetId <asset-guid>

# Count
absuite assets count repairs --TenantId $TENANT_ID --AssetId <asset-guid>

# Get
absuite assets get repair --TenantId $TENANT_ID --AssetId <asset-guid> --RepairId <repair-guid>

# Create
absuite assets create repair --TenantId $TENANT_ID --AssetId <asset-guid> --RepairCreateDto '{
  "Description": "Screen replacement",
  "Cost": 350.00,
  "Date": "2026-06-15T00:00:00Z"
}'

# Update
absuite assets update repair --TenantId $TENANT_ID --AssetId <asset-guid> --RepairId <repair-guid> --RepairUpdateDto '{...}'

# Delete
absuite assets delete repair --TenantId $TENANT_ID --AssetId <asset-guid> --RepairId <repair-guid>
```

## Value Amendments

Record value changes (appreciation/impairment) for an asset.

```bash
# List
absuite assets list value-amends --TenantId $TENANT_ID --AssetId <asset-guid>

# Count
absuite assets count value-amends --TenantId $TENANT_ID --AssetId <asset-guid>

# Get
absuite assets get value-amend --TenantId $TENANT_ID --AssetId <asset-guid> --ValueAmendId <amend-guid>

# Create
absuite assets create value-amend --TenantId $TENANT_ID --AssetId <asset-guid> --ValueAmendCreateDto '{
  "Description": "Market revaluation",
  "Amount": -200.00,
  "Date": "2026-09-30T00:00:00Z"
}'

# Update
absuite assets update value-amend --TenantId $TENANT_ID --AssetId <asset-guid> --ValueAmendId <amend-guid> --ValueAmendUpdateDto '{...}'

# Delete
absuite assets delete value-amend --TenantId $TENANT_ID --AssetId <asset-guid> --ValueAmendId <amend-guid>
```

## Asset Transfers

Transfer assets between locations, departments, or owners.

```bash
# List
absuite assets list transfers --TenantId $TENANT_ID

# Count
absuite assets count transfers --TenantId $TENANT_ID

# Get
absuite assets get transfer --TenantId $TENANT_ID --AssetTransferId <transfer-guid>

# Create
absuite assets create transfer --TenantId $TENANT_ID --AssetTransferCreateDto '{
  "AssetId": "<asset-guid>",
  "Description": "Transfer to marketing department"
}'

# Update
absuite assets update transfer --TenantId $TENANT_ID --AssetTransferId <transfer-guid> --AssetTransferUpdateDto '{...}'

# Delete
absuite assets delete transfer --TenantId $TENANT_ID --AssetTransferId <transfer-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List assets | `absuite assets list --TenantId <guid>` |
| Count assets | `absuite assets count --TenantId <guid>` |
| Get asset | `absuite assets get --TenantId <guid> --AssetId <guid>` |
| Create asset | `absuite assets create --TenantId <guid> --AssetCreateDto '{...}'` |
| Update asset | `absuite assets update --TenantId <guid> --AssetId <guid> --AssetUpdateDto '{...}'` |
| Delete asset | `absuite assets delete --TenantId <guid> --AssetId <guid>` |
| List categories | `absuite assets list categories --TenantId <guid>` |
| List types | `absuite assets list types --TenantId <guid>` |
| List depreciation | `absuite assets list depreciation-records --TenantId <guid> --AssetId <guid>` |
| List repairs | `absuite assets list repairs --TenantId <guid> --AssetId <guid>` |
| List value amends | `absuite assets list value-amends --TenantId <guid> --AssetId <guid>` |
| List transfers | `absuite assets list transfers --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Use `absuite login` before any asset operation.
- **Always provide a tenant context.** Set a default or pass `--TenantId` on each call.
- **Use `--help` before unfamiliar commands.** It shows exact parameter names and DTO schemas.
- **Set up categories and types first** before creating assets that reference them.
- **Save the asset `id` after creation.** Needed for depreciation, repairs, value amends, and transfers.
