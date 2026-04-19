---
name: absuite-tenants
description: >
  Manage tenant configuration in the Alliance Business Suite (ABS) using the `absuite`
  CLI. The largest service — covers tenant CRUD, departments, enrollments (employee
  and standard), industries, invitations, notifications, options, positions, segments,
  sizes, teams, territories, types, units/unit groups, licenses, web portals, avatars,
  carts, wallets, and social profiles. Requires an authenticated CLI session.
---

# Alliance Business Suite — Tenants Skill

Manage tenant configuration through the `absuite` CLI's `tenants` service. This is the most comprehensive service with 140+ commands. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite tenants list-commands`

## Tenant Basics

```bash
# Get current tenant
absuite tenants get --TenantId $TENANT_ID

# Get extended tenant
absuite tenants get extended --TenantId $TENANT_ID

# Update tenant
absuite tenants update --TenantId $TENANT_ID --TenantUpdateDto '{
  "Name": "My Business",
  "LegalName": "My Business LLC",
  "Email": "admin@mybusiness.com"
}'

# Patch tenant (partial update)
absuite tenants patch --TenantId $TENANT_ID --TenantPatchDto '{
  "Name": "Updated Name"
}'

# Create tenant
absuite tenants create --TenantCreateDto '{
  "Name": "New Venture",
  "LegalName": "New Venture Inc.",
  "Email": "info@newventure.com",
  "Phone": "+1-555-0100",
  "WebUrl": "https://newventure.com",
  "Handler": "newventure",
  "About": "A new business venture",
  "Slogan": "Innovation first",
  "CurrencyId": "<currency-guid>",
  "CountryId": "USA"
}'
```

## Departments

```bash
absuite tenants list departments --TenantId $TENANT_ID
absuite tenants count departments --TenantId $TENANT_ID
absuite tenants get department --TenantId $TENANT_ID --DepartmentId <dept-guid>
absuite tenants create department --TenantId $TENANT_ID --DepartmentCreateDto '{
  "Name": "Engineering",
  "Description": "Software engineering department"
}'
absuite tenants update department --TenantId $TENANT_ID --DepartmentId <dept-guid> --DepartmentUpdateDto '{...}'
absuite tenants delete department --TenantId $TENANT_ID --DepartmentId <dept-guid>
```

## Enrollments (User Memberships)

```bash
# Standard enrollments
absuite tenants list enrollments --TenantId $TENANT_ID
absuite tenants count enrollments --TenantId $TENANT_ID
absuite tenants get enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>
absuite tenants create enrollment --TenantId $TENANT_ID --EnrollmentCreateDto '{...}'
absuite tenants update enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --EnrollmentUpdateDto '{...}'
absuite tenants delete enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>

# Extended enrollments (with related data)
absuite tenants list extended-enrollments --TenantId $TENANT_ID
absuite tenants count extended-enrollments --TenantId $TENANT_ID
absuite tenants get extended-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>

# Employee enrollments
absuite tenants list employee-enrollments --TenantId $TENANT_ID
absuite tenants count employee-enrollments --TenantId $TENANT_ID
absuite tenants get employee-enrollment --TenantId $TENANT_ID --EmployeeEnrollmentId <ee-guid>
absuite tenants create employee-enrollment --TenantId $TENANT_ID --EmployeeEnrollmentCreateDto '{...}'
absuite tenants update employee-enrollment --TenantId $TENANT_ID --EmployeeEnrollmentId <ee-guid> --EmployeeEnrollmentUpdateDto '{...}'
absuite tenants delete employee-enrollment --TenantId $TENANT_ID --EmployeeEnrollmentId <ee-guid>
```

## Invitations

```bash
# List
absuite tenants list invitations --TenantId $TENANT_ID
absuite tenants count invitations --TenantId $TENANT_ID

# Send invitation
absuite tenants send invitation --TenantId $TENANT_ID --InvitationCreateDto '{
  "Email": "newmember@example.com",
  "RoleId": "<role-guid>"
}'

# Accept / Decline / Revoke
absuite tenants accept invitation --InvitationId <inv-guid>
absuite tenants decline invitation --InvitationId <inv-guid>
absuite tenants revoke invitation --TenantId $TENANT_ID --InvitationId <inv-guid>
```

## Notifications

```bash
absuite tenants list notifications --TenantId $TENANT_ID
absuite tenants count notifications --TenantId $TENANT_ID
absuite tenants get notification --TenantId $TENANT_ID --NotificationId <notif-guid>
absuite tenants create notification --TenantId $TENANT_ID --NotificationCreateDto '{...}'
absuite tenants update notification --TenantId $TENANT_ID --NotificationId <notif-guid> --NotificationUpdateDto '{...}'
absuite tenants delete notification --TenantId $TENANT_ID --NotificationId <notif-guid>
```

