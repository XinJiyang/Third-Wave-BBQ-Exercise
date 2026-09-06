# Requirements and Development Plan

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
