---
name: absuite-content
description: >
  Manage web content, web pages, web portals, web templates, blog posts, blog
  categories, blog tags, blog comments, blog authors, and business domains in the
  Alliance Business Suite (ABS) using the `absuite` CLI. This is the comprehensive
  content management skill — for blog-only operations see the `absuite-blog` skill.
  Requires an authenticated CLI session.
---

# Alliance Business Suite — Content Skill

Manage all content through the `absuite` CLI's `content` service. Covers web portals, web pages, web templates, web content blocks, and the full blog system.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite content list-commands`

## Web Portals

### List / Get Portals

```bash
# Get current portal
absuite content get current-web-portal --TenantId $TENANT_ID

# Get root portal
absuite content get root-web-portal

# Get portal by ID
absuite content get web-portal-by-id --TenantId $TENANT_ID --WebPortalId <portal-guid>

# Search portal by domain
absuite content search-web-portal --Domain example.com
```

### Create Portal

```bash
absuite content create web-portal --TenantId $TENANT_ID --WebPortalCreateDto '{
  "Title": "Main Website",
  "Domain": "www.example.com",
  "Description": "Company main website",
  "BusinessID": "<tenant-guid>"
}'
```

### Update / Patch Portal

```bash
absuite content update web-portal --TenantId $TENANT_ID --WebPortalId <portal-guid> --WebPortalUpdateDto '{
  "Title": "Updated Website Title"
}'

absuite content patch web-portal --TenantId $TENANT_ID --WebPortalId <portal-guid> --Body '[
  {"op": "replace", "path": "/Title", "value": "New Title"}
]'
```

### Delete Portal

```bash
absuite content delete web-portal --TenantId $TENANT_ID --WebPortalId <portal-guid>
```

### Portal Options & Settings

```bash
absuite content list current-web-portal-options --TenantId $TENANT_ID
absuite content list web-portal-options --TenantId $TENANT_ID --WebPortalId <portal-guid>
absuite content list web-portal-settings --TenantId $TENANT_ID --WebPortalId <portal-guid>
```

### Initialize Portal

```bash
absuite content initialize-current-web-portal --TenantId $TENANT_ID
```

## Business Domains

```bash
absuite content list business-domains --TenantId $TENANT_ID
absuite content count business-domains --TenantId $TENANT_ID
absuite content get business-domain-by-id --TenantId $TENANT_ID --BusinessDomainId <domain-guid>
```

## Web Pages

```bash
# List / Count
absuite content list web-pages --TenantId $TENANT_ID
absuite content count web-pages --TenantId $TENANT_ID

# Get by ID
absuite content get web-page-by-id --TenantId $TENANT_ID --WebPageId <page-guid>

# Create
absuite content create web-page --TenantId $TENANT_ID --WebPageCreateDto '{
  "Title": "About Us",
  "Description": "Company about page"
}'

# Update
absuite content update web-page --TenantId $TENANT_ID --WebPageId <page-guid> --WebPageUpdateDto '{...}'

# Delete
absuite content delete web-page --TenantId $TENANT_ID --WebPageId <page-guid>
```

### Web Page Categories

```bash
absuite content list web-page-categories --TenantId $TENANT_ID
absuite content get web-page-category-by-id --TenantId $TENANT_ID --WebPageCategoryId <category-guid>
absuite content create web-page-category --TenantId $TENANT_ID --WebPageCategoryCreateDto '{...}'
absuite content update web-page-category --TenantId $TENANT_ID --WebPageCategoryId <category-guid> --WebPageCategoryUpdateDto '{...}'
absuite content delete web-page-category --TenantId $TENANT_ID --WebPageCategoryId <category-guid>

# Relate / unrelate
absuite content relate-web-page-to-category --TenantId $TENANT_ID --WebPageId <page-guid> --WebPageCategoryId <category-guid>
absuite content unrelate-web-page-category --TenantId $TENANT_ID --WebPageId <page-guid> --WebPageCategoryId <category-guid>
absuite content get categories-by-web-page --TenantId $TENANT_ID --WebPageId <page-guid>
```

### Web Page Tags

```bash
absuite content list web-page-tags --TenantId $TENANT_ID
absuite content get web-page-tag-by-id --TenantId $TENANT_ID --WebPageTagId <tag-guid>
absuite content create web-page-tag --TenantId $TENANT_ID --WebPageTagCreateDto '{...}'
absuite content update web-page-tag --TenantId $TENANT_ID --WebPageTagId <tag-guid> --WebPageTagUpdateDto '{...}'
absuite content delete web-page-tag --TenantId $TENANT_ID --WebPageTagId <tag-guid>

# Relate / unrelate
absuite content relate-web-page-to-tag --TenantId $TENANT_ID --WebPageId <page-guid> --WebPageTagId <tag-guid>
absuite content unrelate-web-page-tag --TenantId $TENANT_ID --WebPageId <page-guid> --WebPageTagId <tag-guid>
absuite content get tags-by-web-page --TenantId $TENANT_ID --WebPageId <page-guid>
```

## Web Templates

