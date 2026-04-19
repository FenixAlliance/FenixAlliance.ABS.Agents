---
name: absuite-storage
description: >
  Manage file storage, uploads, downloads, and avatars in the Alliance Business Suite
  (ABS) using the `absuite` CLI. Covers single/multiple file uploads, blob storage,
  and avatar management for contacts, users, and tenants. Requires an authenticated
  CLI session.
---

# Alliance Business Suite — Storage Skill

Manage file storage through the `absuite` CLI's `storage` service.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite storage list-commands`

## File Operations

### Upload a Single File

```bash
absuite storage single --File @/path/to/file.pdf
```

### Upload Multiple Files

```bash
absuite storage multiple --Files @/path/to/file1.pdf --Files @/path/to/file2.png
```

### Upload an Image

```bash
absuite storage image --File @/path/to/image.jpg
```

### Upload a Specific File

```bash
absuite storage specific --File @/path/to/file.docx
```

### Save (Upload) a File

```bash
absuite storage save-file --File @/path/to/file.xlsx
```

### Create File Record

```bash
absuite storage create file --FileCreateDto '{...}'
```

### Get File

```bash
absuite storage get file --FileId <file-guid>
```

### Update File

```bash
absuite storage update file --FileId <file-guid> --FileUpdateDto '{...}'
```

### Delete File

```bash
absuite storage delete file --FileId <file-guid>
```

### List Files

```bash
absuite storage list files
```

### Download a File

```bash
absuite storage download-file --FileId <file-guid>
```

## Blob Storage

```bash
# List blobs
absuite storage list blobs

# Get blob by ID
absuite storage get blob --BlobId <blob-guid>

# Submit files by ID
absuite storage submit- --FileIds <file-guid>
```

## Avatars

### Current User Avatar

```bash
# Get
absuite storage get current-user-avatar

# Update
absuite storage update user-avatar --Avatar @/path/to/avatar.png
```

### User Avatar (by User ID)

```bash
absuite storage get user-avatar --UserId <user-guid>
```

### Contact Avatar

```bash
# Get
absuite storage get contact-avatar --ContactId <contact-guid>

# Update
absuite storage update contact-avatar --ContactId <contact-guid> --Avatar @/path/to/avatar.png
```

### Tenant Avatar

```bash
# Get
absuite storage get tenant-avatar --TenantId $TENANT_ID

# Update
absuite storage update tenant-avatar --TenantId $TENANT_ID --Avatar @/path/to/logo.png
```

### Social Profile Avatar

```bash
absuite storage get avatar --SocialProfileId <profile-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Upload file | `absuite storage single --File @/path/to/file` |
| Upload image | `absuite storage image --File @/path/to/image.jpg` |
| Upload multiple | `absuite storage multiple --Files @/path/to/a --Files @/path/to/b` |
| Download file | `absuite storage download-file --FileId <guid>` |
| List files | `absuite storage list files` |
| Get user avatar | `absuite storage get current-user-avatar` |
| Update user avatar | `absuite storage update user-avatar --Avatar @/path/to/avatar.png` |
| Get contact avatar | `absuite storage get contact-avatar --ContactId <guid>` |
| Get tenant avatar | `absuite storage get tenant-avatar --TenantId <guid>` |

## Critical Rules

- **Authenticate first.**
- **File paths use `@` prefix** for file uploads (e.g., `@/path/to/file.pdf`).
- **Use `--help`** on any command for full parameter details.
