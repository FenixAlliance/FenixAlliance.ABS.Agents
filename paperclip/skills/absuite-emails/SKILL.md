---
name: absuite-emails
description: >
  Send emails through the Alliance Business Suite (ABS) Email System. Covers
  sending basic/generic emails, contact emails, tenant emails, user emails,
  billing emails (orders, invoices, quotes), and previewing email templates.
  Requires a valid ABS bearer token (use the `absuite` skill to authenticate first).
  Do NOT use for managing email templates, groups, or signatures as CRM resources —
  this skill is for dispatching and previewing emails.
---

# Alliance Business Suite — Email Sending Skill

Send transactional and notification emails through the ABS platform. The email system has two layers that work together:

## Architecture: Templates vs. Sending Endpoints

Understanding the two layers is essential:

### Email Template Renderer (renders HTML)

The ABS host includes an email template rendering engine that **renders email templates into HTML**. It does NOT send emails — it only produces formatted HTML output. Template endpoints accept an `EmailModel` body and return rendered HTML.

- **Base URL**: `$ABSUITE_HOST_URL/Email/` (same host as the main API)
- **Purpose**: Render branded, localized email HTML from templates
- **Categories**: `/Email/Basic/`, `/Email/Account/`, `/Email/Billing/`, `/Email/Support/`, `/Email/Tenants/`, `/Email/Marketing/`, `/Email/Approvals/`, `/Email/Legal/`, `/Email/Security/`
- **Authentication**: None required (template rendering is stateless)

### Email Sending Endpoints (format + send)

The ABS REST API has email-sending endpoints that **consume the template server internally** — they handle the complete pipeline: resolve recipients, render the template, and dispatch the email via SMTP. You call these endpoints to actually send emails.

- **Base URL**: `$ABSUITE_HOST_URL/api/v2/SystemService/Emails`
- **Purpose**: Orchestrate full email delivery (template rendering + SMTP sending)
- **Authentication**: Bearer token required (global admin only for system emails)

**As an agent, you primarily use the sending endpoints.** The template renderer is used internally by the platform. You only call template endpoints directly if you need to preview/render HTML without sending.

## Prerequisites

1. **Authenticate first** using the `absuite` skill to obtain `ABSUITE_ACCESS_TOKEN`.
2. **Resolve your tenant** — email operations are tenant-aware. See Tenant Resolution below.

Env vars expected: `ABSUITE_HOST_URL`, `ABSUITE_ACCESS_TOKEN` (from login).

## Tenant Resolution

Do NOT hard-code a `TENANT_ID`. Resolve it dynamically after login:

1. Call `GET /api/v2/me/tenants` to list your accessible tenants.
2. Default to the tenant that matches the organization you work for.
3. Store the resolved `TENANT_ID` for the session.

```bash
# After login, resolve tenant
TENANTS_RESPONSE=$(curl -s -X GET "$ABSUITE_HOST_URL/api/v2/me/tenants" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN")

# Pick the first tenant (or match by name/domain for your organization)
TENANT_ID=$(echo "$TENANTS_RESPONSE" | jq -r '.result[0].id')
echo "Using tenant: $TENANT_ID"
```

```powershell
# After login, resolve tenant
$headers = @{ Authorization = "Bearer $accessToken" }
$tenants = Invoke-RestMethod -Uri "$env:ABSUITE_HOST_URL/api/v2/me/tenants" -Method GET -Headers $headers

# Pick the first tenant (or match by name/domain for your organization)
$tenantId = $tenants.result[0].id
Write-Host "Using tenant: $tenantId"
```

When you have multiple tenants, pick the one matching your organization by comparing the tenant `name` or `domain` fields.

## Approval Requirement

**Sending any email through the Alliance Business Suite requires board approval before dispatch.**

Before calling any email-sending endpoint, you MUST:

1. **Draft the email** — prepare the full `EmailDispatchRequest` payload (title, message, recipients, etc.).
2. **Request approval** — create an approval request through Paperclip with the email details, including the recipient list, subject, and message body. Tag it for the board to review.
3. **Wait for approval** — do NOT send the email until approval is granted. If denied, do not send.
4. **Send only after approval** — once approved, proceed with the API call.

This applies to all email-sending operations: `SendBasic`, contact emails, tenant emails, user emails, and billing emails. Template previews (`/Preview`) do NOT require approval since they don't dispatch.

## Agent Identity Signing

All emails sent by an agent must be signed with the agent's ABS user name. After login, fetch your profile:

```bash
ME_RESPONSE=$(curl -s -X GET "$ABSUITE_HOST_URL/api/v2/me" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN")

AGENT_NAME=$(echo "$ME_RESPONSE" | jq -r '.result.firstName + " " + .result.lastName')
```

