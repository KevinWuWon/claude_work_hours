---
name: timeline_claude
description: Generate weekly HTML timelines from Claude Code session logs in ~/.claude/projects. Use when user asks "what did I work on this week", wants to fill out a timesheet, needs hours-per-project breakdown, or asks to visualize/summarize Claude session activity. Groups by project (collapsing worktrees), shows half-hour grid in local timezone, distinguishes hands-on time from autonomous Claude runs.
---

# Claude Timeline Generator

Generates a self-contained weekly HTML timeline from Claude Code JSONL logs in `~/.claude/projects/`, grouped by project in a half-hour grid. It distinguishes hands-on user-message time from autonomous Claude-only activity and uses `codex exec --model gpt-5.4-nano` for session summaries.

## Usage

Run from the user's current directory (output goes there unless `--out` is set):

**Current week (default):**
```bash
command uv run scripts/generate.py
```

**Specific week (any date in that ISO week):**
```bash
command uv run scripts/generate.py --week 2026-05-08
```

**Last week:**
```bash
command uv run scripts/generate.py --week last
```

**Arbitrary date range:**
```bash
command uv run scripts/generate.py --from 2026-04-27 --to 2026-05-10
```

**Last N weeks (emits sibling files with prev/next links):**
```bash
command uv run scripts/generate.py --last-4-weeks
```

**Options:**
- `--tz TIMEZONE` — local timezone (default: `Australia/Sydney`).
- `--out PATH` — output HTML path (default: `./timeline-claude-YYYY-Www.html`).
- `--no-cache` — bypass the project-resolver + summary caches.
- `--no-summarize` — skip LLM session summarization (fall back to first user message).
- `--summary-workers N` — parallel `codex exec` workers (default 4).
- `--open` — open the result in the default browser after writing.

## Preflight

- `command -v uv` (required).
- `~/.claude/projects/` exists (created automatically by Claude Code on first use).
