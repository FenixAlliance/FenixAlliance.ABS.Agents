---
name: absuite-learning
description: >
  Manage learning and e-learning in the Alliance Business Suite (ABS) using the
  `absuite` CLI. Covers courses, sections, units, unit components, assignments,
  problem sets, cohorts, enrollments, certificates, forums, wikis, articles, handouts,
  files, libraries, pages, updates, and instructor/student profiles. Requires an
  authenticated CLI session.
---

# Alliance Business Suite — Learning Skill

Manage e-learning through the `absuite` CLI's `learning` service. All operations are tenant-scoped and require authentication.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Set your tenant**: `absuite config set --tenant-id <tenant-guid>` or pass `--TenantId` on each call.
3. **Discover commands**: `absuite learning list-commands`

## Key Concepts

- **Course** — a top-level learning entity with sections, units, and content.
- **Section** — a grouping within a course (e.g., "Week 1", "Module 2").
- **Unit** — a lesson within a section.
- **Unit Component** — a content block within a unit (video, text, quiz).
- **Cohort** — a group of students taking a course together.
- **Enrollment** — a student's registration in a course.
- **Assignment** — graded work for students.
- **Problem Set** — practice exercises.
- **Certificate** — completion credential.

## Courses

```bash
# List
absuite learning list courses --TenantId $TENANT_ID

# Count
absuite learning count courses --TenantId $TENANT_ID

# Get by ID
absuite learning get course-by-id --TenantId $TENANT_ID --CourseId <course-guid>

# Create
absuite learning create course --TenantId $TENANT_ID --CourseCreateDto '{
  "Title": "Introduction to Cloud Computing"
}'

# Update
absuite learning update course --TenantId $TENANT_ID --CourseId <course-guid> --CourseUpdateDto '{...}'

# Delete
absuite learning delete course --TenantId $TENANT_ID --CourseId <course-guid>
```

## Course Structure: Sections → Units → Components

### Sections

```bash
absuite learning list course-sections --TenantId $TENANT_ID
absuite learning count course-sections --TenantId $TENANT_ID
absuite learning get course-sections-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning get course-section-by-id --TenantId $TENANT_ID --CourseSectionId <section-guid>
absuite learning create course-section --TenantId $TENANT_ID --CourseSectionCreateDto '{"Title": "Week 1: Fundamentals"}'
absuite learning update course-section --TenantId $TENANT_ID --CourseSectionId <section-guid> --CourseSectionUpdateDto '{...}'
absuite learning delete course-section --TenantId $TENANT_ID --CourseSectionId <section-guid>
```

### Units

```bash
absuite learning list course-units --TenantId $TENANT_ID
absuite learning count course-units --TenantId $TENANT_ID
absuite learning get course-units-by-section --TenantId $TENANT_ID --CourseSectionId <section-guid>
absuite learning count course-units-by-section --TenantId $TENANT_ID --CourseSectionId <section-guid>
absuite learning get course-unit-by-id --TenantId $TENANT_ID --CourseUnitId <unit-guid>
absuite learning create course-unit --TenantId $TENANT_ID --CourseUnitCreateDto '{"Title": "Introduction to AWS"}'
absuite learning update course-unit --TenantId $TENANT_ID --CourseUnitId <unit-guid> --CourseUnitUpdateDto '{...}'
absuite learning delete course-unit --TenantId $TENANT_ID --CourseUnitId <unit-guid>
```

### Unit Components

```bash
absuite learning list course-unit-components --TenantId $TENANT_ID
absuite learning count course-unit-components --TenantId $TENANT_ID
absuite learning get course-unit-components-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning count course-unit-components-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning get course-unit-component-by-id --TenantId $TENANT_ID --CourseUnitComponentId <component-guid>
absuite learning create course-unit-component --TenantId $TENANT_ID --CourseUnitComponentCreateDto '{...}'
absuite learning update course-unit-component --TenantId $TENANT_ID --CourseUnitComponentId <component-guid> --CourseUnitComponentUpdateDto '{...}'
absuite learning delete course-unit-component --TenantId $TENANT_ID --CourseUnitComponentId <component-guid>
```

## Enrollments

```bash
absuite learning list enrollments --TenantId $TENANT_ID
absuite learning count enrollments --TenantId $TENANT_ID
absuite learning get course-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>
absuite learning get course-enrollments-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-enrollment --TenantId $TENANT_ID --CourseEnrollmentCreateDto '{...}'
absuite learning update course-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid> --CourseEnrollmentUpdateDto '{...}'
absuite learning delete course-enrollment --TenantId $TENANT_ID --EnrollmentId <enrollment-guid>

# Student-specific enrollments
absuite learning list student-course-enrollments --TenantId $TENANT_ID --StudentProfileId <student-guid>
```

## Cohorts

```bash
absuite learning list course-cohorts --TenantId $TENANT_ID
absuite learning count course-cohorts --TenantId $TENANT_ID
absuite learning get course-cohorts-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning get course-cohort-by-id --TenantId $TENANT_ID --CourseCohortId <cohort-guid>
absuite learning create course-cohort --TenantId $TENANT_ID --CourseCohortCreateDto '{...}'
absuite learning update course-cohort --TenantId $TENANT_ID --CourseCohortId <cohort-guid> --CourseCohortUpdateDto '{...}'
absuite learning delete course-cohort --TenantId $TENANT_ID --CourseCohortId <cohort-guid>
```