```powershell
$me = Invoke-RestMethod -Uri "$env:ABSUITE_HOST_URL/api/v2/me" -Method GET -Headers $headers
$agentName = "$($me.result.firstName) $($me.result.lastName)"
```

Append a signature line to every email message body:

```
— Sent by {AGENT_NAME}
```

The `/api/v2/me` response includes `firstName`, `lastName`, `fullName`, and `email`. Use `fullName` (or `firstName + lastName`) as the signing identity.

## Sending Emails

### Send a Basic Email

Send a generic transactional email to one or more recipients. This is the most flexible endpoint — supports any content with optional CTA button and alert.

**Endpoint**: `POST /api/v2/SystemService/Emails/SendBasic`
**Access**: Global administrators only (`business_owner` role)
**Requires**: Board approval before sending (see Approval Requirement above)

```bash
curl -s -X POST "$ABSUITE_HOST_URL/api/v2/SystemService/Emails/SendBasic" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"title\": \"Your Weekly Report is Ready\",
    \"message\": \"Your weekly activity report has been generated and is ready for review. Click the button below to view it.\",
    \"buttonText\": \"View Report\",
    \"buttonLink\": \"https://app.example.com/reports/weekly\",
    \"alertMessage\": \"This report expires in 7 days.\",
    \"alertType\": 3,
    \"culture\": \"en\",
    \"uiCulture\": \"en\",
    \"recipients\": [\"alice@example.com\", \"bob@example.com\"]
  }"
```

**Expected response**: HTTP 200 on success.

### EmailDispatchRequest Fields

| Field | Type | Required | Description |
|---|---|---|---|
| `title` | string | Yes | Email subject line (max 150 chars) |
| `message` | string | Yes | Main email body text (min 10 chars) |
| `buttonText` | string | No | CTA button label (max 50 chars) |
| `buttonLink` | string (URL) | No | CTA button target URL |
| `alertMessage` | string | No | Alert/warning banner text |
| `alertType` | int | No | Alert style: `0`=None, `1`=Info, `2`=Error, `3`=Warning, `4`=Success, `5`=Action, `6`=Alert |
| `culture` | string | Yes | ISO language code for content (e.g., `en`, `es`, `fr`) |
| `uiCulture` | string | Yes | ISO language code for UI chrome (e.g., `en`, `es`) |
| `recipients` | string[] | Yes | List of email addresses to send to (min 1) |
| `contactIds` | string[] | No | List of ABS Contact IDs — the platform resolves their email addresses |
| `tenantIds` | string[] | No | List of ABS Tenant IDs — the platform resolves their email addresses |
| `userIds` | string[] | No | List of ABS User IDs — the platform resolves their email addresses |
| `templateUrl` | string (URL) | No | Custom template URL to use instead of the default |
| `emailTemplateId` | string | No | ID of a saved marketing email template to use |

### Recipient Resolution

You can target recipients in multiple ways — they can be combined:

- **`recipients`** — direct email addresses (always works)
- **`contactIds`** — ABS Contact GUIDs — platform resolves to contact emails
- **`tenantIds`** — ABS Tenant GUIDs — platform resolves to tenant contact emails
- **`userIds`** — ABS User GUIDs — platform resolves to user emails

### Alert Types

| Value | Name | Use case |
|---|---|---|
| `0` | None | No alert banner |
| `1` | Info | Informational notice |
| `2` | Error | Error notification |
| `3` | Warning | Warning/caution |
| `4` | Success | Success confirmation |
| `5` | Action | Action required |
| `6` | Alert | General alert |

## Previewing Emails

Preview what a rendered email will look like without sending it.

**Endpoint**: `POST /api/v2/SystemService/Emails/Preview`
**Access**: Global administrators only (`business_owner` role)
**Returns**: Rendered HTML

```bash
curl -s -X POST "$ABSUITE_HOST_URL/api/v2/SystemService/Emails/Preview" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"title\": \"Your Weekly Report is Ready\",
    \"message\": \"Your weekly activity report has been generated.\",
    \"buttonText\": \"View Report\",
    \"buttonLink\": \"https://app.example.com/reports/weekly\",
    \"culture\": \"en\",
    \"uiCulture\": \"en\",
    \"recipients\": [\"preview@example.com\"]
  }"
```

**Response**: Raw HTML of the rendered email template.

## Direct Template Rendering (Advanced)

If you need to render template HTML directly (without going through the sending pipeline), call the template renderer on the same host. This is useful for debugging templates or building custom sending flows.

**Base URL**: `$ABSUITE_HOST_URL/Email/` (same host, `/Email/` area prefix)

### Basic Templates

