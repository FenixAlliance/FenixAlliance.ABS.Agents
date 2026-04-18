---
name: absuite-contacts
description: >
  Create, read, update, patch, and delete contacts in the Alliance Business Suite
  (ABS) CRM Service. Covers individual and organization contacts, OData queries,
  avatars, wallets, carts, and social profiles. Requires a valid ABS bearer token
  (use the `absuite` skill to authenticate first).
---

# Alliance Business Suite — Contacts Skill

Manage contacts through the ABS CRM Service REST API. All contact endpoints are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using the `absuite` skill to obtain `ABSUITE_ACCESS_TOKEN`.
2. **Know your tenant** — all contact operations require a `TenantId`. Obtain it from the WhoAmI response or `/api/v2/me/tenants`.

Env vars expected: `ABSUITE_HOST_URL`, `ABSUITE_ACCESS_TOKEN` (from login), and a known `TENANT_ID`.

## Base URL

All contact endpoints live under:

```
$ABSUITE_HOST_URL/api/v2/CrmService/Contacts
```

## Required Headers

Every request must include:

| Header | Value | Purpose |
|---|---|---|
| `Authorization` | `Bearer $ABSUITE_ACCESS_TOKEN` | Authentication |
| `X-TenantId` | `$TENANT_ID` | Tenant scoping — contacts are tenant-owned |
| `Content-Type` | `application/json` | Required for POST/PUT/PATCH bodies |
| `Accept` | `application/json` | Ensures JSON responses |

## CRUD Operations

### List Contacts

Retrieve all contacts for the current tenant.

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Accept: application/json"
```

### List Contacts with OData

Use OData query parameters for filtering, selecting fields, pagination, and counting.

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/OData?\$top=100&\$select=Email,FirstName,LastName,TenantId&\$filter=tenantId%20eq%20'$TENANT_ID'" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Accept: application/json"
```

**Common OData parameters:**

| Parameter | Example | Description |
|---|---|---|
| `$top` | `100` | Limit number of results |
| `$select` | `Email,FirstName,LastName` | Choose which fields to return |
| `$filter` | `email eq 'john@example.com'` | Filter by field value |
| `$count` | `true` | Include total count in response |
| `$expand` | `true` | Expand navigation properties |

### List Contacts by Type

**Individuals only:**

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/Individuals" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

**Organizations only:**

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/Organizations" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Get Single Contact

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Get Organization Contact

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/Organizations/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Create Contact

```bash
curl -s -X POST "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d "{
    \"FirstName\": \"Jane\",
    \"LastName\": \"Doe\",
    \"Email\": \"jane.doe@example.com\",
    \"TenantId\": \"$TENANT_ID\"
  }"
```

**Expected response** (HTTP 201):

```json
{
  "isSuccess": true,
  "result": {
    "contactId": "<guid>",
    "firstName": "Jane",
    "lastName": "Doe",
    "email": "jane.doe@example.com"
  }
}
```

Save `contactId` from the response for subsequent operations on this contact.

**Required fields:** `FirstName`, `LastName`, `Email`, `TenantId`.

### Update Contact (Full Replace)

Replace the entire contact record. Include all fields — any omitted field may be cleared.

```bash
curl -s -X PUT "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Content-Type: application/json" \
  -d "{
    \"tenantId\": \"$TENANT_ID\",
    \"type\": 0,
    \"firstName\": \"Jane\",
    \"lastName\": \"Smith\",
    \"qualifiedName\": \"Jane Smith\",
    \"email\": \"jane.smith@example.com\",
    \"countryId\": \"USA\",
    \"currencyId\": \"USD.USA\",
    \"id\": \"$CONTACT_ID\"
  }"
```

**Common fields for full update:**

| Field | Type | Description |
|---|---|---|
| `tenantId` | string (guid) | Owner tenant |
| `type` | int | `0` = Individual, `1` = Organization |
| `firstName` | string | First name |
| `lastName` | string | Last name |
| `qualifiedName` | string | Display name |
| `email` | string | Email address |
| `countryId` | string | Country code (e.g., `USA`, `COL`) |
| `currencyId` | string | Currency code (e.g., `USD.USA`) |
| `id` | string (guid) | Contact ID (must match URL) |

### Patch Contact (Partial Update)

Update specific fields using JSON Patch operations. Only the specified fields are modified.

```bash
curl -s -X PATCH "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Content-Type: application/json" \
  -d "[
    { \"op\": \"replace\", \"path\": \"/FirstName\", \"value\": \"Janet\" },
    { \"op\": \"replace\", \"path\": \"/LastName\", \"value\": \"Smith\" }
  ]"
```

