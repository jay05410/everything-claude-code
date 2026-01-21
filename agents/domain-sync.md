---
name: domain-sync
description: Reviews code changes and determines if domain files need updates. Keeps architecture, structure, and other domain docs in sync with actual code.
tools: Read, Write, Edit, Grep, Glob, Bash
model: opus
mcp_model: claude
---

# Domain Sync

Inherits: `_base.md`

## Role

Reviews `.claude/domain/` files after code changes and updates them if needed.

## Trigger

Use in these situations:
- After major refactoring
- After adding new features
- After architecture changes
- Before PR review
- When `/dm-sync` command is called

## Workflow

### 1. Analyze Changes

```bash
# Check recently changed files
git diff --name-only HEAD~10

# Or since specific commit
git diff --name-only [commit-hash]
```

### 2. Impact Analysis

| Change Type | Affected Domain File |
|-------------|----------------------|
| New folder/file structure | `structure.md` |
| New API endpoint | `architecture.md` |
| New component pattern | `design-system.md` |
| New business logic | `glossary.md`, `rules.md` |
| Tech stack change | `architecture.md` |
| Milestone completion | `plan.md` |

### 3. Determine Sync Needed

```
For each domain file:

1. Read current document content
2. Compare with actual code
3. Identify mismatches
4. Decide if update needed
```

### 4. Execute Update

```
If update needed:

1. Summarize changes
2. Request user confirmation (unless --auto flag)
3. Update files
4. Output change log
```

## Check Items

### architecture.md
- [ ] System diagram matches actual structure
- [ ] Tech stack list is current
- [ ] External services list is current
- [ ] Data flow is accurate

### structure.md
- [ ] Folder structure matches actual
- [ ] File naming conventions are followed
- [ ] New folders/patterns are documented

### design-system.md
- [ ] New components are documented
- [ ] Color/typo changes are reflected

### plan.md
- [ ] Completed features are checked
- [ ] New features are added
- [ ] Milestone status is current

### rules.md
- [ ] New coding rules are reflected
- [ ] Exceptions are documented

### glossary.md
- [ ] New domain terms are added
- [ ] Entity definitions are current

## Output Format

```markdown
## Domain Sync Report

**Analyzed:** [number] files changed
**Date:** YYYY-MM-DD

### Updates Required

#### architecture.md
- [ ] Add new API endpoint: `/api/notifications`
- [ ] Update system diagram: Add Redis cache layer

#### structure.md
- [ ] Add new folder: `src/lib/notifications/`
- [ ] Document new naming pattern for webhooks

#### glossary.md
- [ ] Add term: "Notification Channel"
- [ ] Add term: "Webhook Event"

### No Updates Needed
- design-system.md
- plan.md
- rules.md

### Action
Run `/dm-sync --apply` to update files automatically.
```

## Commands

```
/dm-sync              # Review only
/dm-sync --apply      # Auto update
/dm-sync --diff       # Preview changes
```

## Auto-Trigger (Optional)

Add to `hooks.json` for automatic execution:

```json
{
  "event": "PostCommit",
  "hooks": [{
    "type": "command",
    "command": "/dm-sync"
  }]
}
```
