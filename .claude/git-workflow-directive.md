# Git Workflow Directive — Pivot

**Purpose:** Enforce safe git operations and prevent destructive actions
**Version:** v1.0
**Enforcement:** STRICTLY ENFORCED

---

## Protected Branches - NO COMMITS

**NEVER commit directly to these branches without EXPLICIT user confirmation:**

### Exact Match (Case-Insensitive)
- `main`
- `master`
- `dev`
- `develop`

### Contains Pattern (Case-Insensitive)
- Anything containing: `beta`, `stage`, `staging`, `production`, `prod`, `deploy`, `release`

---

## Safe Branches - OK to Commit

- `feature/*` - Feature branches
- `fix/*`, `bugfix/*`, `hotfix/*` - Bug fix branches
- `Kremer-dev` - Primary developer branch
- `[username]-*` - User-specific branches

---

## Branch Safety Check (MANDATORY at Startup)

```bash
git branch --show-current
```

**If on Protected Branch:**
1. STOP — Do NOT proceed with commits
2. WARN USER — Display clear warning
3. CREATE NEW BRANCH — Immediately

```bash
git checkout -b feature/[FEATURE_NAME]
# OR stay on Kremer-dev
git checkout Kremer-dev
```

---

## Commit Standards (MANDATORY)

**Required format:** `type(scope): description`

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation only
- `style` - Formatting
- `refactor` - Code restructuring
- `perf` - Performance improvement
- `test` - Adding/updating tests
- `chore` - Maintenance tasks
- `ci` - CI/CD changes

**Requirements:**
- Minimum 10 characters
- Describe WHY, not just WHAT

---

## Prohibited Operations

**NEVER do these without EXPLICIT user confirmation:**

- Force push (`git push --force` / `git push -f`)
- Commit to protected branches
- Delete remote branches (`git push origin --delete branch-name`)
- Amend pushed commits (`git commit --amend` after push)
- Add AI co-author attribution

---

## Required Operations

**ALWAYS do these:**

1. Sync before starting work:
   ```bash
   git fetch origin
   git pull origin $(git branch --show-current)
   ```

2. Use conventional commits:
   ```bash
   git commit -m "type(scope): description"
   ```

3. Push all commits at session end:
   ```bash
   git push origin $(git branch --show-current)
   ```

4. Verify git status:
   ```bash
   git status
   ```

---

## Standard Workflow

### Starting Work
1. `git branch --show-current` — verify safe branch
2. `git fetch origin && git pull origin Kremer-dev`

### During Work
3. Make changes
4. `git add [files]`
5. `git commit -m "type(scope): description"`

### Ending Work
6. `git status` — verify clean
7. `git push origin Kremer-dev`

---

**Version:** v1.0 | **Project:** Pivot
