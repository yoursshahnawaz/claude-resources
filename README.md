# claude-resources

A collection of Claude Code custom slash commands — drop any file from `commands/` into your project's `.claude/commands/` folder to activate it as a `/command-name` in your Claude Code session.

## Commands

| Command | File | What it does |
|---------|------|--------------|
| `/repo-audit` | `commands/repo-audit.md` | Full security + performance audit — prioritized findings report, then interactive fix prompt |
| `/tech-debt` | `commands/tech-debt.md` | Scans for dead code, duplication, complexity, and TODO markers — produces a prioritized backlog with effort estimates |
| `/simplify-file` | `commands/simplify-file.md` | Targets a single file, identifies over-engineering, presents a rewrite plan, then applies it |
| `/dep-audit` | `commands/dep-audit.md` | Checks dependencies for CVEs, outdated versions, and unused packages — generates upgrade commands |

## Usage

```bash
# Copy one or all commands into your project
cp commands/*.md your-project/.claude/commands/

# Then in Claude Code
/repo-audit
/tech-debt
/simplify-file src/utils/helper.ts
/dep-audit
```

Each command explores the codebase, returns a structured report, then asks what you want to action.
