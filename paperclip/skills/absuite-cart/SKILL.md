---
name: absuite-cart
description: >
  Manage shopping carts, cart lines, wish lists, and compare tables in the Alliance
  Business Suite (ABS) using the `absuite` CLI. Covers adding/removing items,
  updating quantities, submitting carts, managing wish lists, and product comparison.
  Requires an authenticated CLI session (use the `absuite-login` skill to authenticate first).
---

# Alliance Business Suite — Cart Skill

Manage shopping carts through the `absuite` CLI's `cart` service. Cart operations support guest, user, and tenant cart contexts.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite cart list-commands`

## Getting Carts

### Get Current User's Cart

```bash
absuite cart get user
```

### Get Acting Cart (Session Cart)

```bash
absuite cart get acting
```

### Get Guest Cart

```bash
absuite cart get guest
```

### Get Tenant's Default Cart

```bash
absuite cart get tenant --TenantId $TENANT_ID
```

### Get Cart by ID

```bash
absuite cart get by-id --CartId <cart-guid>
```

## Adding Items to Cart

### Add Item by ID

```bash
absuite cart add-item-to --CartId <cart-guid> --ItemId <item-guid> --Quantity 2
```

### Add Product with Tracking

```bash
absuite cart add-product-to --CartId <cart-guid> --ItemId <item-guid> --Quantity 1
```

## Cart Lines

### List Cart Lines

```bash
absuite cart list lines --CartId <cart-guid>
```

### List Cart Items

```bash
absuite cart list items --CartId <cart-guid>
```

### Get a Cart Line

```bash
absuite cart get line --CartId <cart-guid> --CartLineId <line-guid>
```

### Update a Cart Line

```bash
absuite cart update line --CartId <cart-guid> --CartLineId <line-guid> --CartLineUpdateDto '{...}'
```

### Increase Cart Line Quantity

```bash
absuite cart convert-to-crease-cart-line --CartId <cart-guid> --CartLineId <line-guid>
```

### Decrease Cart Line Quantity

```bash
absuite cart decrease-cart-line --CartId <cart-guid> --CartLineId <line-guid>
```

### Remove a Cart Line

```bash
absuite cart delete line --CartId <cart-guid> --CartLineId <line-guid>
```

## Cart Item Records

### Get Cart Record

```bash
absuite cart get item-cart-record --CartId <cart-guid> --CartRecordId <record-guid>
```

### Update Cart Record

```bash
absuite cart update item-cart-record --CartId <cart-guid> --CartRecordId <record-guid> --CartRecordUpdateDto '{...}'
```

### Increase Item Quantity

```bash
absuite cart convert-to-crease-item-cart-record --CartId <cart-guid> --CartRecordId <record-guid>
```

### Decrease Item Quantity

```bash
absuite cart decrease-item-cart-record --CartId <cart-guid> --CartRecordId <record-guid>
```

### Remove Item from Cart

```bash
absuite cart delete item-from --CartId <cart-guid> --ItemId <item-guid>
```

### Remove Product by Record ID

```bash
absuite cart delete product-from-cart-by-record-id --CartId <cart-guid> --CartRecordId <record-guid>
```

### Check if Item Is in Cart

```bash
absuite cart is-item-already-in --CartId <cart-guid> --ItemId <item-guid>
```

## Cart Settings

### Get/Set Cart Country

```bash
absuite cart get country --CartId <cart-guid>
absuite cart set country --CartId <cart-guid> --CountryId USA
```

### Get/Set Cart Currency

```bash
absuite cart get currency --CartId <cart-guid>
absuite cart set currency --CartId <cart-guid> --CurrencyId USD.USA
```

### Update Cart

```bash
absuite cart update --CartId <cart-guid> --CartUpdateDto '{...}'
```

### Clear Cart

```bash
absuite cart clear --CartId <cart-guid>
```

## Submit Cart (Checkout)

```bash
absuite cart submit --CartId <cart-guid>
```

This converts the cart into an order for processing.

## Wish Lists

### Create a Wish List

```bash
absuite cart create wish-list --CartId <cart-guid> --WishListCreateDto '{
  "Name": "Birthday Ideas"
}'
```

### Get Wish Lists

```bash
absuite cart get wish-list --CartId <cart-guid>
```

### Get Wish List Details

```bash
absuite cart list wish-list-details --CartId <cart-guid> --WishListId <wishlist-guid>
```

### Add Item to Wish List

```bash
absuite cart add-item-to-wish-list --CartId <cart-guid> --WishListId <wishlist-guid> --ItemId <item-guid>
```

### Add Product to Wish List

```bash
absuite cart add-product-to-wish-list --CartId <cart-guid> --WishListId <wishlist-guid> --ItemId <item-guid>
```

### Get Wish List Items

```bash
absuite cart list wish-list-items --CartId <cart-guid> --WishListId <wishlist-guid>
```

### Get Wish List Item

```bash
absuite cart get wish-list-item --CartId <cart-guid> --WishListId <wishlist-guid> --WishListItemId <item-guid>
```

### Update Wish List

```bash
absuite cart update item-to-wish-list --CartId <cart-guid> --WishListId <wishlist-guid> --WishListUpdateDto '{...}'
```

### Check if Item Is in Any Wish List

```bash
absuite cart is-item-in-wish-lists --CartId <cart-guid> --ItemId <item-guid>
absuite cart is-product-in-wish-lists --CartId <cart-guid> --ItemId <item-guid>
```

### Check Wish List Exists

```bash
absuite cart wish-list-exists --CartId <cart-guid> --WishListId <wishlist-guid>
```

### Delete Wish List

```bash
absuite cart delete wish-list --CartId <cart-guid> --WishListId <wishlist-guid>
```

### Delete Wish List Record

```bash
absuite cart delete wish-list-record --CartId <cart-guid> --WishListId <wishlist-guid> --WishListItemId <item-guid>
```

## Compare Table

### Add Item to Compare Table

```bash
absuite cart add-item-to-compare-table --CartId <cart-guid> --ItemId <item-guid>
```

### List Compare Records

```bash
absuite cart list compare-records --CartId <cart-guid>
```

### Get Compare Record

```bash
absuite cart get compare-record --CartId <cart-guid> --CompareRecordId <record-guid>
```

### Check if Item Is in Compare Table

```bash
absuite cart is-item-in-compare-table --CartId <cart-guid> --ItemId <item-guid>
```

### Remove Item from Compare Table

```bash
absuite cart delete item-from-compare-table --CartId <cart-guid> --ItemId <item-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Get user cart | `absuite cart get user` |
| Get tenant cart | `absuite cart get tenant --TenantId <guid>` |
| Add item | `absuite cart add-item-to --CartId <guid> --ItemId <guid> --Quantity 1` |
| List lines | `absuite cart list lines --CartId <guid>` |
| Remove line | `absuite cart delete line --CartId <guid> --CartLineId <guid>` |
| Clear cart | `absuite cart clear --CartId <guid>` |
| Submit cart | `absuite cart submit --CartId <guid>` |
| Set currency | `absuite cart set currency --CartId <guid> --CurrencyId USD.USA` |
| Create wish list | `absuite cart create wish-list --CartId <guid> --WishListCreateDto '{...}'` |
| Add to compare | `absuite cart add-item-to-compare-table --CartId <guid> --ItemId <guid>` |

## Critical Rules

- **Authenticate first.** Use `absuite login` before any cart operation.
- **Get the cart first.** Use `get user`, `get tenant`, or `get by-id` to obtain the cart ID.
- **Use `submit` to checkout.** This converts the cart into an order.
- **Check item existence** before adding to avoid duplicates with `is-item-already-in`.
