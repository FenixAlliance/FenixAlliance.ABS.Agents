---
name: absuite-contacts
description: >
  Create, read, update, patch, and delete contacts in the Alliance Business Suite
  (ABS) CRM Service using the `absuite` CLI. Covers individual and organization
  contacts, extended views, relationship graphs, avatars, wallets, carts, social
  profiles, contact options (key-value metadata), contact emails, and user/tenant
  sync operations. Requires an authenticated CLI session (use the `absuite-login`
  skill to authenticate first).
---

# Alliance Business Suite — Contacts Skill

Manage contacts through the `absuite` CLI's `crm` service. All contact operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant** — all contact operations require a tenant. Either set a default:
   ```bash
   absuite config set --tenant-id <tenant-guid>
   ```
   Or pass `--TenantId <guid>` on each call.
3. **Discover commands** — run `absuite crm list-commands` to see all CRM commands, or use `--help` on any command for full parameter and output schemas.

## Command Discovery

```bash
# List all CRM commands
absuite crm list-commands

# Filter for contact-related commands
absuite crm list-commands | grep contact

# Get detailed help for any command
absuite crm create contact --help
```

## Key Concepts

- **Contact** — a person or organization in the CRM. Type `0` = Individual, Type `1` = Organization.
- **Extended Contact** — a contact record enriched with related data (profiles, wallets, etc.).
- **Contact Option** — a key-value metadata pair attached to a contact (e.g., preferences, tags).
- **Social Profile** — a contact's linked social/web profiles.
- **Relationship Graph** — contacts can be linked: individual↔individual, individual↔organization, organization↔organization.

## CRUD Operations

### List Contacts

```bash
absuite crm list contacts --TenantId $TENANT_ID
```

### List Extended Contacts (with related data)

```bash
absuite crm list extended-contacts --TenantId $TENANT_ID
```

### Count Contacts

```bash
absuite crm count contacts --TenantId $TENANT_ID
```

### List by Type

**Individuals:**

```bash
absuite crm list business-owned-individuals --TenantId $TENANT_ID
```

**Extended individuals:**

```bash
absuite crm list extended-business-owned-individuals --TenantId $TENANT_ID
```

**Organizations:**

```bash
absuite crm list business-owned-organizations --TenantId $TENANT_ID
```

**Extended organizations:**

```bash
absuite crm list extended-business-owned-organizations --TenantId $TENANT_ID
```

### Count by Type

```bash
absuite crm count business-owned-individuals --TenantId $TENANT_ID
absuite crm count business-owned-organizations --TenantId $TENANT_ID
```

### Get Contact by ID

```bash
absuite crm get contact --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Get Extended Contact

```bash
absuite crm get extended-contact --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Get Individual by ID

```bash
absuite crm get business-owned-individual --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Get Organization by ID

```bash
absuite crm get business-owned-organization --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Create Contact

```bash
absuite crm create contact --TenantId $TENANT_ID --ContactCreateDto '{
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane.doe@example.com",
  "tenantId": "<tenant-guid>",
  "type": "0",
  "countryId": "USA",
  "currencyId": "USD.USA"
}'
```

**Key ContactCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `tenantId` | String | Owner tenant (required) |
| `firstName` | String | First name |
| `lastName` | String | Last name |
| `email` | String | Email address |
| `type` | String | `"0"` = Individual, `"1"` = Organization |
| `countryId` | String | Country code (e.g., `"USA"`, `"COL"`) |
| `currencyId` | String | Currency code (e.g., `"USD.USA"`) |
| `qualifiedName` | String | Display name |
| `companyName` | String | Company name (for organizations) |
| `jobTitle` | String | Job title |
| `phone` | String | Phone number |

Run `absuite crm create contact --help` for the full schema.

### Update Contact (Full Replace)

```bash
absuite crm update contact --TenantId $TENANT_ID --ContactId <contact-guid> --ContactUpdateDto '{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com"
}'
```

### Patch Contact (Partial Update — RFC 6902)

```bash
absuite crm patch contact --TenantId $TENANT_ID --ContactId <contact-guid> --Body '[
  {"op": "replace", "path": "/FirstName", "value": "Janet"},
  {"op": "replace", "path": "/LastName", "value": "Smith"}
]'
```

