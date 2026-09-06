# Third Wave BBQ - AI Development Exercise

Response to the Third Wave BBQ Graduate Software Developer, AI & Automation exercise.

## Folder Structure

```text
Third Wave BBQ/
+-- README.md
+-- deploy/
|   +-- .gitkeep
+-- docs/
|   +-- .gitkeep
|   +-- exercise-brief.md
+-- prompts/
|   +-- .gitkeep
+-- src/
|   +-- .gitkeep
+-- tests/
    +-- .gitkeep
```

## 1. Requirements

Questions before development:

1. What user roles exist, and how is Head Office access represented?
2. Does each issue belong to exactly one venue?
3. What statuses and priority values are required?
4. Who can assign, edit due dates, close issues or delete photos?
5. Should assignees be limited to the same venue, or can Head Office assign across venues?
6. What file size, type, storage and retention rules apply to photographs?
7. Who receives overdue reminders, and at what time/time zone?
8. Are audit trails or reports required?

Assumptions: an issue belongs to one venue; venue employees only access their own venue; Head Office can access all venues; photos are private; reminders run daily; destructive changes are restricted or audited.

## 2. Development Plan

Database: add `issues`, `issue_comments` and `issue_photos`. `issues` should include venue, creator, assignee, title, description, priority, status, due date and timestamps. Add indexes for venue, assignee, status and due date.

Backend/API: add a NestJS issues module with DTO validation and endpoints for list, create, detail, update, assign, status change, comments and photo upload. Keep business rules in services so permission checks are reused.

Frontend: add Next.js screens for issue list, filters, create/edit form, detail view, comments and photographs. Reuse existing components and start with the core workflow: create, view, update status.

Permissions: enforce venue isolation in the backend. Client-supplied `venueId` can filter Head Office views but must not expand a venue user's access.

Photographs: prefer object storage or a private mounted directory, with metadata in PostgreSQL. Validate file type and size, and serve through authorised endpoints.

Email reminders: add a scheduled job that finds overdue unresolved issues and sends daily reminders. Make it idempotent and logged.

Testing: cover permissions, CRUD, assignment, comments, uploads and reminder selection. Add manual cross-venue tests.

Deployment: use reviewed branches, database backups, migrations, environment variables, Docker Compose rollout, logs, health checks and rollback steps.

## 3. Instructions to the AI Coding Agent

First prompt:

```text
Inspect the existing Next.js, NestJS, PostgreSQL and Docker Compose repository before editing. Identify the auth model, user/venue schema, API patterns, validation style, migration tooling, tests, upload handling, scheduler/email setup and Docker configuration.

Implement a maintenance issues feature in small reviewable changes. Add database tables for issues, comments and photographs using the existing migration style. Add NestJS APIs for list, create, detail, update, assign, status change, comments and photo upload. Enforce venue-level access on every backend query: venue employees can only access their own venue; Head Office can access all venues. Do not rely on frontend filtering for security.

Add Next.js UI for list, create form, detail page, comments, priority, assignee, due date, status and photographs. Reuse existing project components and styling. Add daily overdue email reminders using the existing scheduler/email pattern if available.

Before completion, run formatting, linting, unit/API/frontend tests and build checks. Add tests proving venue users cannot read, update, comment on or upload photos to another venue's issue. Avoid new dependencies unless clearly justified.
```

Follow-up prompt:

```text
Trace every issue, comment and photograph query from controller to database. Add regression tests showing that a client-provided venueId cannot override the authenticated user's allowed venue scope.
```

## 4. Reviewing AI-Generated Work

I would inspect migrations, models/entities, DTOs, controllers, services, guards, frontend pages/components, upload code, scheduler code, Docker changes and environment examples. I would check whether the code follows existing patterns instead of introducing duplicate architecture.

Checks I would run:

```bash
npm install
npm run lint
npm run test
npm run test:e2e
npm run build
docker compose build
docker compose up -d
```

Manual testing: create issues as a venue manager, view as same venue, attempt access as another venue, verify Head Office access, comments, uploads, status changes and overdue reminders.

Security checks: backend authorization, direct issue ID access, client-supplied `venueId`, private photo access, upload validation and secrets. To spot suspicious dependencies, inspect `package.json`, lockfile diffs, import usage, package reputation and whether existing libraries already cover the need.

## 5. Code Review

The endpoint trusts `req.query.venueId`. A venue user could request another venue's ID, or omit the parameter and receive all issues. The most serious problem is broken authorization causing cross-venue data leakage.

Fix: derive scope from the authenticated user. Venue users must always be filtered to their own venue. Head Office users may view all venues or filter by venue. Put this rule in the backend service and reuse it for list, detail, update, comments and uploads.

Test with two venues. A venue A user should only receive venue A issues even when passing venue B's ID. Direct access to venue B issue IDs should return 403 or 404. Head Office should see both.

## 6. Deployment

Use a feature branch, pull request and code review. Before release, back up PostgreSQL and confirm the restore process. Review migrations and run them in a controlled window.

Add email/photo storage secrets on the VPS, not in Git. Deploy with Docker Compose by building or pulling the new image, running migrations, restarting services and checking health. Inspect app, proxy, migration and scheduler logs. Roll back using the previous commit/image and, if needed, a tested down migration or database backup.

## 7. Production Problem

I would prioritise the cross-venue visibility issue because it is a data privacy incident. The HTTP 500s are also important, but data leakage is the immediate risk.

First, I would consider disabling the feature or restricting it to Head Office. Then I would inspect app logs, proxy logs, recent deployment changes, migration output, auth/session data and affected users. I would reproduce with users from different venues and compare UI requests with direct API calls.

I would determine whether the cause is backend authorization, bad venue data, cached claims, migration error or deployment configuration. Before releasing a fix, I would add a regression test for the exact leak, review all issue-related queries, run the full test/build pipeline and verify in staging or a production-like environment.

## 8. Previous Project

TODO: Add one real software project or automation.

- What the project did: TODO
- My responsibilities: TODO
- AI tools used: TODO
- Hosting/deployment: TODO
- What AI initially got wrong: TODO
- How I identified and corrected it: TODO
- Project/GitHub link: TODO

## AI Usage Disclosure

AI tools used: ChatGPT/Codex.

Relevant prompt:

```text
Read the Third Wave BBQ exercise requirements. Create a basic project folder structure and a README that answers the requested questions. If a question needs personal project information or final implementation details that are not available yet, leave a TODO placeholder.
```

AI recommendations changed or rejected: I did not build the full feature because the brief asks for reasoning, not implementation. I kept the repository minimal so the submission stays focused on the written response.
