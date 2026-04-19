---
name: absuite-subscriptions
description: >
  Manage subscriptions and subscription plans in the Alliance Business Suite (ABS)
  using the `absuite` CLI. Covers creating and managing recurring subscription plans,
  customer subscriptions, and subscription lifecycle. Requires an authenticated CLI session.
---

# Alliance Business Suite — Subscriptions Skill

Manage subscriptions through the `absuite` CLI's `subscriptions` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite subscriptions list-commands`

## Subscription Plans

Define the plans that customers can subscribe to.

```bash
# List
absuite subscriptions list plans --TenantId $TENANT_ID

# Count
absuite subscriptions count plans --TenantId $TENANT_ID

# Get by ID
absuite subscriptions get plan --TenantId $TENANT_ID --SubscriptionPlanId <plan-guid>

# Create
absuite subscriptions create plan --TenantId $TENANT_ID --SubscriptionPlanCreateDto '{
  "Name": "Professional Monthly",
  "Description": "Professional tier with all features, billed monthly",
  "Price": 49.99,
  "BillingInterval": "Monthly"
}'

# Update
absuite subscriptions update plan --TenantId $TENANT_ID --SubscriptionPlanId <plan-guid> --SubscriptionPlanUpdateDto '{...}'

# Delete
absuite subscriptions delete plan --TenantId $TENANT_ID --SubscriptionPlanId <plan-guid>
```

## Subscriptions

Manage individual customer subscriptions.

```bash
# List
absuite subscriptions list --TenantId $TENANT_ID

# Count
absuite subscriptions count --TenantId $TENANT_ID

# Get by ID
absuite subscriptions get --TenantId $TENANT_ID --SubscriptionId <sub-guid>

# Create
absuite subscriptions create --TenantId $TENANT_ID --SubscriptionCreateDto '{
  "IndividualId": "<contact-guid>",
  "SubscriptionPlanId": "<plan-guid>",
  "SubscriptionClass": "Individual"
}'

# Update
absuite subscriptions update --TenantId $TENANT_ID --SubscriptionId <sub-guid> --SubscriptionUpdateDto '{...}'

# Delete
absuite subscriptions delete --TenantId $TENANT_ID --SubscriptionId <sub-guid>
```

**Key SubscriptionCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `IndividualId` | String | Customer contact (individual) |
| `OrganizationId` | String | Customer contact (organization) |
| `SubscriptionPlanId` | String | Plan to subscribe to |
| `SubscriptionClass` | String | `Individual` or `Organization` |

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List plans | `absuite subscriptions list plans --TenantId <guid>` |
| Create plan | `absuite subscriptions create plan --TenantId <guid> --SubscriptionPlanCreateDto '{...}'` |
| List subscriptions | `absuite subscriptions list --TenantId <guid>` |
| Create subscription | `absuite subscriptions create --TenantId <guid> --SubscriptionCreateDto '{...}'` |
| Get subscription | `absuite subscriptions get --TenantId <guid> --SubscriptionId <guid>` |
| Count subscriptions | `absuite subscriptions count --TenantId <guid>` |

## Full Example: Set Up & Subscribe

```bash
# 1. Create a plan
absuite subscriptions create plan --SubscriptionPlanCreateDto '{
  "Name": "Starter Monthly",
  "Price": 19.99,
  "BillingInterval": "Monthly"
}'

# 2. Subscribe a customer
absuite subscriptions create --SubscriptionCreateDto '{
  "IndividualId": "<contact-guid>",
  "SubscriptionPlanId": "<plan-guid>",
  "SubscriptionClass": "Individual"
}'

# 3. Check subscriptions
absuite subscriptions list
```

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Create plans first** before subscribing customers.
- **SubscriptionClass** must match the contact type — `Individual` for persons, `Organization` for companies.