## Tenant Options (Key-Value Configuration)

```bash
absuite tenants list options --TenantId $TENANT_ID
absuite tenants count options --TenantId $TENANT_ID
absuite tenants get option-by-id --TenantId $TENANT_ID --OptionId <option-guid>
absuite tenants get option-by-key --TenantId $TENANT_ID --Key "feature.new-ui-enabled"
absuite tenants create option --TenantId $TENANT_ID --TenantOptionCreateDto '{
  "Key": "feature.new-ui-enabled",
  "Value": "true"
}'
absuite tenants update option --TenantId $TENANT_ID --OptionId <option-guid> --TenantOptionUpdateDto '{...}'
absuite tenants upsert option --TenantId $TENANT_ID --Key "feature.new-ui-enabled" --TenantOptionUpdateDto '{"Value": "false"}'
absuite tenants delete option --TenantId $TENANT_ID --OptionId <option-guid>
```

## Positions

```bash
absuite tenants list positions --TenantId $TENANT_ID
absuite tenants count positions --TenantId $TENANT_ID
absuite tenants get position --TenantId $TENANT_ID --PositionId <position-guid>
absuite tenants create position --TenantId $TENANT_ID --PositionCreateDto '{
  "Name": "Senior Engineer",
  "Description": "Senior software engineering role"
}'
absuite tenants update position --TenantId $TENANT_ID --PositionId <position-guid> --PositionUpdateDto '{...}'
absuite tenants delete position --TenantId $TENANT_ID --PositionId <position-guid>
```

## Teams

```bash
# Team CRUD
absuite tenants list teams --TenantId $TENANT_ID
absuite tenants count teams --TenantId $TENANT_ID
absuite tenants get team --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants create team --TenantId $TENANT_ID --TeamCreateDto '{
  "Name": "Platform Team",
  "Description": "Platform engineering team"
}'
absuite tenants update team --TenantId $TENANT_ID --TeamId <team-guid> --TeamUpdateDto '{...}'
absuite tenants delete team --TenantId $TENANT_ID --TeamId <team-guid>

# Team contact enrollments
absuite tenants list team-contact-enrollments --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants count team-contact-enrollments --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants create team-contact-enrollment --TenantId $TENANT_ID --TeamId <team-guid> --TeamContactEnrollmentCreateDto '{...}'
absuite tenants delete team-contact-enrollment --TenantId $TENANT_ID --TeamContactEnrollmentId <tce-guid>

# Team project enrollments
absuite tenants list team-project-enrollments --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants count team-project-enrollments --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants create team-project-enrollment --TenantId $TENANT_ID --TeamId <team-guid> --TeamProjectEnrollmentCreateDto '{...}'
absuite tenants delete team-project-enrollment --TenantId $TENANT_ID --TeamProjectEnrollmentId <tpe-guid>

# Team records
absuite tenants list team-records --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants count team-records --TenantId $TENANT_ID --TeamId <team-guid>
absuite tenants create team-record --TenantId $TENANT_ID --TeamId <team-guid> --TeamRecordCreateDto '{...}'
absuite tenants update team-record --TenantId $TENANT_ID --TeamRecordId <record-guid> --TeamRecordUpdateDto '{...}'
absuite tenants delete team-record --TenantId $TENANT_ID --TeamRecordId <record-guid>
```

## Territories

```bash
absuite tenants list territories --TenantId $TENANT_ID
absuite tenants count territories --TenantId $TENANT_ID
absuite tenants get territory --TenantId $TENANT_ID --TerritoryId <territory-guid>
absuite tenants create territory --TenantId $TENANT_ID --TerritoryCreateDto '{
  "Name": "North America",
  "Description": "North American sales territory"
}'
absuite tenants update territory --TenantId $TENANT_ID --TerritoryId <territory-guid> --TerritoryUpdateDto '{...}'
absuite tenants delete territory --TenantId $TENANT_ID --TerritoryId <territory-guid>
```

## Industries, Segments, Sizes, Types

