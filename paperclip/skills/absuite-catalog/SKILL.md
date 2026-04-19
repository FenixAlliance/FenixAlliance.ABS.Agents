---
name: absuite-catalog
description: >
  Manage the product catalog in the Alliance Business Suite (ABS) using the
  `absuite` CLI. Covers stock items (products), categories, types, brands, tags,
  images, attachments, reviews, questions, pricing rules, attribute options, and
  policy relationships (tax, shipping, return, refund, warranty). Includes Google
  category mapping. Requires an authenticated CLI session.
---

# Alliance Business Suite — Catalog Skill

Manage the product catalog through the `absuite` CLI's `catalog` service. All operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite catalog list-commands`

## Stock Items (Products)

### List Stock Items

```bash
absuite catalog get stock-items-query --TenantId $TENANT_ID
```

### Count Stock Items

```bash
absuite catalog count stock-items-by-business --TenantId $TENANT_ID
```

### Get Stock Item by ID

```bash
absuite catalog get stock-item-by-id --TenantId $TENANT_ID --ItemId <item-guid>
```

### Get Extended Stock Item (with related data)

```bash
absuite catalog get extended-stock-item-by-id --TenantId $TENANT_ID --ItemId <item-guid>
```

### Create Stock Item

```bash
absuite catalog create stock-item --TenantId $TENANT_ID --CatalogItemCreateDto '{
  "Name": "Premium Widget",
  "Title": "Premium Widget - Professional Grade",
  "Sku": "WDG-PRO-001",
  "Description": "High-quality professional widget",
  "ShortDescription": "Pro-grade widget",
  "RegularPrice": 49.99,
  "CurrencyId": "<currency-guid>",
  "CategoryId": "<category-guid>",
  "ItemTypeId": "<type-guid>",
  "BrandId": "<brand-guid>",
  "InStock": true,
  "Published": true,
  "Taxable": true,
  "ManageInventory": true,
  "CurrentStock": 100.0,
  "Weight": 0.5,
  "SelectedCategories": ["<cat-guid-1>", "<cat-guid-2>"],
  "SelectedTags": ["<tag-guid-1>"],
  "SelectedTaxPolicies": ["<tax-policy-guid>"]
}'
```

**Key CatalogItemCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `Name` | String | Product name |
| `Title` | String | Display title |
| `Sku` / `Upc` / `Ean` | String | Product identifiers |
| `RegularPrice` | Double | Base price |
| `DiscountPrice` | Double | Sale price |
| `CurrencyId` | String | Currency |
| `CategoryId` | String | Primary category |
| `ItemTypeId` | String | Product type |
| `BrandId` | String | Brand |
| `InStock` / `Published` | Boolean | Availability flags |
| `Taxable` | Boolean | Subject to tax |
| `ManageInventory` | Boolean | Track stock levels |
| `CurrentStock` | Double | Quantity on hand |
| `Weight` / `Width` / `Height` / `Length` | Double | Dimensions |
| `PrimaryImageUrl` | String | Main product image URL |
| `Featured` / `OnSale` / `Hot` | Boolean | Merchandising flags |
| `SelectedCategories` | String[] | Linked category IDs |
| `SelectedTags` | String[] | Linked tag IDs |
| `SelectedTaxPolicies` | String[] | Linked tax policy IDs |

### Update Stock Item

```bash
absuite catalog update stock-item --TenantId $TENANT_ID --ItemId <item-guid> --CatalogItemUpdateDto '{
  "RegularPrice": 59.99,
  "OnSale": true,
  "DiscountPrice": 44.99
}'
```

### Delete Stock Item

```bash
absuite catalog delete stock-item --TenantId $TENANT_ID --ItemId <item-guid>
```

### Get Price Range

```bash
absuite catalog get stock-items-odata-min-price --TenantId $TENANT_ID
absuite catalog get stock-items-odata-max-price --TenantId $TENANT_ID
```

## Item Categories

```bash
# List
absuite catalog list item-categories --TenantId $TENANT_ID

# Count
absuite catalog count item-categories --TenantId $TENANT_ID

# Get
absuite catalog get item-category-by-id --TenantId $TENANT_ID --ItemCategoryId <category-guid>

# Create
absuite catalog create item-category --TenantId $TENANT_ID --ItemCategoryCreateDto '{
  "Name": "Electronics",
  "Description": "Electronic devices and accessories"
}'

# Update
absuite catalog update item-category --TenantId $TENANT_ID --ItemCategoryId <category-guid> --ItemCategoryUpdateDto '{...}'

# Delete
absuite catalog delete item-category --TenantId $TENANT_ID --ItemCategoryId <category-guid>
```

### Relate/Unrelate Category to Stock Item

```bash
absuite catalog relate-category-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemCategoryId <category-guid>
absuite catalog delete category-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemCategoryId <category-guid>

# View categories for an item
absuite catalog get stock-item-categories-by-item-id --TenantId $TENANT_ID --ItemId <item-guid>
```