**Supported operations:** `replace`, `add`, `remove`.

### Delete Contact

```bash
absuite crm delete contact --TenantId $TENANT_ID --ContactId <contact-guid>
```

## Contact Relationships

Contacts can be linked to each other, forming a relationship graph.

### Individual → Individuals

```bash
absuite crm list individual-related-individuals --TenantId $TENANT_ID --ContactId <individual-guid>
```

### Individual → Organizations

```bash
absuite crm list individual-related-organizations --TenantId $TENANT_ID --ContactId <individual-guid>
```

### Organization → Individuals

```bash
absuite crm list organization-related-individuals --TenantId $TENANT_ID --ContactId <organization-guid>
```

### Organization → Organizations

```bash
absuite crm list organization-related-organizations --TenantId $TENANT_ID --ContactId <organization-guid>
```

## Related Resources

### Wallet

```bash
absuite crm get contact-wallet --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Cart

```bash
absuite crm get contact-cart --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Social Profile

```bash
# Get primary social profile
absuite crm get contact-social-profile --TenantId $TENANT_ID --ContactId <contact-guid>

# List all social profiles
absuite crm list contact-profiles --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Avatar

```bash
# Get avatar
absuite crm get contact-avatar --TenantId $TENANT_ID --ContactId <contact-guid>

# Update avatar
absuite crm update contact-avatar --TenantId $TENANT_ID --ContactId <contact-guid> --Avatar @/path/to/avatar.png
```

## Contact Options (Key-Value Metadata)

Store arbitrary metadata on contacts using key-value pairs.

### List All Options

```bash
absuite crm list contact-options --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Count Options

```bash
absuite crm count contact-options --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Get Option by Key

```bash
absuite crm get contact-option-by-key --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionKey preferred-language
```

### Get Option by ID

```bash
absuite crm get contact-option-by-id --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid>
```

### Create Option

```bash
absuite crm create contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionCreateDto '{
  "key": "preferred-language",
  "value": "en-US"
}'
```

### Upsert Option (Create or Update by Key)

```bash
absuite crm upsert contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionKey preferred-language --ContactOptionUpdateDto '{
  "value": "es-CO"
}'
```

### Update Option

```bash
absuite crm update contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid> --ContactOptionUpdateDto '{
  "value": "fr-FR"
}'
```

### Delete Option

```bash
absuite crm delete contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid>
```

## Contact Emails

### Send Email to a Contact

```bash
absuite crm send contact-email --TenantId $TENANT_ID --ContactId <contact-guid> --EmailDispatchRequest '{
  "title": "Your Report Is Ready",
  "message": "Please find your monthly report attached.",
  "culture": "en-US",
  "uiCulture": "en-US"
}'
```

### Preview Contact Email Template

```bash
absuite crm preview contact-email-template --TenantId $TENANT_ID --ContactId <contact-guid>
```

## Sync Operations

Synchronize users and tenants into a tenant's contact list.

### Sync Current User → Current Tenant

```bash
absuite crm sync-current-holder-to-current-tenant
```

### Sync Current User → Specific Tenant

```bash
absuite crm sync-current-holder-to-tenant --TenantId $TENANT_ID
```

### Sync a User → a Tenant

```bash
absuite crm sync-holder-to-tenant --TenantId $TENANT_ID --UserId <user-guid>
```

### Sync a Tenant → Another Tenant

```bash
absuite crm sync-tenant-to-tenant --TenantId $TARGET_TENANT_ID --SourceTenantId <source-tenant-guid>
```

### Upsert User onto a Tenant's Contact List

```bash
absuite crm upsert user-onto-another-tenant-contact-list --TenantId $TENANT_ID --UserId <user-guid>
```

### Upsert Tenant onto Another Tenant's Contact List

```bash
absuite crm upsert tenant-onto-another-tenant-contact-list --TenantId $TARGET_TENANT_ID --SourceTenantId <source-tenant-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List contacts | `absuite crm list contacts --TenantId <guid>` |
| List extended | `absuite crm list extended-contacts --TenantId <guid>` |
| Count contacts | `absuite crm count contacts --TenantId <guid>` |
| List individuals | `absuite crm list business-owned-individuals --TenantId <guid>` |
| List organizations | `absuite crm list business-owned-organizations --TenantId <guid>` |
| Get contact | `absuite crm get contact --TenantId <guid> --ContactId <guid>` |
| Get extended | `absuite crm get extended-contact --TenantId <guid> --ContactId <guid>` |
| Create contact | `absuite crm create contact --TenantId <guid> --ContactCreateDto '{...}'` |
| Update contact | `absuite crm update contact --TenantId <guid> --ContactId <guid> --ContactUpdateDto '{...}'` |
| Patch contact | `absuite crm patch contact --TenantId <guid> --ContactId <guid> --Body '[...]'` |
| Delete contact | `absuite crm delete contact --TenantId <guid> --ContactId <guid>` |
| Related individuals | `absuite crm list individual-related-individuals --TenantId <guid> --ContactId <guid>` |
| Related orgs | `absuite crm list individual-related-organizations --TenantId <guid> --ContactId <guid>` |
| Get wallet | `absuite crm get contact-wallet --TenantId <guid> --ContactId <guid>` |
| Get cart | `absuite crm get contact-cart --TenantId <guid> --ContactId <guid>` |
| Get avatar | `absuite crm get contact-avatar --TenantId <guid> --ContactId <guid>` |
| List options | `absuite crm list contact-options --TenantId <guid> --ContactId <guid>` |
| Upsert option | `absuite crm upsert contact-option --TenantId <guid> --ContactId <guid> --ContactOptionKey <key> --ContactOptionUpdateDto '{...}'` |
| Send email | `absuite crm send contact-email --TenantId <guid> --ContactId <guid> --EmailDispatchRequest '{...}'` |
| Sync user | `absuite crm sync-holder-to-tenant --TenantId <guid> --UserId <guid>` |

