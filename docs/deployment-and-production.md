# Deployment and Production

## 6. Deployment

Use a feature branch, pull request and code review. Before release, back up PostgreSQL and confirm the restore process. Review migrations and run them in a controlled window.

Add email/photo storage secrets on the VPS, not in Git. Deploy with Docker Compose by building or pulling the new image, running migrations, restarting services and checking health. Inspect app, proxy, migration and scheduler logs. Roll back using the previous commit/image and, if needed, a tested down migration or database backup.

## 7. Production Problem

I would prioritise the cross-venue visibility issue because it is a data privacy incident. The HTTP 500s are also important, but data leakage is the immediate risk.

First, I would consider disabling the feature or restricting it to Head Office. Then I would inspect app logs, proxy logs, recent deployment changes, migration output, auth/session data and affected users. I would reproduce with users from different venues and compare UI requests with direct API calls.

I would determine whether the cause is backend authorization, bad venue data, cached claims, migration error or deployment configuration. Before releasing a fix, I would add a regression test for the exact leak, review all issue-related queries, run the full test/build pipeline and verify in staging or a production-like environment.
