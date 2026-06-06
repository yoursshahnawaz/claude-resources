Simplify the file at `$ARGUMENTS`. Reduce it to the minimum code that preserves all existing behaviour — no over-engineering, no unused flexibility, no indirection that exists only for its own sake.

If no path is provided, ask: "Which file should I simplify?"

---

## Phase 1 — Read & Understand

1. Read the full file.
2. Identify every public function / export and what callers expect from it (grep the codebase for usages).
3. Note the test coverage — read any test files that exercise this file so behaviour constraints are clear.

---

## Phase 2 — Analyse

Look for these patterns and flag each one found:

| Pattern | Example |
|---------|---------|
| **Wrapper with no logic** | A function that only calls another function with the same args |
| **Premature abstraction** | A class / interface used in exactly one place |
| **Config object for one caller** | An options object where only one field is ever set |
| **Unnecessary indirection** | `getX()` → `fetchX()` → `loadX()` → actual DB call |
| **Dead parameters** | Function params that are never read inside the body |
| **Redundant state** | A variable that can be derived from another at read time |
| **Over-generalised logic** | A loop / map that handles N cases but only 1 ever occurs |
| **Trivial helpers** | One-liner utils that inline cleanly at call site |

---

## Phase 3 — Rewrite Plan

Before touching anything, present a **diff summary**:

---

### ✂️ Simplification Plan — `<filename>`

**Current:** ~N lines · **After:** ~N lines · **Reduction:** ~N%

| # | What gets removed / inlined | Reason |
|---|----------------------------|--------|
| 1 | `wrapperFunction()` — just proxies `realFunction()` | Zero added value |
| 2 | `OptionsConfig` interface — only one field used by one caller | Premature abstraction |
| … | | |

**Behaviour preserved:** List every public export and confirm it keeps the same signature and return value.

**Tests needed:** Note if any test needs updating to reflect the simpler API.

---

Then ask:

> "Does this plan look right? I'll apply it now unless you want to exclude anything."

---

## Phase 4 — Apply

1. Rewrite the file — do not change logic, only structure.
2. Run the build (`npm run build`, `go build`, `python -m py_compile`, etc.) — fix any errors before moving on.
3. Run tests if a test command exists — all must pass.
4. Show a final summary:

---

### ✅ Done — `<filename>`

- **Removed:** list of deleted functions / types / params
- **Inlined:** list of helpers merged into call sites
- **Lines:** N → N (−N%)
- **Tests:** passing / updated
