---
name: absuite-contacts
description: >
  Create, read, update, patch, and delete contacts in the Alliance Business Suite
  (ABS) CRM Service using the `absuite` CLI. Covers individual and organization
  contacts, avatars, wallets, carts, and social profiles. Requires an authenticated
  CLI session (use the `absuite-login` skill to authenticate first).
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

## CRUD Operations

### List Contacts

```bash
absuite crm list contacts --TenantId $TENANT_ID
```

### List Contacts (Extended — includes related data)

```bash
absuite crm list extended-contacts --TenantId $TENANT_ID
```

### Count Contacts

```bash
absuite crm count contacts --TenantId $TENANT_ID
```

### List Contacts by Type

**Individuals only:**

```bash
absuite crm list business-owned-individuals --TenantId $TENANT_ID
```

**Organizations only:**

```bash
absuite crm list business-owned-organizations --TenantId $TENANT_ID
```

### Get Single Contact

```bash
absuite crm get contact --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Extended Contact (includes related data)

```bash
absuite crm get extended-contact --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Organization Contact

```bash
absuite crm get business-owned-organization --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Create Contact

Check the schema first:

```bash
absuite crm create contact --help
```

Create:

```bash
absuite crm create contact --TenantId $TENANT_ID --ContactCreateDto '{"firstName":"Jane","lastName":"Doe","email":"jane.doe@example.com","tenantId":"'$TENANT_ID'"}'
```

The response envelope contains the new contact in the `result` field. Save the `id` for subsequent operations.

**Key ContactCreateDto fields:**

| Field | Type | Description |
|---|---|---|
| `tenantId` | string | Owner tenant (required) |
| `firstName` | string | First name |
| `lastName` | string | Last name |
| `email` | string | Email address |
| `type` | string | `0` = Individual, `1` = Organization |
| `countryId` | string | Country code (e.g., `USA`, `COL`) |
| `currencyId` | string | Currency code (e.g., `USD.USA`) |
| `qualifiedName` | string | Display name |

Run `absuite crm create contact --help` for the full schema.

### Update Contact (Full Replace)

```bash
absuite crm update contact --TenantId $TENANT_ID --ContactId $CONTACT_ID --ContactUpdateDto '{"tenantId":"'$TENANT_ID'","firstName":"Jane","lastName":"Smith","email":"jane.smith@example.com","id":"'$CONTACT_ID'"}'
```

### Patch Contact (Partial Update)

```bash
absuite crm patch contact --TenantId $TENANT_ID --ContactId $CONTACT_ID --Body '[{"op":"replace","path":"/FirstName","value":"Janet"},{"op":"replace","path":"/LastName","value":"Smith"}]'
```

**JSON Patch format:** Array of operations following RFC 6902. Supported ops: `replace`, `add`, `remove`.

### Delete Contact

```bash
absuite crm delete contact --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

## Related Resources

### Get Contact Wallet

```bash
absuite crm get contact-wallet --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Contact Cart

```bash
absuite crm get contact-cart --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Contact Social Profile

```bash
absuite crm get contact-social-profile --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### List Contact Social Profiles

```bash
absuite crm list contact-profiles --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Contact Avatar

```bash
absuite crm get contact-avatar --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Update Contact Avatar

```bash
absuite crm update contact-avatar --TenantId $TENANT_ID --ContactId $CONTACT_ID --Avatar @/path/to/avatar.png
```

## Contact Options (Key-Value Metadata)

### List Contact Options

```bash
absuite crm list contact-options --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

### Get Option by Key

```bash
absuite crm get contact-option-by-key --TenantId $TENANT_ID --ContactId $CONTACT_ID --ContactOptionKey my-key
```

### Create / Upsert Option

```bash
absuite crm upsert contact-option --TenantId $TENANT_ID --ContactId $CONTACT_ID --ContactOptionKey my-key --ContactOptionUpdateDto '{"value":"my-value"}'
```

## Contact Emails

### Send Email to a Contact

```bash
absuite crm send contact-email --TenantId $TENANT_ID --ContactId $CONTACT_ID --EmailDispatchRequest '{"title":"Hello","message":"Your report is ready.","culture":"en","uiCulture":"en"}'
```

### Preview Contact Email

```bash
absuite crm preview contact-email-template --TenantId $TENANT_ID --ContactId $CONTACT_ID
```

## Sync Operations

### Sync Current User into a Tenant's Contact List

```bash
absuite crm sync-current-holder-to-tenant --TenantId $TENANT_ID
```

### Sync a User into a Tenant's Contact List

```bash
absuite crm sync-holder-to-tenant --TenantId $TENANT_ID --UserId $USER_ID
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List contacts | `absuite crm list contacts --TenantId <guid>` |
| Count contacts | `absuite crm count contacts --TenantId <guid>` |
| List individuals | `absuite crm list business-owned-individuals --TenantId <guid>` |
| List organizations | `absuite crm list business-owned-organizations --TenantId <guid>` |
| Get contact | `absuite crm get contact --TenantId <guid> --ContactId <guid>` |
| Create contact | `absuite crm create contact --TenantId <guid> --ContactCreateDto '{...}'` |
| Update contact | `absuite crm update contact --TenantId <guid> --ContactId <guid> --ContactUpdateDto '{...}'` |
| Patch contact | `absuite crm patch contact --TenantId <guid> --ContactId <guid> --Body '[...]'` |
| Delete contact | `absuite crm delete contact --TenantId <guid> --ContactId <guid>` |
| Get wallet | `absuite crm get contact-wallet --TenantId <guid> --ContactId <guid>` |
| Get cart | `absuite crm get contact-cart --TenantId <guid> --ContactId <guid>` |
| Get avatar | `absuite crm get contact-avatar --TenantId <guid> --ContactId <guid>` |
| Send email | `absuite crm send contact-email --TenantId <guid> --ContactId <guid> --EmailDispatchRequest '{...}'` |

## Critical Rules

- **Authenticate first.** Use `absuite login` before any CRM operation.
- **Always provide a tenant context.** Set a default with `absuite config set --tenant-id` or pass `--TenantId` on each call.
- **Use `--help` before unfamiliar commands.** It shows exact parameter names, types, DTO schemas, and return types.
- **Use `list-commands` for discovery.** Don't guess — run `absuite crm list-commands`.
- **Save the contact `id` after creation.** The response `result` includes the new contact's ID.
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
