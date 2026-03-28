# Epic Story Fields Template

This shows the new fields added to each story in epic files.

## Before (existing format)

```markdown
### Story 1.2: User Login

As a returning user,
I want to log in with my credentials,
So that I can access my account.

Acceptance Criteria:
- Given: User is on login page
- When: They enter valid credentials
- Then: They are logged in and redirected to dashboard
```

## After (with new fields)

```markdown
### Story 1.2: User Login
key: 1-2-user-login
points: 5
status: done
jira_key: null

As a returning user,
I want to log in with my credentials,
So that I can access my account.

Acceptance Criteria:
- Given: User is on login page
- When: They enter valid credentials
- Then: They are logged in and redirected to dashboard
```

## Field Definitions

| Field | Type | Description |
|-------|------|-------------|
| `key` | string | Story identifier: `{epic}-{story}-{slug}` |
| `points` | number | Story point estimate (1, 2, 3, 5, 8, 13, 21) |
| `status` | enum | `done` (only done stories remain in epics.md after migration) |
| `jira_key` | string | Optional Jira issue key (e.g., PROJ-123) |

## Story Summary Table (added to each epic section)

Only done stories appear in the table after migration. Undone stories are removed and their files archived.

```markdown
**Story Summary:**
| Story | Title | Points | Status |
|-------|-------|--------|--------|
| 1.1 | User Registration | 5 | done |
| 1.2 | User Login | 5 | done |
| 1.3 | Password Reset | 3 | done |
| **Total** | | **13** | |
```
