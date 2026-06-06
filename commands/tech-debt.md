Scan the current repository for technical debt. Use parallel Explore agents to cover the full codebase, then produce a prioritized backlog the team can act on.

---

## Phase 1 — Explore

Launch **2–3 parallel Explore agents** split by concern:

- **Agent 1 — Dead & Orphaned Code**: unused imports, unexported functions never called, commented-out code blocks, unreachable branches (`if false`, code after `return`), variables assigned but never read.
- **Agent 2 — Duplication & Complexity**: copy-pasted logic across files, functions over 40 lines, deeply nested conditionals (3+ levels), magic numbers/strings, large switch/if-else chains that should be data-driven.
- **Agent 3 — Signals & Markers**: every `TODO`, `FIXME`, `HACK`, `XXX`, `TEMP` comment with its file and line; deprecated API usage; `any` types in TypeScript; `eslint-disable` suppressions.

Each agent must return **actual findings with file paths and line numbers**.

---

## Phase 2 — Report

Synthesize into this format:

---

### 🏗️ Tech Debt Report — `<repo-name>`

#### Summary

| Category | Items | Est. Total Effort |
|----------|-------|-------------------|
| 🗑️ Dead Code | — | — |
| 🔁 Duplication | — | — |
| 🌀 Complexity | — | — |
| 📌 Markers (TODO/FIXME) | — | — |

Effort scale: **S** = < 30 min · **M** = 30 min–2 hr · **L** = 2–8 hr · **XL** = multiple days

---

#### 🗑️ Dead Code

For each finding:

```
**[D-N] <File>:<line> — <Short Title>**
Effort: S / M / L
What: One sentence describing the dead code.
Action: Delete `functionName` — nothing imports it.
```

---

#### 🔁 Duplication

```
**[R-N] <Title>**
Effort: S / M / L
Files: `src/a.js:12` and `src/b.js:34`
What: X lines of near-identical logic.
Action: Extract to `src/utils/<name>.js` and import in both places.
```

---

#### 🌀 Complexity

```
**[C-N] <File>:<line> — <Function / Block Name>**
Effort: M / L / XL
What: Why this is hard to maintain (e.g. 6-level nesting, 80-line function, 12-branch switch).
Action: Concrete restructuring approach.
```

---

#### 📌 Markers & Suppressions

List all TODO / FIXME / HACK / eslint-disable occurrences as a table:

| ID | File | Line | Type | Comment |
|----|------|------|------|---------|
| M-1 | `src/...` | 42 | TODO | "..." |

---

#### 🎯 Suggested Sprint Order

Top 5 highest-ROI items — prioritised by (impact × effort⁻¹):

1. **[ID]** — reason
2. …

---

## Phase 3 — Ask

After the report, say:

> Which items do you want addressed? You can say **"clean up all dead code"**, **"fix D-1 and R-2"**, **"tackle everything S-sized"**, or **"just the report — I'll handle it"**.

Apply fixes surgically. After each change verify the app still builds and tests pass, then summarise what was removed or refactored.
