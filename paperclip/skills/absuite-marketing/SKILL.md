---
name: absuite-marketing
description: >
  Manage marketing campaigns, email groups, email signatures, email templates,
  marketing lists, newsletters, social media posts, and social post buckets in the
  Alliance Business Suite (ABS) using the `absuite` CLI. Includes tracking pixel
  retrieval. Requires an authenticated CLI session.
---

# Alliance Business Suite — Marketing Skill

Manage marketing through the `absuite` CLI's `marketing` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite marketing list-commands`

## Campaigns

```bash
# List (OData)
absuite marketing get campaign-o-data --TenantId $TENANT_ID

# Count
absuite marketing count campaigns --TenantId $TENANT_ID

# Get details by ID
absuite marketing list campaign-details --TenantId $TENANT_ID --MarketingCampaignId <campaign-guid>

# Create
absuite marketing create campaign --TenantId $TENANT_ID --MarketingCampaignCreateDto '{
  "Name": "Spring Sale 2026",
  "Offer": "20% off all products",
  "Active": true,
  "ProposedStart": "2026-03-01T00:00:00Z",
  "ProposedEnd": "2026-04-30T23:59:59Z",
  "AllocatedBudget": 10000.00,
  "CurrencyId": "<currency-guid>",
  "Code": "SPRING26"
}'

# Update
absuite marketing update campaign --TenantId $TENANT_ID --MarketingCampaignId <campaign-guid> --MarketingCampaignUpdateDto '{...}'

# Delete
absuite marketing delete campaign --TenantId $TENANT_ID --MarketingCampaignId <campaign-guid>
```

**Key MarketingCampaignCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `Name` | String | Campaign name |
| `Offer` | String | Campaign offer text |
| `Code` | String | Promo code |
| `Active` | Boolean | Whether campaign is active |
| `ProposedStart` / `ProposedEnd` | DateTime | Planned dates |
| `ActualStart` / `ActualEnd` | DateTime | Actual execution dates |
| `AllocatedBudget` | Double | Budget |
| `ActivityCost` / `MiscCost` | Double | Cost tracking |
| `ExpectedResponsePercent` | Double | Expected response rate |
| `CurrencyId` | String | Budget currency |

## Marketing Lists

```bash
absuite marketing get list-o-data --TenantId $TENANT_ID
absuite marketing count lists --TenantId $TENANT_ID
absuite marketing list list-details --TenantId $TENANT_ID --MarketingListId <list-guid>
absuite marketing create list --TenantId $TENANT_ID --MarketingListCreateDto '{
  "Name": "VIP Customers",
  "Description": "High-value customer segment"
}'
absuite marketing update list --TenantId $TENANT_ID --MarketingListId <list-guid> --MarketingListUpdateDto '{...}'
absuite marketing delete list --TenantId $TENANT_ID --MarketingListId <list-guid>
```

## Email Groups

```bash
absuite marketing get email-groups-o-data --TenantId $TENANT_ID
absuite marketing count email-groups --TenantId $TENANT_ID
absuite marketing list email-group-details --TenantId $TENANT_ID --EmailGroupId <group-guid>
absuite marketing create email-group --TenantId $TENANT_ID --EmailGroupCreateDto '{
  "Name": "Newsletter Subscribers"
}'
absuite marketing update email-group --TenantId $TENANT_ID --EmailGroupId <group-guid> --EmailGroupUpdateDto '{...}'
absuite marketing delete email-group --TenantId $TENANT_ID --EmailGroupId <group-guid>
```

## Email Signatures

```bash
absuite marketing get email-signatures-o-data --TenantId $TENANT_ID
absuite marketing count email-signatures --TenantId $TENANT_ID
absuite marketing list email-signature-details --TenantId $TENANT_ID --EmailSignatureId <sig-guid>
absuite marketing create email-signature --TenantId $TENANT_ID --EmailSignatureCreateDto '{
  "Name": "Corporate Signature",
  "Content": "<p>Best regards,<br/>Acme Corp</p>"
}'
absuite marketing update email-signature --TenantId $TENANT_ID --EmailSignatureId <sig-guid> --EmailSignatureUpdateDto '{...}'
absuite marketing delete email-signature --TenantId $TENANT_ID --EmailSignatureId <sig-guid>
```