## Full Example: End-to-End Contact Management

```bash
# 1. Authenticate
absuite login --email admin@company.com

# 2. Set tenant
absuite config set --tenant-id 00000000-0000-0000-0000-000000000000

# 3. Create an individual contact
absuite crm create contact --ContactCreateDto '{
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane.doe@acme.com",
  "type": "0",
  "jobTitle": "VP of Engineering",
  "companyName": "Acme Corp",
  "countryId": "USA",
  "currencyId": "USD.USA"
}'
# Note the returned contact ID

# 4. Create an organization contact
absuite crm create contact --ContactCreateDto '{
  "qualifiedName": "Acme Corporation",
  "email": "info@acme.com",
  "type": "1",
  "companyName": "Acme Corporation",
  "countryId": "USA"
}'

# 5. Set metadata on the individual
absuite crm upsert contact-option --ContactId <jane-id> --ContactOptionKey preferred-language --ContactOptionUpdateDto '{"value": "en-US"}'
absuite crm upsert contact-option --ContactId <jane-id> --ContactOptionKey tier --ContactOptionUpdateDto '{"value": "enterprise"}'

# 6. Send a welcome email
absuite crm send contact-email --ContactId <jane-id> --EmailDispatchRequest '{
  "title": "Welcome to Our Platform",
  "message": "Hi Jane, welcome aboard!",
  "culture": "en-US"
}'

# 7. View relationships
absuite crm list individual-related-organizations --ContactId <jane-id>

# 8. Get extended view
absuite crm get extended-contact --ContactId <jane-id>

# 9. Patch update
absuite crm patch contact --ContactId <jane-id> --Body '[
  {"op": "replace", "path": "/JobTitle", "value": "CTO"}
]'

# 10. Verify
absuite crm get contact --ContactId <jane-id>
```

## Critical Rules