| Template | Method | Endpoint | Description |
|---|---|---|---|
| Action | `POST` | `/Email/Basic/Action` | Action-required email |
| Alert | `POST` | `/Email/Basic/Alert` | General alert |
| Error | `POST` | `/Email/Basic/Error` | Error notification |
| Info | `POST` | `/Email/Basic/Info` | Informational notice |
| Success | `POST` | `/Email/Basic/Success` | Success confirmation |
| Warning | `POST` | `/Email/Basic/Warning` | Warning/caution |

### Account Templates

| Template | Method | Endpoint | Description |
|---|---|---|---|
| Welcome | `GET/POST` | `/Email/Account/Welcome` | Welcome new user |
| New User | `GET/POST` | `/Email/Account/NewUser` | New user created |
| Credentials | `GET/POST` | `/Email/Account/Credentials` | Credential delivery |
| Verify Account | `GET/POST` | `/Email/Account/VerifyAccount` | Email verification |
| Change Password | `GET/POST` | `/Email/Account/ChangePasswordRequest` | Password reset request |
| Password Changed | `GET/POST` | `/Email/Account/PasswordChanged` | Password changed confirmation |
| New Device | `GET/POST` | `/Email/Account/NewDevice` | New device login detected |

### Billing Templates

| Template | Method | Endpoint | Description |
|---|---|---|---|
| New Invoice | `POST` | `/Email/Billing/NewInvoice` | Invoice created |
| Invoice Ready | `POST` | `/Email/Billing/InvoiceReady` | Invoice ready for payment |
| Invoice Paid | `POST` | `/Email/Billing/InvoicePaidConfirmation` | Invoice paid confirmation |
| Invoice Reminder | `POST` | `/Email/Billing/InvoiceReminder` | Invoice payment reminder |
| Invoice Due Soon | `POST` | `/Email/Billing/InvoiceDueSoon` | Invoice approaching due date |
| Invoice Overdue | `POST` | `/Email/Billing/InvoiceOverdue` | Invoice past due |
| New Order | `POST` | `/Email/Billing/NewOrder` | New order placed |
| New Payment | `POST` | `/Email/Billing/NewPayment` | Payment received |
| New Quote | `POST` | `/Email/Billing/NewQuote` | New quote generated |

### Support Templates

| Template | Method | Endpoint | Description |
|---|---|---|---|
| New Inquiry | `POST` | `/Email/Support/NewInquiry` | New support inquiry |
| New Ticket | `POST` | `/Email/Support/NewTicket` | New support ticket |
| Ticket Status Changed | `POST` | `/Email/Support/TicketStatusChanged` | Ticket status update |

### Template Request Body (EmailModel)

When calling template endpoints directly, use this body shape:

```json
{
  "title": "Email Subject Line",
  "message": "Main email body text.",
  "buttonText": "Call to Action",
  "buttonLink": "https://example.com/action",
  "alertMessage": "Optional alert/warning text.",
  "alertType": 3,
  "headerLogoUrl": "https://email.absuite.net/images/icx.logo.white.png",
  "footerLogoUrl": "https://email.absuite.net/images/abs.logo.white.png",
  "portalUrl": "https://example.com",
  "trackingPixel": "tracking-id-string"
}
```

### Template Localization

All template endpoints support localization via query parameters:

| Parameter | Example | Description |
|---|---|---|
| `culture` | `es` | Content language |
| `ui-culture` | `es` | UI chrome language |
| `devStyles` | `true` | Enable development-mode styling (debugging only) |

Example: `/Email/Basic/Action?culture=es&ui-culture=es`

## Managing Email Resources (Marketing Service)

The Marketing Service provides CRUD endpoints for managing email resources as tenant-owned entities:

### Email Templates (saved, reusable)

| Action | Method | Endpoint |
|---|---|---|
| List templates (OData) | `GET` | `/api/v2/MarketingService/EmailTemplates` |
| Count templates | `GET` | `/api/v2/MarketingService/EmailTemplates/Count` |
| Get template | `GET` | `/api/v2/MarketingService/EmailTemplates/{id}` |
| Create template | `POST` | `/api/v2/MarketingService/EmailTemplates` |
| Update template | `PUT` | `/api/v2/MarketingService/EmailTemplates/{id}` |
| Delete template | `DELETE` | `/api/v2/MarketingService/EmailTemplates/{id}` |

### Email Groups (recipient lists)

| Action | Method | Endpoint |
|---|---|---|
| List groups (OData) | `GET` | `/api/v2/MarketingService/EmailGroups` |
| Count groups | `GET` | `/api/v2/MarketingService/EmailGroups/Count` |
| Get group | `GET` | `/api/v2/MarketingService/EmailGroups/{id}` |
| Create group | `POST` | `/api/v2/MarketingService/EmailGroups` |
| Update group | `PUT` | `/api/v2/MarketingService/EmailGroups/{id}` |
| Delete group | `DELETE` | `/api/v2/MarketingService/EmailGroups/{id}` |