**JSON Patch format:** Array of operations following [RFC 6902](https://datatracker.ietf.org/doc/html/rfc6902). Supported ops: `replace`, `add`, `remove`.

### Delete Contact

```bash
curl -s -X DELETE "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

## Related Resources

Each contact has associated resources accessible via sub-endpoints.

### Get Contact Wallet

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID/Wallet" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Get Contact Cart

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID/Cart" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Get Contact Social Profile

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID/SocialProfile" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Get Contact Avatar

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID/Avatar" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID"
```

### Update Contact Avatar

Upload a new avatar image using multipart form data.

```bash
curl -s -X POST "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$CONTACT_ID/Avatar" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -F "Avatar=@/path/to/avatar.png"
```

## Endpoint Quick Reference

| Action | Method | Endpoint |
|---|---|---|
| List contacts | `GET` | `/api/v2/CrmService/Contacts` |
| List contacts (OData) | `GET` | `/api/v2/CrmService/Contacts/OData` |
| List individuals | `GET` | `/api/v2/CrmService/Contacts/Individuals` |
| List organizations | `GET` | `/api/v2/CrmService/Contacts/Organizations` |
| Get contact | `GET` | `/api/v2/CrmService/Contacts/{contactId}` |
| Get organization | `GET` | `/api/v2/CrmService/Contacts/Organizations/{contactId}` |
| Create contact | `POST` | `/api/v2/CrmService/Contacts` |
| Update contact (full) | `PUT` | `/api/v2/CrmService/Contacts/{contactId}` |
| Patch contact (partial) | `PATCH` | `/api/v2/CrmService/Contacts/{contactId}` |
| Delete contact | `DELETE` | `/api/v2/CrmService/Contacts/{contactId}` |
| Get wallet | `GET` | `/api/v2/CrmService/Contacts/{contactId}/Wallet` |
| Get cart | `GET` | `/api/v2/CrmService/Contacts/{contactId}/Cart` |
| Get social profile | `GET` | `/api/v2/CrmService/Contacts/{contactId}/SocialProfile` |
| Get avatar | `GET` | `/api/v2/CrmService/Contacts/{contactId}/Avatar` |
| Upload avatar | `POST` | `/api/v2/CrmService/Contacts/{contactId}/Avatar` |

## Critical Rules

- **Authenticate first.** Use the `absuite` skill to obtain a bearer token before calling any contact endpoint.
- **Always include `X-TenantId`.** All contact operations are tenant-scoped. Omitting this header will result in errors.
- **Use PATCH for partial updates, PUT for full replacements.** PATCH uses JSON Patch format (RFC 6902 array of operations). PUT replaces the entire contact record.
- **Save the `contactId` after creation.** The response from `POST` includes the new contact's ID — store it for subsequent GET/PUT/PATCH/DELETE calls.
- **Never hard-code IDs or credentials.** Use environment variables and values from API responses.
- **Contact types:** `0` = Individual, `1` = Organization. Filter by type using the `/Individuals` and `/Organizations` sub-routes.
- **OData for bulk queries.** Use the `/OData` endpoint when you need filtering, field selection, or pagination — it avoids fetching unnecessary data.

## Example: Create Contact and Verify (bash)

```bash
#!/usr/bin/env bash
set -euo pipefail

# Assumes ABSUITE_ACCESS_TOKEN and TENANT_ID are already set (from absuite skill)

# Create a contact
CREATE_RESPONSE=$(curl -s -X POST "$ABSUITE_HOST_URL/api/v2/CrmService/Contacts" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "X-TenantId: $TENANT_ID" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d "{
    \"FirstName\": \"Alice\",
    \"LastName\": \"Johnson\",
    \"Email\": \"alice.johnson@example.com\",
    \"TenantId\": \"$TENANT_ID\"
  }")

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

## Example: Create Contact and Verify (PowerShell)

```powershell
# Assumes $accessToken and $tenantId are already set (from absuite skill)

$headers = @{
    Authorization = "Bearer $accessToken"
    "X-TenantId"  = $tenantId
    Accept        = "application/json"
}

# Create a contact
$body = @{
    FirstName = "Alice"
    LastName  = "Johnson"
    Email     = "alice.johnson@example.com"
    TenantId  = $tenantId
} | ConvertTo-Json

$createResponse = Invoke-RestMethod -Uri "$env:ABSUITE_HOST_URL/api/v2/CrmService/Contacts" `
    -Method POST -Headers $headers -ContentType "application/json" -Body $body

$contactId = $createResponse.result.contactId
Write-Host "Contact created: $contactId"

# Verify by fetching it
$contact = Invoke-RestMethod -Uri "$env:ABSUITE_HOST_URL/api/v2/CrmService/Contacts/$contactId" `
    -Method GET -Headers $headers

Write-Host "Contact details:"
$contact.result | Format-List
```