## Email Templates

```bash
absuite marketing get email-templates-o-data --TenantId $TENANT_ID
absuite marketing count email-templates --TenantId $TENANT_ID
absuite marketing list email-template-details --TenantId $TENANT_ID --EmailTemplateId <template-guid>
absuite marketing create email-template --TenantId $TENANT_ID --EmailTemplateCreateDto '{
  "Name": "Welcome Email",
  "Subject": "Welcome to {{CompanyName}}",
  "Body": "<h1>Welcome!</h1><p>Thank you for joining.</p>"
}'
absuite marketing update email-template --TenantId $TENANT_ID --EmailTemplateId <template-guid> --EmailTemplateUpdateDto '{...}'
absuite marketing delete email-template --TenantId $TENANT_ID --EmailTemplateId <template-guid>
```

## Newsletters

```bash
absuite marketing get newsletter-o-data --TenantId $TENANT_ID
absuite marketing count newsletters --TenantId $TENANT_ID
absuite marketing list newsletter-details --TenantId $TENANT_ID --NewsletterId <newsletter-guid>
absuite marketing create newsletter --TenantId $TENANT_ID --NewsletterCreateDto '{
  "Name": "Monthly Digest - April 2026"
}'
absuite marketing update newsletter --TenantId $TENANT_ID --NewsletterId <newsletter-guid> --NewsletterUpdateDto '{...}'
absuite marketing delete newsletter --TenantId $TENANT_ID --NewsletterId <newsletter-guid>
```

## Social Media Posts

```bash
absuite marketing get social-media-posts-o-data --TenantId $TENANT_ID
absuite marketing count social-media-posts --TenantId $TENANT_ID
absuite marketing list social-media-post-details --TenantId $TENANT_ID --SocialMediaPostId <post-guid>
absuite marketing create social-media-post --TenantId $TENANT_ID --SocialMediaPostCreateDto '{
  "Title": "Spring Sale Announcement",
  "Content": "Get 20% off everything this spring! Use code SPRING26"
}'
absuite marketing update social-media-post --TenantId $TENANT_ID --SocialMediaPostId <post-guid> --SocialMediaPostUpdateDto '{...}'
absuite marketing delete social-media-post --TenantId $TENANT_ID --SocialMediaPostId <post-guid>
```

## Social Post Buckets

Organize social media posts into scheduled buckets.

```bash
absuite marketing get social-post-buckets-o-data --TenantId $TENANT_ID
absuite marketing count social-post-buckets --TenantId $TENANT_ID
absuite marketing list social-post-bucket-details --TenantId $TENANT_ID --SocialPostBucketId <bucket-guid>
absuite marketing create social-post-bucket --TenantId $TENANT_ID --SocialPostBucketCreateDto '{...}'
absuite marketing update social-post-bucket --TenantId $TENANT_ID --SocialPostBucketId <bucket-guid> --SocialPostBucketUpdateDto '{...}'
absuite marketing delete social-post-bucket --TenantId $TENANT_ID --SocialPostBucketId <bucket-guid>
```

## Tracking Pixel

```bash
absuite marketing get tracking-pixel --TenantId $TENANT_ID
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List campaigns | `absuite marketing get campaign-o-data --TenantId <guid>` |
| Create campaign | `absuite marketing create campaign --TenantId <guid> --MarketingCampaignCreateDto '{...}'` |
| List marketing lists | `absuite marketing get list-o-data --TenantId <guid>` |
| Create marketing list | `absuite marketing create list --TenantId <guid> --MarketingListCreateDto '{...}'` |
| List email templates | `absuite marketing get email-templates-o-data --TenantId <guid>` |
| Create email template | `absuite marketing create email-template --TenantId <guid> --EmailTemplateCreateDto '{...}'` |
| Create newsletter | `absuite marketing create newsletter --TenantId <guid> --NewsletterCreateDto '{...}'` |
| Get tracking pixel | `absuite marketing get tracking-pixel --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **List commands use OData** (e.g., `get campaign-o-data`), while detail views use `list *-details`.
- **Use `--help`** on any command for full DTO schemas.