### Email Signatures

| Action | Method | Endpoint |
|---|---|---|
| List signatures (OData) | `GET` | `/api/v2/MarketingService/EmailSignatures` |
| Count signatures | `GET` | `/api/v2/MarketingService/EmailSignatures/Count` |
| Get signature | `GET` | `/api/v2/MarketingService/EmailSignatures/{id}` |
| Create signature | `POST` | `/api/v2/MarketingService/EmailSignatures` |
| Update signature | `PUT` | `/api/v2/MarketingService/EmailSignatures/{id}` |
| Delete signature | `DELETE` | `/api/v2/MarketingService/EmailSignatures/{id}` |

All Marketing Service endpoints require `Authorization: Bearer` and `X-TenantId` headers.

### View a Saved Email Template (rendered)

Render a saved marketing email template by ID:

```bash
curl -s -X GET "$ABSUITE_HOST_URL/api/v2/Marketing/EmailTemplates/{templateId}/View?tenantId=$TENANT_ID&data=eyB9" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN"
```

The `data` query parameter is a Base64-encoded JSON payload for template variable substitution. Use `eyB9` (Base64 for `{ }`) for an empty payload.

## Critical Rules

- **Board approval is REQUIRED before sending any email.** Draft the email, request approval through Paperclip, and only send after approval is granted. Previews are exempt.
- **Sign every email with your ABS user name.** Fetch your name from `/api/v2/me` and append `— Sent by {name}` to the message body.
- **Authenticate first.** Use the `absuite` skill to obtain a bearer token.
- **System email endpoints require `business_owner` role.** The `SendBasic` and `Preview` endpoints are restricted to global administrators.
- **Marketing Service endpoints require `X-TenantId`.** Email templates, groups, and signatures are tenant-scoped.
- **Use `recipients` for direct addresses, ID fields for platform-resolved addresses.** You can combine both — `recipients` for external addresses and `contactIds`/`userIds`/`tenantIds` for platform-known entities.
- **Never hard-code credentials, tokens, or tenant IDs.** Resolve the tenant dynamically from `/api/v2/me/tenants`.
- **The template renderer does NOT send emails.** It only renders HTML. To actually send, use the `SystemService/Emails/SendBasic` endpoint.
- **Localization is controlled per-request.** Always set `culture` and `uiCulture` to match the recipient's preferred language.
- **Template endpoints live under `/Email/` on the same host.** There is no separate email server URL.

## Example: Send an Email to Contacts and Direct Recipients (bash)

```bash
#!/usr/bin/env bash
set -euo pipefail

# Assumes ABSUITE_ACCESS_TOKEN is already set (from absuite skill)

curl -s -X POST "$ABSUITE_HOST_URL/api/v2/SystemService/Emails/SendBasic" \
  -H "Authorization: Bearer $ABSUITE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"title\": \"Monthly Newsletter: April 2026\",
    \"message\": \"Here is your monthly update with the latest news, product updates, and upcoming events.\\n\\n— Sent by $AGENT_NAME\",
    \"buttonText\": \"Read Full Newsletter\",
    \"buttonLink\": \"https://app.example.com/newsletter/april-2026\",
    \"alertMessage\": \"You are receiving this because you are subscribed to our monthly updates.\",
    \"alertType\": 1,
    \"culture\": \"en\",
    \"uiCulture\": \"en\",
    \"recipients\": [\"external-partner@example.com\"],
    \"contactIds\": [\"$CONTACT_ID_1\", \"$CONTACT_ID_2\"],
    \"userIds\": [\"$USER_ID_1\"]
  }"

echo "Email sent successfully."
```

## Example: Send an Email (PowerShell)

```powershell
# Assumes $accessToken is already set (from absuite skill)

$body = @{
    title        = "Monthly Newsletter: April 2026"
    message      = "Here is your monthly update with the latest news.`n`n— Sent by $agentName"
    buttonText   = "Read Full Newsletter"
    buttonLink   = "https://app.example.com/newsletter/april-2026"
    alertMessage = "You are receiving this because you are subscribed."
    alertType    = 1
    culture      = "en"
    uiCulture    = "en"
    recipients   = @("external-partner@example.com")
    contactIds   = @("$contactId1", "$contactId2")
} | ConvertTo-Json

$headers = @{ Authorization = "Bearer $accessToken" }

Invoke-RestMethod -Uri "$env:ABSUITE_HOST_URL/api/v2/SystemService/Emails/SendBasic" `
    -Method POST -Headers $headers -ContentType "application/json" -Body $body

Write-Host "Email sent successfully."
```