```bash
absuite content list web-templates --TenantId $TENANT_ID
absuite content count web-templates --TenantId $TENANT_ID
absuite content get web-template-by-id --TenantId $TENANT_ID --WebTemplateId <template-guid>
absuite content create web-template --TenantId $TENANT_ID --WebTemplateCreateDto '{...}'
absuite content update web-template --TenantId $TENANT_ID --WebTemplateId <template-guid> --WebTemplateUpdateDto '{...}'
absuite content delete web-template --TenantId $TENANT_ID --WebTemplateId <template-guid>
```

## Web Content Blocks

```bash
absuite content list web-contents --TenantId $TENANT_ID
absuite content count web-contents --TenantId $TENANT_ID
absuite content get web-content-by-id --TenantId $TENANT_ID --WebContentId <content-guid>
absuite content create web --TenantId $TENANT_ID --WebContentCreateDto '{...}'
absuite content update web --TenantId $TENANT_ID --WebContentId <content-guid> --WebContentUpdateDto '{...}'
absuite content delete web --TenantId $TENANT_ID --WebContentId <content-guid>
```

## Blog Posts

```bash
# List / Count
absuite content list blog-posts --TenantId $TENANT_ID
absuite content count blog-posts --TenantId $TENANT_ID

# Get by ID
absuite content get blog-post-by-id --TenantId $TENANT_ID --BlogPostId <post-guid>

# Create
absuite content create blog-post --TenantId $TENANT_ID --BlogPostCreateDto '{
  "Title": "Getting Started with ABS",
  "Excerpt": "A quick guide to the Alliance Business Suite",
  "Code": "# Welcome\n\nThis is your first post.",
  "Slug": "getting-started-with-abs"
}'

# Update
absuite content update blog-post --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostUpdateDto '{...}'

# Delete
absuite content delete blog-post --TenantId $TENANT_ID --BlogPostId <post-guid>
```

### Blog Authors

```bash
absuite content list blog-authors --TenantId $TENANT_ID
absuite content get blog-author-by-id --TenantId $TENANT_ID --BlogAuthorId <author-guid>
absuite content count blog-posts-by-author --TenantId $TENANT_ID --BlogAuthorId <author-guid>
absuite content get blog-posts-by-author --TenantId $TENANT_ID --BlogAuthorId <author-guid>
```

### Blog Categories

```bash
absuite content list blog-post-categories --TenantId $TENANT_ID
absuite content count blog-post-categories --TenantId $TENANT_ID
absuite content get blog-post-category-by-id --TenantId $TENANT_ID --BlogPostCategoryId <category-guid>
absuite content create blog-post-category --TenantId $TENANT_ID --BlogPostCategoryCreateDto '{...}'
absuite content update blog-post-category --TenantId $TENANT_ID --BlogPostCategoryId <category-guid> --BlogPostCategoryUpdateDto '{...}'
absuite content delete blog-post-category --TenantId $TENANT_ID --BlogPostCategoryId <category-guid>

# Relate/unrelate category to a blog post
absuite content create category-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostCategoryCreateDto '{...}'
absuite content post relate-category-to-blog --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostCategoryId <category-guid>
absuite content post unrelate-category-from-blog --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostCategoryId <category-guid>
absuite content get categories-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid>
```

### Blog Tags

```bash
absuite content list blog-post-tags --TenantId $TENANT_ID
absuite content get blog-post-tag-by-id --TenantId $TENANT_ID --BlogPostTagId <tag-guid>
absuite content create blog-post-tag --TenantId $TENANT_ID --BlogPostTagCreateDto '{...}'
absuite content update blog-post-tag --TenantId $TENANT_ID --BlogPostTagId <tag-guid> --BlogPostTagUpdateDto '{...}'
absuite content delete blog-post-tag --TenantId $TENANT_ID --BlogPostTagId <tag-guid>

# Relate/unrelate tag to a blog post
absuite content create tag-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostTagCreateDto '{...}'
absuite content post relate-tag-to-blog --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostTagId <tag-guid>
absuite content post unrelate-tag-from-blog --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostTagId <tag-guid>
absuite content get tags-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid>
```

### Blog Comments

```bash
absuite content create comment-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid> --BlogPostCommentCreateDto '{...}'
absuite content get comments-for-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid>
absuite content reply-to-comment --TenantId $TENANT_ID --BlogPostId <post-guid> --CommentId <comment-guid> --BlogPostCommentCreateDto '{...}'
absuite content get replies-for-comment --TenantId $TENANT_ID --CommentId <comment-guid>
absuite content delete comment-from-blog-post --TenantId $TENANT_ID --BlogPostId <post-guid> --CommentId <comment-guid>
```

### Currency Rates (Utility)

```bash
absuite content get latest-currency-rates-model --TenantId $TENANT_ID
```

## Critical Rules

- **Authenticate first.**
- **Always provide a tenant context.**
- **Blog content goes in the `Code` field** as Markdown.
- **Use `post relate-*` / `post unrelate-*`** to link/unlink existing categories and tags to blog posts.
- **Use `create category-for-blog-post` / `create tag-for-blog-post`** to create AND link in one step.
- **Use `--help`** on any command for full DTO schemas.