## Assignments & Problem Sets

```bash
# Assignments
absuite learning list course-assignments --TenantId $TENANT_ID
absuite learning count course-assignments --TenantId $TENANT_ID
absuite learning get course-assignments-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning get course-assignment-by-id --TenantId $TENANT_ID --CourseAssignmentId <assignment-guid>
absuite learning create course-assignment --TenantId $TENANT_ID --CourseAssignmentCreateDto '{...}'
absuite learning update course-assignment --TenantId $TENANT_ID --CourseAssignmentId <assignment-guid> --CourseAssignmentUpdateDto '{...}'
absuite learning delete course-assignment --TenantId $TENANT_ID --CourseAssignmentId <assignment-guid>

# Problem Sets
absuite learning list course-problem-sets --TenantId $TENANT_ID
absuite learning count course-problem-sets --TenantId $TENANT_ID
absuite learning get course-problem-sets-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning get course-problem-set-by-id --TenantId $TENANT_ID --CourseProblemSetId <problemset-guid>
absuite learning create course-problem-set --TenantId $TENANT_ID --CourseProblemSetCreateDto '{...}'
absuite learning update course-problem-set --TenantId $TENANT_ID --CourseProblemSetId <problemset-guid> --CourseProblemSetUpdateDto '{...}'
absuite learning delete course-problem-set --TenantId $TENANT_ID --CourseProblemSetId <problemset-guid>
```

## Certificates & Templates

```bash
absuite learning list course-certificates --TenantId $TENANT_ID
absuite learning count course-certificates --TenantId $TENANT_ID
absuite learning get course-certificate --TenantId $TENANT_ID --CourseCertificateId <cert-guid>
absuite learning create course-certificate --TenantId $TENANT_ID --CourseCertificateCreateDto '{...}'
absuite learning update course-certificate --TenantId $TENANT_ID --CourseCertificateId <cert-guid> --CourseCertificateUpdateDto '{...}'
absuite learning delete course-certificate --TenantId $TENANT_ID --CourseCertificateId <cert-guid>

# Certificate templates
absuite learning list course-certificate-templates --TenantId $TENANT_ID
absuite learning get course-certificate-template --TenantId $TENANT_ID --CertificateTemplateId <template-guid>
absuite learning create course-certificate-template --TenantId $TENANT_ID --CertificateTemplateCreateDto '{...}'
absuite learning delete course-certificate-template --TenantId $TENANT_ID --CertificateTemplateId <template-guid>
```

## Course Content: Categories, Forums, Wikis, Articles, Pages, Updates

```bash
# Categories
absuite learning list course-categories --TenantId $TENANT_ID
absuite learning get course-categories-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-category --TenantId $TENANT_ID --CourseCategoryCreateDto '{...}'

# Forums
absuite learning list course-forums --TenantId $TENANT_ID
absuite learning get course-forums-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-forum --TenantId $TENANT_ID --CourseForumCreateDto '{...}'

# Wikis
absuite learning get course-wikis --TenantId $TENANT_ID
absuite learning get course-wikis-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-wiki --TenantId $TENANT_ID --CourseWikiCreateDto '{...}'

# Articles (within wikis)
absuite learning list course-articles --TenantId $TENANT_ID
absuite learning get course-articles-by-course-wiki --TenantId $TENANT_ID --CourseWikiId <wiki-guid>
absuite learning create course-article --TenantId $TENANT_ID --CourseArticleCreateDto '{...}'

# Pages
absuite learning list course-pages --TenantId $TENANT_ID
absuite learning get course-pages-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-page --TenantId $TENANT_ID --CoursePageCreateDto '{...}'

# Updates / Announcements
absuite learning list course-updates --TenantId $TENANT_ID
absuite learning get course-updates-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-update --TenantId $TENANT_ID --CourseUpdateCreateDto '{...}'
```

## Course Resources: Files, Handouts, Libraries

```bash
# Files
absuite learning list course-files --TenantId $TENANT_ID
absuite learning get course-files-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-file --TenantId $TENANT_ID --CourseFileCreateDto '{...}'

# Handouts
absuite learning list course-handouts --TenantId $TENANT_ID
absuite learning get course-handouts-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-handout --TenantId $TENANT_ID --CourseHandoutCreateDto '{...}'

# Libraries
absuite learning list course-libraries --TenantId $TENANT_ID
absuite learning get course-libraries-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create course-library --TenantId $TENANT_ID --CourseLibraryCreateDto '{...}'
```

## Instructor & Student Profiles

```bash
# Instructors
absuite learning list instructor-profiles --TenantId $TENANT_ID
absuite learning count instructor-profiles --TenantId $TENANT_ID
absuite learning get instructor-profiles-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create instructor-profiles --TenantId $TENANT_ID --InstructorProfileCreateDto '{...}'

# Students
absuite learning list student-profiles --TenantId $TENANT_ID
absuite learning count student-profiles --TenantId $TENANT_ID
absuite learning get student-profiles-by-course --TenantId $TENANT_ID --CourseId <course-guid>
absuite learning create student-profiles --TenantId $TENANT_ID --StudentProfileCreateDto '{...}'
```

## Critical Rules

- **Authenticate first.** Use `absuite login` before any learning operation.
- **Always provide a tenant context.**
- **Course hierarchy**: Course → Section → Unit → Unit Component. Create in order.
- **Use `--help`** on any command for full DTO schemas.