## Item Types

```bash
absuite catalog list item-types --TenantId $TENANT_ID
absuite catalog count item-types --TenantId $TENANT_ID
absuite catalog get item-type-by-id --TenantId $TENANT_ID --ItemTypeId <type-guid>
absuite catalog create item-type --TenantId $TENANT_ID --ItemTypeCreateDto '{"Name": "Physical Product"}'
absuite catalog update item-type --TenantId $TENANT_ID --ItemTypeId <type-guid> --ItemTypeUpdateDto '{...}'
absuite catalog delete item-type --TenantId $TENANT_ID --ItemTypeId <type-guid>
```

## Item Brands

```bash
absuite catalog list item-brands --TenantId $TENANT_ID
absuite catalog get item-brand-by-id --TenantId $TENANT_ID --ItemBrandId <brand-guid>
absuite catalog create item-brand --TenantId $TENANT_ID --ItemBrandCreateDto '{"Name": "Acme"}'
absuite catalog update item-brand --TenantId $TENANT_ID --ItemBrandId <brand-guid> --ItemBrandUpdateDto '{...}'
absuite catalog delete item-brand --TenantId $TENANT_ID --ItemBrandId <brand-guid>

# Relate/unrelate
absuite catalog relate-brand-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemBrandId <brand-guid>
absuite catalog delete brand-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemBrandId <brand-guid>
```

## Item Tags

```bash
absuite catalog list item-tags --TenantId $TENANT_ID
absuite catalog get item-tag-by-id --TenantId $TENANT_ID --ItemTagId <tag-guid>
absuite catalog create item-tag --TenantId $TENANT_ID --ItemTagCreateDto '{"Name": "Best Seller"}'
absuite catalog update item-tag --TenantId $TENANT_ID --ItemTagId <tag-guid> --ItemTagUpdateDto '{...}'
absuite catalog delete item-tag --TenantId $TENANT_ID --ItemTagId <tag-guid>

# Relate/unrelate
absuite catalog relate-tag-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemTagId <tag-guid>
absuite catalog delete tag-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemTagId <tag-guid>

# Count tags for an item
absuite catalog count stock-item-tags-by-item-id --TenantId $TENANT_ID --ItemId <item-guid>
```

## Item Images

```bash
absuite catalog list item-images --TenantId $TENANT_ID
absuite catalog get item-image-by-id --TenantId $TENANT_ID --ItemImageId <image-guid>
absuite catalog create item-image --TenantId $TENANT_ID --ItemImageCreateDto '{...}'
absuite catalog update item-image --TenantId $TENANT_ID --ItemImageId <image-guid> --ItemImageUpdateDto '{...}'
absuite catalog delete item-image --TenantId $TENANT_ID --ItemImageId <image-guid>

# Relate to stock item
absuite catalog relate-image-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemImageId <image-guid>
absuite catalog delete image-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemImageId <image-guid>

# Primary image
absuite catalog get product-primary-image --TenantId $TENANT_ID --ItemId <item-guid>
absuite catalog update product-primary-image --TenantId $TENANT_ID --ItemId <item-guid> --PrimaryImageUpdateDto '{...}'
```

## Item Attachments

```bash
absuite catalog list item-attachments --TenantId $TENANT_ID
absuite catalog get item-attachment-by-id --TenantId $TENANT_ID --ItemAttachmentId <attachment-guid>
absuite catalog create item-attachment --TenantId $TENANT_ID --ItemAttachmentCreateDto '{...}'
absuite catalog update item-attachment --TenantId $TENANT_ID --ItemAttachmentId <attachment-guid> --ItemAttachmentUpdateDto '{...}'
absuite catalog delete item-attachment --TenantId $TENANT_ID --ItemAttachmentId <attachment-guid>

# Relate to stock item
absuite catalog relate-attachment-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemAttachmentId <attachment-guid>
absuite catalog delete attachment-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemAttachmentId <attachment-guid>
```

## Item Attributes & Options

```bash
absuite catalog list item-attributes --TenantId $TENANT_ID
absuite catalog count item-attributes --TenantId $TENANT_ID
absuite catalog get item-attribute-by-id --TenantId $TENANT_ID --ItemAttributeId <attr-guid>
absuite catalog create item-attribute --TenantId $TENANT_ID --ItemAttributeCreateDto '{"Name": "Color"}'
absuite catalog update item-attribute --TenantId $TENANT_ID --ItemAttributeId <attr-guid> --ItemAttributeUpdateDto '{...}'
absuite catalog delete item-attribute --TenantId $TENANT_ID --ItemAttributeId <attr-guid>

# Attribute options on stock items
absuite catalog get stock-item-attribute-options-by-item-id --TenantId $TENANT_ID --ItemId <item-guid>
absuite catalog relate-attribute-option-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --AttributeOptionId <option-guid>
absuite catalog delete attribute-option-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --AttributeOptionId <option-guid>
```

