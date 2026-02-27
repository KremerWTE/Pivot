# Claude Code Directives — Pivot

This directory contains directives and templates for Claude Code sessions on this project.

---

## Start Here

### For Every Session:

1. **Read the startup directive first:**
   - **Location:** `.claude/SESSION_STARTUP_DIRECTIVE.md`
   - **Purpose:** Startup protocol, branch safety, and session workflow

2. **Follow the protocol:**
   - Check branch (must be `Kremer-dev` or a feature branch)
   - Sync with remote
   - Load project context
   - Present startup summary to user

---

## Available Directives

| File | Purpose |
|------|---------|
| `SESSION_STARTUP_DIRECTIVE.md` | **Start here** — startup protocol |
| `SESSION_SUMMARY_TEMPLATE.md` | End-of-session documentation template |
| `git-workflow-directive.md` | Git safety rules and commit standards |
| `FILE_ORGANIZATION_DIRECTIVE.md` | Where files belong |

---

## Critical Rules Summary

### Git Safety
- **Active dev branch:** `Kremer-dev`
- **Protected:** `main`, `master`, and any branch with `prod/stage/deploy`
- **Never** force push or commit to protected branches
- **Never** add AI co-author attribution

### Session End
1. Update `MASTER_TODO.md` and `NEXT_STEPS.md`
2. Write session summary to `sessions/recent/YYYY-MM-DD_description.md`
3. Commit and push all changes
4. Verify `git status` is clean

---

**Version:** v1.0
**Last Updated:** 2026-02-27
