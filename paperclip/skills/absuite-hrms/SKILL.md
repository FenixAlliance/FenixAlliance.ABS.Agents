---
name: absuite-hrms
description: >
  Manage human resources in the Alliance Business Suite (ABS) using the `absuite`
  CLI. Covers employees, employers, gigs, and job offers. Requires an authenticated
  CLI session.
---

# Alliance Business Suite — HRMS Skill

Manage human resources through the `absuite` CLI's `hrms` service. All operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite hrms list-commands`

## Employees

```bash
# List
absuite hrms list employees --TenantId $TENANT_ID

# Count
absuite hrms count employees --TenantId $TENANT_ID

# Get by ID
absuite hrms get employee-by-id --TenantId $TENANT_ID --EmployeeId <employee-guid>

# Create
absuite hrms create employee --TenantId $TENANT_ID --EmployeeCreateDto '{
  "ContactId": "<contact-guid>",
  "EmployerId": "<employer-guid>",
  "Title": "Software Engineer",
  "Department": "Engineering"
}'

# Update
absuite hrms update employee --TenantId $TENANT_ID --EmployeeId <employee-guid> --EmployeeUpdateDto '{...}'

# Delete
absuite hrms delete employee --TenantId $TENANT_ID --EmployeeId <employee-guid>
```

## Employers

```bash
# List
absuite hrms list employers --TenantId $TENANT_ID

# Count
absuite hrms count employers --TenantId $TENANT_ID

# Get by ID
absuite hrms get employer-by-id --TenantId $TENANT_ID --EmployerId <employer-guid>

# Create
absuite hrms create employer --TenantId $TENANT_ID --EmployerCreateDto '{
  "Name": "Acme Corporation",
  "Description": "Technology company"
}'

# Update
absuite hrms update employer --TenantId $TENANT_ID --EmployerId <employer-guid> --EmployerUpdateDto '{...}'

# Delete
absuite hrms delete employer --TenantId $TENANT_ID --EmployerId <employer-guid>
```

## Gigs

Short-term or freelance work assignments.

```bash
# List
absuite hrms list gigs --TenantId $TENANT_ID

# Count
absuite hrms count gigs --TenantId $TENANT_ID

# Get by ID
absuite hrms get gig-by-id --TenantId $TENANT_ID --GigId <gig-guid>

# Create
absuite hrms create gig --TenantId $TENANT_ID --GigCreateDto '{
  "Title": "Frontend Development Sprint",
  "Description": "Build checkout flow components"
}'

# Update
absuite hrms update gig --TenantId $TENANT_ID --GigId <gig-guid> --GigUpdateDto '{...}'

# Delete
absuite hrms delete gig --TenantId $TENANT_ID --GigId <gig-guid>
```

## Job Offers

```bash
# List
absuite hrms list job-offers --TenantId $TENANT_ID

# Count
absuite hrms count job-offers --TenantId $TENANT_ID

# Get by ID
absuite hrms get job-offer-by-id --TenantId $TENANT_ID --JobOfferId <offer-guid>

# Create
absuite hrms create job-offer --TenantId $TENANT_ID --JobOfferCreateDto '{
  "Title": "Senior .NET Developer",
  "Description": "Remote position, full-time"
}'

# Update
absuite hrms update job-offer --TenantId $TENANT_ID --JobOfferId <offer-guid> --JobOfferUpdateDto '{...}'

# Delete
absuite hrms delete job-offer --TenantId $TENANT_ID --JobOfferId <offer-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List employees | `absuite hrms list employees --TenantId <guid>` |
| Create employee | `absuite hrms create employee --TenantId <guid> --EmployeeCreateDto '{...}'` |
| List employers | `absuite hrms list employers --TenantId <guid>` |
| Create employer | `absuite hrms create employer --TenantId <guid> --EmployerCreateDto '{...}'` |
| List gigs | `absuite hrms list gigs --TenantId <guid>` |
| Create gig | `absuite hrms create gig --TenantId <guid> --GigCreateDto '{...}'` |
| List job offers | `absuite hrms list job-offers --TenantId <guid>` |
| Create job offer | `absuite hrms create job-offer --TenantId <guid> --JobOfferCreateDto '{...}'` |

## Critical Rules

- **Authenticate first.** Use `absuite login` before any HRMS operation.
- **Always provide a tenant context.**
- **Create employers first** before creating employees that reference them.
- **Use `--help`** on any command for full DTO schemas.
