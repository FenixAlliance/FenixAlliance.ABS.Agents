---
name: absuite-support
description: >
  Manage customer support tickets, support requests, request attachments, ticket
  conversations, ticket priorities, ticket types, and support entitlements in the
  Alliance Business Suite (ABS) using the `absuite` CLI. Requires an authenticated
  CLI session.
---

# Alliance Business Suite — Support Skill

Manage customer support through the `absuite` CLI's `support` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite support list-commands`

## Support Tickets

```bash
# List
absuite support list tickets --TenantId $TENANT_ID

# Count
absuite support count tickets --TenantId $TENANT_ID

# Get by ID
absuite support get ticket --TenantId $TENANT_ID --SupportTicketId <ticket-guid>

# Create
absuite support create ticket --TenantId $TENANT_ID --SupportTicketCreateDto '{
  "Description": "Cannot access billing portal",
  "ContactID": "<contact-guid>",
  "SupportTicketTypeID": "<type-guid>",
  "SupportPriorityID": "<priority-guid>",
  "SupportEntitlementID": "<entitlement-guid>"
}'

# Update
absuite support update ticket --TenantId $TENANT_ID --SupportTicketId <ticket-guid> --SupportTicketUpdateDto '{...}'

# Delete
absuite support delete ticket --TenantId $TENANT_ID --SupportTicketId <ticket-guid>
```

### Ticket Conversations

```bash
# List conversations for a ticket
absuite support list ticket-conversations --TenantId $TENANT_ID --SupportTicketId <ticket-guid>

# Get a specific conversation
absuite support get ticket-conversation --TenantId $TENANT_ID --SupportTicketId <ticket-guid> --ConversationId <conv-guid>

# Create a conversation on a ticket
absuite support relate-support-ticket-to-conversation --TenantId $TENANT_ID --SupportTicketId <ticket-guid> --ConversationCreateDto '{...}'

# List messages in a conversation
absuite support list ticket-conversation-messages --TenantId $TENANT_ID --ConversationId <conv-guid>

# Delete a conversation
absuite support delete ticket-conversation --TenantId $TENANT_ID --SupportTicketId <ticket-guid> --ConversationId <conv-guid>
```

### Ticket Priorities

```bash
absuite support list ticket-priorities --TenantId $TENANT_ID
absuite support count ticket-priorities --TenantId $TENANT_ID
absuite support get ticket-priority --TenantId $TENANT_ID --SupportTicketPriorityId <priority-guid>
absuite support create ticket-priority --TenantId $TENANT_ID --SupportTicketPriorityCreateDto '{
  "Name": "Critical",
  "Description": "System-down or data-loss scenarios"
}'
absuite support update ticket-priority --TenantId $TENANT_ID --SupportTicketPriorityId <priority-guid> --SupportTicketPriorityUpdateDto '{...}'
absuite support delete ticket-priority --TenantId $TENANT_ID --SupportTicketPriorityId <priority-guid>
```

### Ticket Types

```bash
absuite support list ticket-types --TenantId $TENANT_ID
absuite support count ticket-types --TenantId $TENANT_ID
absuite support get ticket-type --TenantId $TENANT_ID --SupportTicketTypeId <type-guid>
absuite support create ticket-type --TenantId $TENANT_ID --SupportTicketTypeCreateDto '{
  "Name": "Bug Report",
  "Description": "Software defect or error"
}'
absuite support update ticket-type --TenantId $TENANT_ID --SupportTicketTypeId <type-guid> --SupportTicketTypeUpdateDto '{...}'
absuite support delete ticket-type --TenantId $TENANT_ID --SupportTicketTypeId <type-guid>
```

## Support Requests

```bash
# List
absuite support list requests --TenantId $TENANT_ID

# Count
absuite support count requests --TenantId $TENANT_ID

# Get by ID
absuite support get request --TenantId $TENANT_ID --SupportRequestId <request-guid>

# Create
absuite support create request --TenantId $TENANT_ID --SupportRequestCreateDto '{
  "Description": "Need assistance with invoice reconciliation"
}'

# Update
absuite support update request --TenantId $TENANT_ID --SupportRequestId <request-guid> --SupportRequestUpdateDto '{...}'

# Delete
absuite support delete request --TenantId $TENANT_ID --SupportRequestId <request-guid>

# List tickets for a request
absuite support list request-tickets --TenantId $TENANT_ID --SupportRequestId <request-guid>
```

### Request Attachments

```bash
# List all
absuite support list request-attachments --TenantId $TENANT_ID

# Count
absuite support count request-attachments --TenantId $TENANT_ID

# Get by ID
absuite support get request-attachment --TenantId $TENANT_ID --SupportRequestAttachmentId <attachment-guid>

# Get attachments for a request
absuite support get request-attachments-by-request --TenantId $TENANT_ID --SupportRequestId <request-guid>
absuite support get request-attachments-count-by-request --TenantId $TENANT_ID --SupportRequestId <request-guid>
absuite support get request-attachment-by-request --TenantId $TENANT_ID --SupportRequestId <request-guid> --SupportRequestAttachmentId <attachment-guid>

# Create
absuite support create request-attachment --TenantId $TENANT_ID --SupportRequestAttachmentCreateDto '{...}'

# Relate attachment to request
absuite support relate-support-request-to-attachment --TenantId $TENANT_ID --SupportRequestId <request-guid> --SupportRequestAttachmentId <attachment-guid>

# Update
absuite support update request-attachment --TenantId $TENANT_ID --SupportRequestAttachmentId <attachment-guid> --SupportRequestAttachmentUpdateDto '{...}'

# Delete
absuite support delete request-attachment --TenantId $TENANT_ID --SupportRequestAttachmentId <attachment-guid>
```

## Support Entitlements

Define what level of support a customer is entitled to.

```bash
# List
absuite support list entitlements --TenantId $TENANT_ID

# Count
absuite support count entitlements --TenantId $TENANT_ID

# Get by ID
absuite support get entitlement --TenantId $TENANT_ID --SupportEntitlementId <entitlement-guid>

# Create
absuite support create entitlement --TenantId $TENANT_ID --SupportEntitlementCreateDto '{
  "Name": "Premium Support",
  "Description": "24/7 support with 1-hour SLA"
}'

# Update
absuite support update entitlement --TenantId $TENANT_ID --SupportEntitlementId <entitlement-guid> --SupportEntitlementUpdateDto '{...}'

# Delete
absuite support delete entitlement --TenantId $TENANT_ID --SupportEntitlementId <entitlement-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List tickets | `absuite support list tickets --TenantId <guid>` |
| Create ticket | `absuite support create ticket --TenantId <guid> --SupportTicketCreateDto '{...}'` |
| List conversations | `absuite support list ticket-conversations --TenantId <guid> --SupportTicketId <guid>` |
| List requests | `absuite support list requests --TenantId <guid>` |
| Create request | `absuite support create request --TenantId <guid> --SupportRequestCreateDto '{...}'` |
| List priorities | `absuite support list ticket-priorities --TenantId <guid>` |
| List types | `absuite support list ticket-types --TenantId <guid>` |
| List entitlements | `absuite support list entitlements --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Set up priorities, types, and entitlements first** before creating tickets.
- **Use conversations on tickets** for threaded discussion with customers.
