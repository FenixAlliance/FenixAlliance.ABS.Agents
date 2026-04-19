---
name: absuite-projects
description: >
  Manage projects, project tasks, task categories, task types, project periods, and
  time logs in the Alliance Business Suite (ABS) using the `absuite` CLI. Requires
  an authenticated CLI session.
---

# Alliance Business Suite — Projects Skill

Manage projects through the `absuite` CLI's `projects` service. All operations are tenant-scoped.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite projects list-commands`

## Projects

```bash
# List
absuite projects get projects-by-tenant-id --TenantId $TENANT_ID

# Count
absuite projects get projects-count-by-tenant-id --TenantId $TENANT_ID

# Get by ID
absuite projects get by-id --TenantId $TENANT_ID --ProjectId <project-guid>

# Create
absuite projects create --TenantId $TENANT_ID --ProjectCreateDto '{
  "Title": "Website Redesign",
  "Description": "Complete redesign of the company website",
  "ProjectStartDate": "2026-04-01T00:00:00Z",
  "ProjectEndDate": "2026-09-30T00:00:00Z"
}'

# Update
absuite projects update --TenantId $TENANT_ID --ProjectId <project-guid> --ProjectUpdateDto '{...}'

# Delete
absuite projects delete --TenantId $TENANT_ID --ProjectId <project-guid>
```

## Project Tasks

```bash
# List tasks for a project
absuite projects list tasks --TenantId $TENANT_ID --ProjectId <project-guid>

# Count tasks
absuite projects count tasks --TenantId $TENANT_ID --ProjectId <project-guid>

# Create task
absuite projects create task --TenantId $TENANT_ID --ProjectTaskCreateDto '{
  "Title": "Design homepage wireframe",
  "Description": "Create wireframe for new homepage layout",
  "ProjectId": "<project-guid>",
  "TaskCategoryId": "<category-guid>",
  "TaskTypeId": "<type-guid>"
}'

# Update task
absuite projects update task --TenantId $TENANT_ID --ProjectTaskId <task-guid> --ProjectTaskUpdateDto '{...}'

# Delete task
absuite projects delete task --TenantId $TENANT_ID --ProjectTaskId <task-guid>
```

## Task Categories

```bash
# List (tenant-wide)
absuite projects list tenant-task-categories --TenantId $TENANT_ID
absuite projects count tenant-task-categories --TenantId $TENANT_ID

# List (project-specific)
absuite projects list task-categories --TenantId $TENANT_ID --ProjectId <project-guid>
absuite projects count task-categories --TenantId $TENANT_ID --ProjectId <project-guid>

# Get by ID
absuite projects get task-category-by-id --TenantId $TENANT_ID --TaskCategoryId <category-guid>

# Create
absuite projects create task-category --TenantId $TENANT_ID --TaskCategoryCreateDto '{
  "Name": "Design",
  "Description": "Design and UX tasks"
}'

# Update
absuite projects update task-category --TenantId $TENANT_ID --TaskCategoryId <category-guid> --TaskCategoryUpdateDto '{...}'

# Delete
absuite projects delete task-category --TenantId $TENANT_ID --TaskCategoryId <category-guid>
```

## Task Types

```bash
# Get by ID
absuite projects get task-type-by-id --TenantId $TENANT_ID --TaskTypeId <type-guid>

# List types for a category
absuite projects list task-category-task-types --TenantId $TENANT_ID --TaskCategoryId <category-guid>

# Create
absuite projects create task-type --TenantId $TENANT_ID --TaskTypeCreateDto '{
  "Name": "Bug Fix"
}'

# Update
absuite projects update task-type --TenantId $TENANT_ID --TaskTypeId <type-guid> --TaskTypeUpdateDto '{...}'

# Delete
absuite projects delete task-type --TenantId $TENANT_ID --TaskTypeId <type-guid>
```

## Project Periods

```bash
# List
absuite projects list periods --TenantId $TENANT_ID --ProjectId <project-guid>

# Create
absuite projects create period --TenantId $TENANT_ID --ProjectPeriodCreateDto '{
  "Name": "Sprint 1",
  "StartDate": "2026-04-01T00:00:00Z",
  "EndDate": "2026-04-14T00:00:00Z"
}'

# Update
absuite projects update period --TenantId $TENANT_ID --ProjectPeriodId <period-guid> --ProjectPeriodUpdateDto '{...}'

# Delete
absuite projects delete period --TenantId $TENANT_ID --ProjectPeriodId <period-guid>
```

## Time Logs

```bash
# List time logs for a project
absuite projects list time-logs --TenantId $TENANT_ID --ProjectId <project-guid>

# Count
absuite projects count time-logs --TenantId $TENANT_ID --ProjectId <project-guid>
```

For detailed time tracking operations, see the `absuite-timetracker` skill.

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List projects | `absuite projects get projects-by-tenant-id --TenantId <guid>` |
| Create project | `absuite projects create --TenantId <guid> --ProjectCreateDto '{...}'` |
| List tasks | `absuite projects list tasks --TenantId <guid> --ProjectId <guid>` |
| Create task | `absuite projects create task --TenantId <guid> --ProjectTaskCreateDto '{...}'` |
| List categories | `absuite projects list tenant-task-categories --TenantId <guid>` |
| List periods | `absuite projects list periods --TenantId <guid> --ProjectId <guid>` |
| List time logs | `absuite projects list time-logs --TenantId <guid> --ProjectId <guid>` |

## Critical Rules

- **Authenticate first.** Always provide a tenant context.
- **Create task categories and types first** before referencing them in tasks.
- **Use the `timetracker` service** for detailed time log CRUD and approval workflows.
