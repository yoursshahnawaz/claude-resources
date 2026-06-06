# claude-resources

A collection of Claude Code custom slash commands — drop any file from `commands/` into your project's `.claude/commands/` folder to activate it as a `/command-name` in your Claude Code session.

## Commands

| Command | File | What it does |
|---------|------|--------------|
| `/repo-audit` | `commands/repo-audit.md` | Full security + performance audit of the current repo — produces a prioritized findings report and offers to apply fixes interactively |

## Usage

```bash
# Copy a command into your project
cp commands/repo-audit.md your-project/.claude/commands/repo-audit.md

# Then in Claude Code
/repo-audit
```

Claude will explore the codebase with parallel agents, return a structured report grouped by severity, then ask which findings you want fixed.
