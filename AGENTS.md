# Agent Instructions

See [AGENT_INSTRUCTIONS.md](AGENT_INSTRUCTIONS.md) for full instructions.

This file exists for compatibility with tools that look for AGENTS.md.

## Key Sections

- **Issue Tracking** - How to use bd for work management
- **Development Guidelines** - Code standards and testing
- **Visual Design System** - Status icons, colors, and semantic styling for CLI output

## Visual Design Anti-Patterns

**NEVER use emoji-style icons** (🔴🟠🟡🔵⚪) in CLI output. They cause cognitive overload.

**ALWAYS use small Unicode symbols** with semantic colors:
- Status: `○ ◐ ● ✓ ❄`
- Priority: `● P0` (filled circle with color)

See [AGENT_INSTRUCTIONS.md](AGENT_INSTRUCTIONS.md) for full development guidelines.

## Agent Warning: Interactive Commands

**DO NOT use `bd edit`** - it opens an interactive editor ($EDITOR) which AI agents cannot use.

Use `bd update` with flags instead:
```bash
bd update <id> --description "new description"
bd update <id> --title "new title"
bd update <id> --design "design notes"
bd update <id> --notes "additional notes"
bd update <id> --acceptance "acceptance criteria"
```

## Search Cheat Sheet

Use `bd search` for full-text search across title/description/ID. Use `bd list` for field-specific filters.

```bash
# Full-text search
bd search "auth"
bd search "RFC 6749" --status open --limit 20
bd search "migration" --updated-after 2025-01-01 --sort updated --reverse

# Field-specific filters
bd list --title-contains "auth"
bd list --desc-contains "decision"
bd list --notes-contains "TODO"

# Labels (AND / OR)
bd list --label bug,critical
bd list --label-any frontend,backend

# Priority and time ranges
bd list --priority-min 0 --priority-max 1
bd list --created-after 2024-01-01 --closed-before 2024-12-31
```

## Landing the Plane (Session Completion)

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
