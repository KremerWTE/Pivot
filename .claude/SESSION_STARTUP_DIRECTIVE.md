# Session Startup Directive — Pivot

**Purpose:** Startup protocol for every new session on this repository
**Status:** MANDATORY — execute at the start of every session
**Version:** 1.0

---

## Session Startup Protocol

### Step 1: Verify Project Context and Branch Safety

```bash
pwd
git branch --show-current
git status
```

**Expected:**
- Working directory: `C:\Users\Chris Kremer\Documents\GitHub\Pivot`
- Active dev branch: `Kremer-dev` (or feature branch)

**Protected Branches (NO DIRECT COMMITS):**
- `main` or `master`
- Any branch containing: `beta`, `stage`, `production`, `prod`, `deploy`, `release`

**If on protected branch, STOP and warn the user before proceeding.**

---

### Step 2: Sync with Remote

```bash
git fetch origin
CURRENT_BRANCH=$(git branch --show-current)
git pull origin $CURRENT_BRANCH
```

---

### Step 3: Load Project Context

Read in order:

1. **Latest session file** in `sessions/recent/` (most recent `YYYY-MM-DD_*.md`)
2. **`MASTER_TODO.md`** — overall completion and critical items
3. **`CLAUDE.md`** — project-specific rules and constraints

---

### Step 4: Present Session Startup Summary

```
# Session Startup — Pivot

**Branch:** [current branch]
**Branch Status:** [Safe / Protected - needs confirmation]
**Last Session:** [date from sessions/recent/ or "No previous session found"]

## Context Loaded
- Latest session summary reviewed
- MASTER_TODO checked
- CLAUDE.md reviewed

## Current State
- Project completion: [% from MASTER_TODO.md]
- Critical tasks: [top 2-3 items]
- Recent accomplishments: [from last session]

## Recommended Next Steps
1. [Highest priority from TODO or last session]
2. [Second priority]
3. [Third priority]

What would you like to focus on this session?
```

---

## Project Overview

**Type:** [TBD — update as project takes shape]
**Active dev branch:** `Kremer-dev`
**Default branch:** `main`

**Key directories:**
- `src/` — application source code
- `data/` — data files and imports
- `docs/` — project documentation
- `scripts/` — utility and automation scripts
- `sessions/` — session notes
- `.github/` — GitHub Actions workflows and templates

**Key files:**
- `CLAUDE.md` — project rules
- `MASTER_TODO.md` — task tracking
- `NEXT_STEPS.md` — immediate priorities

---

## Critical Rules

### Git Workflow

**ALLOWED:**
- Commit to `Kremer-dev` or feature branches
- Create PRs to `main` from `Kremer-dev`

**FORBIDDEN (without explicit user confirmation):**
- Direct commits to `main` or `master`
- Force push to any branch
- Delete remote branches

### Commit Standards

**DO:**
- Use conventional commit format: `feat/fix/docs/test/chore`
- Keep commits focused and atomic

**NEVER:**
- Add "Co-Authored-By: Claude" or any AI attribution
- Commit secrets or credentials

### Security-Sensitive Files
- `appsettings.json` — may contain credentials
- `.env` files — environment variables / secrets
- Never commit actual secrets

---

## Session End Protocol

After significant work:

1. **Update** `MASTER_TODO.md` if tasks changed or completion % shifted
2. **Update** `NEXT_STEPS.md` with immediate priorities for next session
3. **Write** session summary: `sessions/recent/YYYY-MM-DD_brief-description.md`
4. **Commit** staged changes with conventional commit message
5. **Push** all commits to `Kremer-dev`

---

## Quick Verification Checklist

- [ ] Current branch identified and safety checked
- [ ] Synced with remote (git pull completed)
- [ ] Latest session summary reviewed
- [ ] MASTER_TODO and CLAUDE.md checked
- [ ] Session startup summary presented to user

---

**Created:** 2026-02-27
**Version:** 1.0
**Project:** Pivot
**Repo:** `C:\Users\Chris Kremer\Documents\GitHub\Pivot`
