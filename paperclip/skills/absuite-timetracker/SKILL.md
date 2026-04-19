---
name: absuite-timetracker
description: >
  Manage project time tracking in the Alliance Business Suite (ABS) using the
  `absuite` CLI. Covers time logs, approval workflows, and time queries by
  responsible contact or creator. Requires an authenticated CLI session.
---

# Alliance Business Suite — Time Tracker Skill

Manage time tracking through the `absuite` CLI's `timetracker` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite timetracker list-commands`

## Project Time Logs

```bash
# List time logs
absuite timetracker list project-time-logs --TenantId $TENANT_ID

# Count time logs
absuite timetracker count project-time-logs --TenantId $TENANT_ID

# Get by ID
absuite timetracker get project-time-log --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid>

# Create time log
absuite timetracker create project-time-log --TenantId $TENANT_ID --ProjectTimeLogCreateDto '{
  "ProjectId": "<project-guid>",
  "ProjectTaskId": "<task-guid>",
  "Hours": 4.5,
  "Description": "Code review and refactoring",
  "Date": "2026-04-19T00:00:00Z"
}'

# Update time log
absuite timetracker update project-time-log --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid> --ProjectTimeLogUpdateDto '{...}'

# Delete time log
absuite timetracker delete project-time-log --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid>
```

## Querying Time Logs

### By Responsible Contact

```bash
absuite timetracker list project-time-logs-by-responsible-contact --TenantId $TENANT_ID --ContactId <contact-guid>
absuite timetracker count project-time-logs-by-responsible-contact --TenantId $TENANT_ID --ContactId <contact-guid>
```

### By Creator

```bash
absuite timetracker list project-time-logs-by-creator --TenantId $TENANT_ID --ContactId <contact-guid>
absuite timetracker count project-time-logs-by-creator --TenantId $TENANT_ID --ContactId <contact-guid>
```

## Approval Workflow

```bash
# Request approval for a time log
absuite timetracker request-approval --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid>

# Update approval status (approve / reject)
absuite timetracker update-approval-status --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid> --ApprovalStatus "Approved"

# Update the approver assigned to a time log
absuite timetracker update-approver --TenantId $TENANT_ID --ProjectTimeLogId <timelog-guid> --ApproverId <contact-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List time logs | `absuite timetracker list project-time-logs --TenantId <guid>` |
| Create time log | `absuite timetracker create project-time-log --TenantId <guid> --ProjectTimeLogCreateDto '{...}'` |
| By responsible | `absuite timetracker list project-time-logs-by-responsible-contact --TenantId <guid> --ContactId <guid>` |
| By creator | `absuite timetracker list project-time-logs-by-creator --TenantId <guid> --ContactId <guid>` |
| Request approval | `absuite timetracker request-approval --TenantId <guid> --ProjectTimeLogId <guid>` |
| Approve time log | `absuite timetracker update-approval-status --TenantId <guid> --ProjectTimeLogId <guid> --ApprovalStatus "Approved"` |

## Full Example: Log & Approve Time

```bash
# 1. Log time
absuite timetracker create project-time-log --ProjectTimeLogCreateDto '{
  "ProjectId": "<project-guid>",
  "ProjectTaskId": "<task-guid>",
  "Hours": 8,
  "Description": "Sprint development work",
  "Date": "2026-04-19T00:00:00Z"
}'

# 2. Request approval
absuite timetracker request-approval --ProjectTimeLogId <timelog-guid>

# 3. Manager approves
absuite timetracker update-approval-status --ProjectTimeLogId <timelog-guid> --ApprovalStatus "Approved"
```

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Time logs reference projects and tasks** — create projects/tasks first (see `absuite-projects` skill).
- **Use the approval workflow** for time entries that require manager sign-off.