- **Authenticate first.** Use `absuite login` before any CRM operation.
- **Always provide a tenant context.** Set a default with `absuite config set --tenant-id` or pass `--TenantId` on each call.
- **Use `--help` before unfamiliar commands.** It shows exact parameter names, types, DTO schemas, and return types.
- **Use `list-commands` for discovery.** Don't guess — run `absuite crm list-commands`.
- **Save the contact `id` after creation.** The response `result` includes the new contact's ID.
- **Use `upsert contact-option`** to set metadata — it creates or updates in one call
```bash
absuite crm list contact-options --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Count Options

```bash
absuite crm count contact-options --TenantId $TENANT_ID --ContactId <contact-guid>
```

### Get Option by Key

```bash
absuite crm get contact-option-by-key --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionKey preferred-language
```

### Get Option by ID

```bash
absuite crm get contact-option-by-id --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid>
```

### Create Option

```bash
absuite crm create contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionCreateDto '{
  "key": "preferred-language",
  "value": "en-US"
}'
```

### Upsert Option (Create or Update by Key)

```bash
absuite crm upsert contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionKey preferred-language --ContactOptionUpdateDto '{
  "value": "es-CO"
}'
```

### Update Option

```bash
absuite crm update contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid> --ContactOptionUpdateDto '{
  "value": "fr-FR"
}'
```

### Delete Option

```bash
absuite crm delete contact-option --TenantId $TENANT_ID --ContactId <contact-guid> --ContactOptionId <option-guid>
```

## Contact Emails

### Send Email to a Contact

```bash
absuite crm send contact-email --TenantId $TENANT_ID --ContactId <contact-guid> --EmailDispatchRequest '{
  "title": "Your Report Is Ready",
  "message": "Please find your monthly report attached.",
  "culture": "en-US",
  "uiCulture": "en-US"
}'
```

### Preview Contact Email Template

```bash
absuite crm preview contact-email-template --TenantId $TENANT_ID --ContactId <contact-guid>
```

## Sync Operations

Synchronize users and tenants into a tenant's contact list.

### Sync Current User → Current Tenant

```bash
absuite crm sync-current-holder-to-current-tenant
```

### Sync Current User → Specific Tenant

```bash
absuite crm sync-current-holder-to-tenant --TenantId $TENANT_ID
```

### Sync a User → a Tenant

```bash
absuite crm sync-holder-to-tenant --TenantId $TENANT_ID --UserId <user-guid>
```

### Sync a Tenant → Another Tenant

```bash
absuite crm sync-tenant-to-tenant --TenantId $TARGET_TENANT_ID --SourceTenantId <source-tenant-guid>
```

### Upsert User onto a Tenant's Contact List

```bash
absuite crm upsert user-onto-another-tenant-contact-list --TenantId $TENANT_ID --UserId <user-guid>
```

### Upsert Tenant onto Another Tenant's Contact List

```bash
absuite crm upsert tenant-onto-another-tenant-contact-list --TenantId $TARGET_TENANT_ID --SourceTenantId <source-tenant-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List contacts | `absuite crm list contacts --TenantId <guid>` |
| List extended | `absuite crm list extended-contacts --TenantId <guid>` |
| Count contacts | `absuite crm count contacts --TenantId <guid>` |
| List individuals | `absuite crm list business-owned-individuals --TenantId <guid>` |
| List organizations | `absuite crm list business-owned-organizations --TenantId <guid>` |
| Get contact | `absuite crm get contact --TenantId <guid> --ContactId <guid>` |
| Get extended | `absuite crm get extended-contact --TenantId <guid> --ContactId <guid>` |
| Create contact | `absuite crm create contact --TenantId <guid> --ContactCreateDto '{...}'` |
| Update contact | `absuite crm update contact --TenantId <guid> --ContactId <guid> --ContactUpdateDto '{...}'` |
| Patch contact | `absuite crm patch contact --TenantId <guid> --ContactId <guid> --Body '[...]'` |
| Delete contact | `absuite crm delete contact --TenantId <guid> --ContactId <guid>` |
| Related individuals | `absuite crm list individual-related-individuals --TenantId <guid> --ContactId <guid>` |
| Related orgs | `absuite crm list individual-related-organizations --TenantId <guid> --ContactId <guid>` |
| Get wallet | `absuite crm get contact-wallet --TenantId <guid> --ContactId <guid>` |
| Get cart | `absuite crm get contact-cart --TenantId <guid> --ContactId <guid>` |
| Get avatar | `absuite crm get contact-avatar --TenantId <guid> --ContactId <guid>` |
| List options | `absuite crm list contact-options --TenantId <guid> --ContactId <guid>` |
| Upsert option | `absuite crm upsert contact-option --TenantId <guid> --ContactId <guid> --ContactOptionKey <key> --ContactOptionUpdateDto '{...}'` |
| Send email | `absuite crm send contact-email --TenantId <guid> --ContactId <guid> --EmailDispatchRequest '{...}'` |
| Sync user | `absuite crm sync-holder-to-tenant --TenantId <guid> --UserId <guid>` |

