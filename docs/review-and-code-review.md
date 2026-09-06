# Review and Code Review

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
