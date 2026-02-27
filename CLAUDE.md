# Claude Code Directives

**Welcome!** This file directs you to the correct startup directives for this project.

---

## Start Here

### For Claude Code Sessions:

1. **Read the startup directive first:**
   - **Location:** `.claude/SESSION_STARTUP_DIRECTIVE.md`
   - **Purpose:** Startup protocol, mode selection, and session workflow

2. **Choose your startup mode:**
   - **Quick Mode** (most sessions): Read `NEXT_STEPS.md` ONLY
   - **Balanced Mode** (phase transitions): Read condensed directives
   - **Comprehensive Mode** (major planning): Read all directives

3. **Follow the protocol:**
   - The SESSION_STARTUP_DIRECTIVE will guide you through the rest
   - DO NOT read files randomly — follow the directive's instructions

---

## Available Directives

All directives are located in the `.claude/` directory:

- **SESSION_STARTUP_DIRECTIVE.md** — Start here! Startup protocol
- **FILE_ORGANIZATION_DIRECTIVE.md** — Where to put files
- **git-workflow-directive.md** — Git safety rules and commit standards
- **SESSION_SUMMARY_TEMPLATE.md** — End-of-session documentation template

---

## CRITICAL DIRECTIVES — MUST FOLLOW

### Git Safety — MANDATORY at Every Startup

**Branch Safety Check (REQUIRED FIRST STEP):**
```bash
git branch --show-current
```

**Protected Branches — NO COMMITS WITHOUT EXPLICIT USER PERMISSION:**
- `main`, `master`, `dev`, `develop`
- Any branch containing: `beta`, `stage`, `staging`, `production`, `prod`, `deploy`, `release`

**Active dev branch: `Kremer-dev`**

**If on protected branch → IMMEDIATELY warn user and create new branch:**
```bash
git checkout -b feature/[NAME]
# OR switch to
git checkout Kremer-dev
```

**Sync with Remote (REQUIRED):**
```bash
git fetch origin && git pull origin $(git branch --show-current)
```

### Git Prohibitions — NEVER Without User Confirmation

- Force push (`--force`, `-f`)
- Commit to protected branches
- Delete remote branches
- Amend pushed commits
- Add AI co-author attribution ("Co-Authored-By: Claude")
- Commit secrets/credentials

### Git Requirements — ALWAYS Required

- Conventional commit format: `type(scope): description`
  - Types: `feat`, `fix`, `docs`, `refactor`, `perf`, `test`, `chore`, `ci`, `style`
  - Minimum 10 characters
  - Explain WHY, not just WHAT
- Push all commits at session end
- Verify git status before ending session

---

### File Organization — MANDATORY

**Session Notes:**
- Pattern: `sessions/recent/YYYY-MM-DD_description.md`
- Archive: `sessions/archive/YYYY-MM/`

**Documentation:**
- Reports → `docs/reports/`
- Setup → `docs/setup/`
- Guides → `docs/guides/`
- Reference → `docs/reference/`
- Architecture → `docs/architecture/`

**Root Files (Allowed ONLY):**
- README.md, MASTER_TODO.md, NEXT_STEPS.md, CLAUDE.md
- Build/config files (.gitignore, etc.)

**NEVER:**
- Create files in project root not on the allowed list
- Put session notes anywhere except `sessions/`
- Commit `.claude/settings.local.json`

---

### Session End Protocol — MANDATORY

**Required at EVERY session end:**
1. Update `NEXT_STEPS.md`
2. Update `MASTER_TODO.md` (if significant work completed)
3. Create session summary: `sessions/recent/YYYY-MM-DD_description.md`
4. Push all commits
5. Verify `git status` is clean

---

**Version:** v1.0
**Last Updated:** 2026-02-27
**Project:** Pivot
**Repo:** `C:\Users\Chris Kremer\Documents\GitHub\Pivot`
