# File Organization Directive — Pivot

**Purpose:** Defines where files belong in this repository
**Version:** v1.0
**Enforcement:** STRICTLY ENFORCED

---

## Directory Structure

```
Pivot/
├── .claude/          ← Claude Code directives and templates
├── .github/          ← GitHub workflows and issue templates
│   ├── workflows/    ← GitHub Actions CI/CD
│   └── ISSUE_TEMPLATE/
├── data/             ← Data files, imports, exports, seeds
├── docs/             ← All project documentation
│   ├── architecture/ ← System design, diagrams, decisions
│   ├── guides/       ← How-to guides, tutorials
│   ├── reference/    ← API docs, schemas, specs
│   ├── reports/      ← Analysis reports, findings
│   └── setup/        ← Installation and environment setup
├── scripts/          ← Utility scripts, automation, CI helpers
├── sessions/         ← Session notes (DO NOT commit frequently)
│   ├── recent/       ← Last 10 session summaries
│   └── archive/      ← Archived by YYYY-MM/
└── src/              ← All application source code
```

---

## File Placement Rules

### Root Files (Allowed ONLY)
- `README.md`
- `MASTER_TODO.md`
- `NEXT_STEPS.md`
- `CLAUDE.md`
- `.gitignore`
- `.editorconfig`
- `LICENSE`
- `CHANGELOG.md`
- Build/config files (package.json, etc.)

**NEVER create other files in root without checking this list.**

### Documentation (`docs/`)
- Reports → `docs/reports/`
- Setup guides → `docs/setup/`
- How-to guides → `docs/guides/`
- Reference material → `docs/reference/`
- Architecture decisions → `docs/architecture/`

### Session Notes (`sessions/`)
- Pattern: `sessions/recent/YYYY-MM-DD_brief-description.md`
- After 10 notes accumulate in `recent/`, archive oldest to `sessions/archive/YYYY-MM/`
- **NEVER** put session notes in root or docs/

### Source Code (`src/`)
- All application code lives here
- Organize by feature or layer within `src/`

### Data Files (`data/`)
- Input/output data files
- Seed data
- Import/export files
- **NEVER** commit sensitive data files

### Scripts (`scripts/`)
- Automation scripts
- Build helpers
- One-off utility scripts

---

## Naming Conventions

| Item | Convention | Example |
|------|-----------|---------|
| Session notes | `YYYY-MM-DD_description.md` | `2026-02-27_initial-setup.md` |
| Directories | `lowercase-kebab` | `user-auth/` |
| Config files | `lowercase` | `.env.example` |

---

## NEVER
- Create files in root not on the allowed list
- Put session notes anywhere except `sessions/`
- Use wrong date format (must be YYYY-MM-DD)
- Commit `.claude/settings.local.json`
- Commit `.env` or credential files

---

**Version:** v1.0 | **Project:** Pivot