## Reviews & Questions

```bash
# Reviews
absuite catalog list item-reviews --TenantId $TENANT_ID
absuite catalog get item-review-by-id --TenantId $TENANT_ID --ItemReviewId <review-guid>
absuite catalog create item-review --TenantId $TENANT_ID --ItemReviewCreateDto '{...}'
absuite catalog relate-review-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemReviewId <review-guid>
absuite catalog get stock-item-reviews-by-item-id --TenantId $TENANT_ID --ItemId <item-guid>

# Questions
absuite catalog list item-questions --TenantId $TENANT_ID
absuite catalog get item-question-by-id --TenantId $TENANT_ID --ItemQuestionId <question-guid>
absuite catalog create item-question --TenantId $TENANT_ID --ItemQuestionCreateDto '{...}'
absuite catalog relate-question-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --ItemQuestionId <question-guid>
```

## Pricing Rules

```bash
absuite catalog list pricing-rules --TenantId $TENANT_ID
absuite catalog get pricing-rule-by-id --TenantId $TENANT_ID --PricingRuleId <rule-guid>
absuite catalog create pricing-rule --TenantId $TENANT_ID --PricingRuleCreateDto '{...}'
absuite catalog update pricing-rule --TenantId $TENANT_ID --PricingRuleId <rule-guid> --PricingRuleUpdateDto '{...}'
absuite catalog delete pricing-rule --TenantId $TENANT_ID --PricingRuleId <rule-guid>

# Relate to stock item
absuite catalog relate-price-rule-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --PricingRuleId <rule-guid>
absuite catalog delete price-rule-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --PricingRuleId <rule-guid>
```

## Policy Relationships

Attach policies to items or stock items:

```bash
# Tax policies
absuite catalog relate-item-to-tax-policy --TenantId $TENANT_ID --ItemId <item-guid> --TaxPolicyId <policy-guid>
absuite catalog delete tax-policy-from-item --TenantId $TENANT_ID --ItemId <item-guid> --TaxPolicyId <policy-guid>
absuite catalog get stock-item-tax-policies-by-item-id --TenantId $TENANT_ID --ItemId <item-guid>

# Shipping policies
absuite catalog relate-item-to-shipping-policy --TenantId $TENANT_ID --ItemId <item-guid> --ShippingPolicyId <policy-guid>
absuite catalog delete shipping-policy-from-item --TenantId $TENANT_ID --ItemId <item-guid> --ShippingPolicyId <policy-guid>

# Return policies
absuite catalog relate-item-to-return-policy --TenantId $TENANT_ID --ItemId <item-guid> --ReturnPolicyId <policy-guid>
absuite catalog delete return-policy-from-item --TenantId $TENANT_ID --ItemId <item-guid> --ReturnPolicyId <policy-guid>

# Refund policies
absuite catalog relate-item-to-refund-policy --TenantId $TENANT_ID --ItemId <item-guid> --RefundPolicyId <policy-guid>
absuite catalog delete refund-policy-from-item --TenantId $TENANT_ID --ItemId <item-guid> --RefundPolicyId <policy-guid>

# Warranty policies
absuite catalog relate-item-to-warranty-policy --TenantId $TENANT_ID --ItemId <item-guid> --WarrantyPolicyId <policy-guid>
absuite catalog delete warranty-policy-from-item --TenantId $TENANT_ID --ItemId <item-guid> --WarrantyPolicyId <policy-guid>
```

## Google Categories

```bash
absuite catalog list item-google-categories --TenantId $TENANT_ID
absuite catalog list root-item-google-categories --TenantId $TENANT_ID
absuite catalog list all-item-google-categories --TenantId $TENANT_ID
absuite catalog get item-google-category-by-id --TenantId $TENANT_ID --GoogleCategoryId <gcat-guid>
absuite catalog get children-item-google-categories-by-id --TenantId $TENANT_ID --GoogleCategoryId <gcat-guid>
absuite catalog get item-google-categories-tree --TenantId $TENANT_ID
absuite catalog map-item-google-categories-tree --TenantId $TENANT_ID

# Relate to stock item
absuite catalog relate-google-category-to-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --GoogleCategoryId <gcat-guid>
absuite catalog delete google-category-from-stock-item --TenantId $TENANT_ID --ItemId <item-guid> --GoogleCategoryId <gcat-guid>
```

## Merchants

```bash
absuite catalog list merchants --TenantId $TENANT_ID
absuite catalog count merchants --TenantId $TENANT_ID
absuite catalog get merchant-by-id --TenantId $TENANT_ID --MerchantId <merchant-guid>
```

## Critical Rules

- **Authenticate first.** Use `absuite login` before any catalog operation.
- **Always provide a tenant context.**
- **Create categories, types, and brands first** before creating stock items.
- **Use `relate-*` commands** to link entities (categories, tags, policies) to stock items.
- **Use `--help`** on any command for full DTO schemas.
