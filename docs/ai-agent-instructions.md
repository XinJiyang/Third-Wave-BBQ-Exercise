# AI Agent Instructions

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