## Full Example: End-to-End Contact Management

```bash
# 1. Authenticate
absuite login --email admin@company.com

# 2. Set tenant
absuite config set --tenant-id 00000000-0000-0000-0000-000000000000

# 3. Create an individual contact
absuite crm create contact --ContactCreateDto '{
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane.doe@acme.com",
  "type": "0",
  "jobTitle": "VP of Engineering",
  "companyName": "Acme Corp",
  "countryId": "USA",
  "currencyId": "USD.USA"
}'
# Note the returned contact ID

# 4. Create an organization contact
absuite crm create contact --ContactCreateDto '{
  "qualifiedName": "Acme Corporation",
  "email": "info@acme.com",
  "type": "1",
  "companyName": "Acme Corporation",
  "countryId": "USA"
}'

# 5. Set metadata on the individual
absuite crm upsert contact-option --ContactId <jane-id> --ContactOptionKey preferred-language --ContactOptionUpdateDto '{"value": "en-US"}'
absuite crm upsert contact-option --ContactId <jane-id> --ContactOptionKey tier --ContactOptionUpdateDto '{"value": "enterprise"}'

# 6. Send a welcome email
absuite crm send contact-email --ContactId <jane-id> --EmailDispatchRequest '{
  "title": "Welcome to Our Platform",
  "message": "Hi Jane, welcome aboard!",
  "culture": "en-US"
}'

# 7. View relationships
absuite crm list individual-related-organizations --ContactId <jane-id>

# 8. Get extended view
absuite crm get extended-contact --ContactId <jane-id>

# 9. Patch update
absuite crm patch contact --ContactId <jane-id> --Body '[
  {"op": "replace", "path": "/JobTitle", "value": "CTO"}
]'

# 10. Verify
absuite crm get contact --ContactId <jane-id>
```

## Critical Rules

- **Authenticate first.** Use `absuite login` before any CRM operation.
- **Always provide a tenant context.** Set a default with `absuite config set --tenant-id` or pass `--TenantId` on each call.
- **Use `--help` before unfamiliar commands.** It shows exact parameter names, types, DTO schemas, and return types.
- **Use `list-commands` for discovery.** Don't guess — run `absuite crm list-commands`.
- **Save the contact `id` after creation.** The response `result` includes the new contact's ID.
- **Use `upsert contact-option`** to set metadata — it creates or updates in one call.
- **Contact types:** `0` = Individual, `1` = Organization. Use `list business-owned-individuals` or `list business-owned-organizations` to filter.
- **JSON Patch for partial updates.** Use `patch contact` with RFC 6902 operations. Use `update contact` for full replacements.
- **Never hard-code IDs or credentials.** Use environment variables and values from API responses.

## Example: Create Contact and Verify

```bash
# 1. Create a contact
absuite crm create contact --TenantId $TENANT_ID --ContactCreateDto '{"firstName":"Alice","lastName":"Johnson","email":"alice.johnson@example.com","tenantId":"'$TENANT_ID'"}'

CONTACT_ID=$(echo "$CREATE_RESPONSE" | jq -r '.result.contactId')

if [ "$CONTACT_ID" = "null" ] || [ -z "$CONTACT_ID" ]; then
  echo "Failed to create contact."
  echo "$CREATE_RESPONSE" | jq .
  exit 1
fi

echo "Contact created: $CONTACT_ID"

# Verify by fetching it
GET_RESPONSE=$(curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID")

echo "Contact details:"
echo "$GET_RESPONSE" | jq '.result'
```


# 2. List contacts to confirm
absuite crm list contacts --TenantId $TENANT_ID

# 3. Get the specific contact (extract id from step 1 response)
absuite crm get contact --TenantId $TENANT_ID --ContactId $CONTACT_ID

# 4. Update the contact
absuite crm update contact --TenantId $TENANT_ID --ContactId $CONTACT_ID --ContactUpdateDto '{"firstName":"Alice","lastName":"Smith","email":"alice.smith@example.com","tenantId":"'$TENANT_ID'","id":"'$CONTACT_ID'"}'

# 5. Delete the contact
absuite crm delete contact --TenantId $TENANT_ID --ContactId $CONTACT_ID
```
