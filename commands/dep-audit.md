Audit every dependency in the current repository. Find vulnerable versions, outdated packages with available upgrades, and unused dependencies that can be removed entirely.

Supports: `package.json` (npm/yarn/pnpm) · `requirements.txt` / `pyproject.toml` (pip) · `go.mod` (Go) · `Gemfile` (Ruby) · `Cargo.toml` (Rust).

---

## Phase 1 — Gather

Run the following in parallel:

1. **Vulnerability scan** — run the ecosystem's native audit tool:
   - npm: `npm audit --json`
   - pip: `pip-audit --format json` (install if missing: `pip install pip-audit`)
   - Go: `govulncheck ./...` (install if missing: `go install golang.org/x/vuln/cmd/govulncheck@latest`)
   - Cargo: `cargo audit --json`

2. **Outdated check** — list packages with available upgrades:
   - npm: `npm outdated --json`
   - pip: `pip list --outdated --format json`
   - Go: `go list -u -m all`
   - Cargo: `cargo outdated`

3. **Usage scan** — for each listed dependency, grep `src/` (and root-level source files) for actual import/require/use. Flag any dependency that has **zero usages** in source code.

---

## Phase 2 — Report

---

### 📦 Dependency Audit — `<repo-name>`

**Package manager:** `<npm / pip / go / cargo>`  
**Total dependencies:** N direct · N transitive (if available)

---

#### Summary

| Category | Count |
|----------|-------|
| 🔴 Critical / High CVEs | — |
| 🟠 Moderate CVEs | — |
| ⬆️ Outdated (patch available) | — |
| ⬆️ Outdated (minor available) | — |
| ⬆️ Outdated (major available) | — |
| 🗑️ Unused (safe to remove) | — |

---

#### 🔴 Vulnerabilities

For each CVE, ordered by severity:

```
**[V-N] 🔴 CRITICAL / 🟠 HIGH / 🟡 MODERATE — <Package>@<current-version>**
CVE: CVE-YYYY-XXXXX
Issue: One sentence describing the vulnerability.
Affected versions: <= X.Y.Z
Fix: Upgrade to <package>@<safe-version>
Command: npm install <package>@<safe-version>
```

---

#### ⬆️ Outdated Packages

Group into three tables:

**Patch updates** *(safe, apply immediately)*

| Package | Current | Latest | Command |
|---------|---------|--------|---------|
| `lodash` | `4.17.20` | `4.17.21` | `npm i lodash@4.17.21` |

**Minor updates** *(usually safe — check changelog)*

| Package | Current | Latest | Notable changes |
|---------|---------|--------|-----------------|

**Major updates** *(breaking changes likely — review before upgrading)*

| Package | Current | Latest | Key breaking change |
|---------|---------|--------|---------------------|

---

#### 🗑️ Unused Dependencies

| Package | In package.json | Found in source | Action |
|---------|----------------|-----------------|--------|
| `moment` | ✅ | ❌ | `npm uninstall moment` |

---

#### 🛠️ Bulk Fix Commands

```bash
# Fix all vulnerabilities (non-breaking)
<command>

# Apply all patch updates
<command>

# Remove all unused packages
<command>
```

---

## Phase 3 — Ask

After the report, say:

> What do you want to action? Options: **"fix all CVEs"**, **"apply patch updates"**, **"remove unused deps"**, **"upgrade everything"**, or **"just the report"**.

When actioning upgrades: apply them, run `npm run build` (or equivalent) to catch breakage, run tests, then summarise what changed. For major version bumps, check the package's CHANGELOG for breaking changes before upgrading.