```bash
# Industries
absuite tenants list industries --TenantId $TENANT_ID
absuite tenants count industries --TenantId $TENANT_ID
absuite tenants get industry --TenantId $TENANT_ID --IndustryId <industry-guid>
absuite tenants create industry --TenantId $TENANT_ID --IndustryCreateDto '{...}'
absuite tenants update industry --TenantId $TENANT_ID --IndustryId <industry-guid> --IndustryUpdateDto '{...}'
absuite tenants delete industry --TenantId $TENANT_ID --IndustryId <industry-guid>

# Segments
absuite tenants list segments --TenantId $TENANT_ID
absuite tenants count segments --TenantId $TENANT_ID
absuite tenants get segment --TenantId $TENANT_ID --SegmentId <segment-guid>
absuite tenants create segment --TenantId $TENANT_ID --SegmentCreateDto '{...}'
absuite tenants update segment --TenantId $TENANT_ID --SegmentId <segment-guid> --SegmentUpdateDto '{...}'
absuite tenants delete segment --TenantId $TENANT_ID --SegmentId <segment-guid>

# Sizes
absuite tenants list sizes --TenantId $TENANT_ID
absuite tenants count sizes --TenantId $TENANT_ID
absuite tenants get size --TenantId $TENANT_ID --SizeId <size-guid>
absuite tenants create size --TenantId $TENANT_ID --SizeCreateDto '{...}'
absuite tenants update size --TenantId $TENANT_ID --SizeId <size-guid> --SizeUpdateDto '{...}'
absuite tenants delete size --TenantId $TENANT_ID --SizeId <size-guid>

# Types
absuite tenants list types --TenantId $TENANT_ID
absuite tenants count types --TenantId $TENANT_ID
absuite tenants get type --TenantId $TENANT_ID --TypeId <type-guid>
absuite tenants create type --TenantId $TENANT_ID --TypeCreateDto '{...}'
absuite tenants update type --TenantId $TENANT_ID --TypeId <type-guid> --TypeUpdateDto '{...}'
absuite tenants delete type --TenantId $TENANT_ID --TypeId <type-guid>
```

## Units & Unit Groups

```bash
# Unit groups
absuite tenants list unit-groups --TenantId $TENANT_ID
absuite tenants count unit-groups --TenantId $TENANT_ID
absuite tenants get unit-group --TenantId $TENANT_ID --UnitGroupId <group-guid>
absuite tenants create unit-group --TenantId $TENANT_ID --UnitGroupCreateDto '{...}'
absuite tenants update unit-group --TenantId $TENANT_ID --UnitGroupId <group-guid> --UnitGroupUpdateDto '{...}'
absuite tenants delete unit-group --TenantId $TENANT_ID --UnitGroupId <group-guid>

# Units
absuite tenants list units --TenantId $TENANT_ID
absuite tenants count units --TenantId $TENANT_ID
absuite tenants get unit --TenantId $TENANT_ID --UnitId <unit-guid>
absuite tenants create unit --TenantId $TENANT_ID --UnitCreateDto '{...}'
absuite tenants update unit --TenantId $TENANT_ID --UnitId <unit-guid> --UnitUpdateDto '{...}'
absuite tenants delete unit --TenantId $TENANT_ID --UnitId <unit-guid>
```

## Licenses

```bash
absuite tenants list licenses --TenantId $TENANT_ID
absuite tenants get license --TenantId $TENANT_ID --LicenseId <license-guid>
```

## Web Portals

```bash
absuite tenants list web-portals --TenantId $TENANT_ID
absuite tenants get web-portal --TenantId $TENANT_ID --WebPortalId <portal-guid>
```

## Tenant-Level Resources

```bash
# Avatar
absuite tenants get avatar --TenantId $TENANT_ID

# Cart
absuite tenants get cart --TenantId $TENANT_ID

# Wallet
absuite tenants get wallet --TenantId $TENANT_ID

# Social profile
absuite tenants get social-profile --TenantId $TENANT_ID
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Get tenant | `absuite tenants get --TenantId <guid>` |
| Update tenant | `absuite tenants update --TenantId <guid> --TenantUpdateDto '{...}'` |
| Patch tenant | `absuite tenants patch --TenantId <guid> --TenantPatchDto '{...}'` |
| List departments | `absuite tenants list departments --TenantId <guid>` |
| List enrollments | `absuite tenants list enrollments --TenantId <guid>` |
| Send invitation | `absuite tenants send invitation --TenantId <guid> --InvitationCreateDto '{...}'` |
| Accept invitation | `absuite tenants accept invitation --InvitationId <guid>` |
| List teams | `absuite tenants list teams --TenantId <guid>` |
| List options | `absuite tenants list options --TenantId <guid>` |
| Upsert option | `absuite tenants upsert option --TenantId <guid> --Key <k> --TenantOptionUpdateDto '{...}'` |
| List positions | `absuite tenants list positions --TenantId <guid>` |
| List territories | `absuite tenants list territories --TenantId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Enrollments** are how users join a tenant — each enrollment can have roles and permissions assigned via the security service.
- **Invitation lifecycle**: send → accept/decline. Revoke to cancel pending invitations.
- **Options** store tenant-level configuration — use `upsert option` for idempotent key-value updates.
- **Teams** can have both contact enrollments and project enrollments — use these to organize members and projects.
