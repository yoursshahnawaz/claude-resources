You are running a security and performance audit on the current repository. Work through it systematically, present all findings in a single well-formatted report, then ask the user what to fix.

---

## Phase 1 — Explore

Launch **2–3 parallel Explore agents** based on what the repo contains. Split by layer:

- **Agent 1 — Entry & Config**: `package.json` / `go.mod` / `requirements.txt`, server entry point (`app.js`, `main.go`, `server.py`, etc.), environment config, middleware setup, CORS, auth middleware, rate limiting, security headers.
- **Agent 2 — Data & Business Logic**: routes, controllers, services, database queries/models, input validation, error handling, file uploads.
- **Agent 3 — Frontend** *(only if a frontend exists)*: API client setup, auth token storage, component rendering of user content, build config (`next.config.js`, `vite.config.ts`), bundle size concerns.

Each agent must return **actual findings with file paths and line numbers** — no generic advice.

---

## Phase 2 — Report

After all agents return, synthesize into this exact format:

---

### 🔍 Audit Report — `<repo-name>`

**Stack detected:** `<e.g. Node.js / Express / Firestore / Next.js>`

#### Summary

| Severity | 🔐 Security | ⚡ Performance |
|----------|-------------|----------------|
| 🔴 Critical | — | — |
| 🟠 High | — | — |
| 🟡 Medium | — | — |
| 🔵 Low | — | — |

---

#### 🔐 Security Findings

For each finding use this block — ordered **Critical → Low**:

```
**[S-N] 🔴 CRITICAL / 🟠 HIGH / 🟡 MEDIUM / 🔵 LOW — <Short Title>**
File: `src/path/to/file.js:42`
Issue: One sentence describing the exact problem found in the code.
Risk: What an attacker can do with this.
Fix: Concrete code snippet or specific change required.
```

---

#### ⚡ Performance Findings

Same block format — ordered **High → Low**:

```
**[P-N] 🟠 HIGH / 🟡 MEDIUM / 🔵 LOW — <Short Title>**
File: `src/path/to/file.js:42`
Issue: One sentence describing the bottleneck found in the code.
Impact: Effect on latency / memory / cost.
Fix: Concrete code snippet or specific change required.
```

---

#### 🚨 Fix First

List the top 3 highest-impact issues (mix of security + perf) with a one-line reason each.

---

## Phase 3 — Ask

After presenting the report, say exactly this:

> Which findings do you want fixed? You can say **"fix S-1 and S-3"**, **"fix all critical"**, **"fix everything"**, or **"just show me — I'll fix manually"**.

When the user responds, apply fixes surgically (one finding at a time), verify the app still builds/runs, then summarise what changed.

---

## What to look for

### Security checklist
- Secrets / API keys hardcoded in source or committed `.env` files
- Missing security headers (`helmet` or equivalent)
- No rate limiting on auth or write endpoints
- Body size limit absent (DoS via oversized payloads)
- User input used directly in DB queries / field names (injection)
- Error messages exposing stack traces or DB schema to clients
- Auth tokens stored in `localStorage` instead of `httpOnly` cookies
- `dangerouslySetInnerHTML`, `rehype-raw`, or `v-html` without sanitization
- Wildcard CORS or image remote patterns
- JWT with weak / hardcoded secrets
- Missing input validation before DB writes
- Sensitive data in `console.log` / `console.error`

### Performance checklist
- Sequential DB reads that could be `Promise.all` / goroutine / async
- Full collection scans to compute counts (should be aggregates)
- No caching on expensive or repeated reads
- `uuidv4()` or equivalent called on every read response
- No compression middleware (`gzip` / `brotli`)
- Offset-based pagination (`LIMIT x OFFSET y`) on large tables
- `localStorage.getItem` called on every network request
- Duplicate `useEffect` / lifecycle hooks fetching the same data
- Heavy libraries imported synchronously at module load
- Wildcard image hosts in Next.js / Nuxt config
- No request cancellation (`AbortController`) on navigation
